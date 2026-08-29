# Brute Force Response

## Tổng quan

| Thuộc tính | Giá trị |
|---|---|
| Phạm vi | SSH trên Linux, RDP trên Windows |
| Severity | High |
| MITRE ATT&CK | T1110.001 - Password Guessing |
| Detection | [SSH](../detections/linux/ssh_brute_force), [RDP](../detections/windows/rdp_brute_force) |

## 1. Detect

- Xác minh số lần failed login, `src_ip`, user, host và thời gian.
- SSH: kiểm tra `/var/log/auth.log` với các dòng `Failed password`.
- RDP: kiểm tra Event ID 4625, Logon Type 10.
- Loại trừ user nhập sai mật khẩu, VPN/jump host, scanner hoặc automation hợp lệ.
- Xác định số account và host bị nhắm tới.

## 2. Respond

### Investigation

- Tìm các lần login khác từ cùng `src_ip`.
- Detection chính chỉ xác định failed login; kiểm tra successful login riêng bằng Windows Event ID 4624 hoặc SSH `Accepted`.
- Nếu có successful login, kiểm tra activity sau đăng nhập bằng Sysmon, PowerShell, `sudo`, cron hoặc network log.
- Lưu timeline và log liên quan.

### Containment & Eradication

Nếu xác nhận malicious:

- Tạm khóa account bị ảnh hưởng.
- Block source IP nếu phù hợp.
- Isolate endpoint nếu có dấu hiệu compromise.
- Reset credential và xử lý persistence/artifact bất thường nếu phát hiện.

### Escalation

Nâng mức độ nếu có successful login, account đặc quyền, nhiều user/host bị ảnh hưởng hoặc có post-compromise activity.

## 3. Recover

- Mở lại account/endpoint sau khi xác nhận an toàn.
- Theo dõi authentication log để phát hiện activity lặp lại.

## 4. Improve / Lessons Learned

- Ghi kết luận True Positive / False Positive và action đã thực hiện.
- Lưu evidence chính: source IP, user, host, failed/successful login và timeline.
- Tuning threshold/whitelist nếu false positive đã được xác minh.
