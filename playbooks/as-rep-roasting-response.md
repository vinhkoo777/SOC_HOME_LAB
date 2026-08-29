# AS-REP Roasting Response

## Tổng quan

| Thuộc tính | Giá trị |
|---|---|
| Severity | High |
| MITRE ATT&CK | T1558.004 - AS-REP Roasting |
| Event chính | Windows Security Event ID 4768 |
| Detection | [as_rep_roasting](../detections/active-directory/as_rep_roasting) |

## 1. Detect

- Mở Event ID 4768 và xác minh user, source IP, `TicketEncryptionType` và `PreAuthType`.
- Kiểm tra account có bật `Do not require Kerberos preauthentication` hay không.
- Xác định source IP có request nhiều account trong thời gian ngắn không.
- Loại trừ activity thực hành hoặc cấu hình hợp lệ đã biết.
- Xác định số account bị ảnh hưởng.

## 2. Respond

### Investigation

- Tìm thêm Event ID 4768 từ cùng source IP.
- Kiểm tra authentication bất thường.
- Kiểm tra activity đáng ngờ khác từ source host nếu có telemetry.
- Lưu Event 4768, user, source IP và timeline liên quan.

### Containment & Eradication

Nếu xác nhận malicious:

- Bật lại Kerberos pre-authentication cho account nếu không có yêu cầu hợp lệ.
- Tạm khóa/reset credential nếu có dấu hiệu credential bị compromise.
- Điều tra hoặc isolate source host nếu cần.
- Kiểm tra các account khác có cùng cấu hình không an toàn.

### Escalation

Nâng mức độ nếu source IP request nhiều account, account có quyền cao hoặc xuất hiện authentication/activity đáng ngờ sau đó.

## 3. Recover

- Mở lại account sau khi xác nhận an toàn nếu đã khóa.
- Xác nhận Kerberos pre-authentication hoạt động bình thường.
- Theo dõi Event ID 4768 sau xử lý.

## 4. Improve / Lessons Learned

- Ghi kết luận True Positive / False Positive và thay đổi đã thực hiện.
- Lưu evidence chính: Event ID 4768, user, source IP, encryption/pre-authentication type và timeline.
- Tuning detection nếu cần nhưng không whitelist rộng account có cấu hình rủi ro.
