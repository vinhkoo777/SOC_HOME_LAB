# Suspicious PowerShell Response

## Tổng quan

| Thuộc tính | Giá trị |
|---|---|
| Severity | Medium / High |
| MITRE ATT&CK | T1059.001 - PowerShell |
| Detection | [suspicious_powershell](../detections/windows/suspicious_powershell) |

## 1. Detect

- Xác định keyword/indicator khiến detection match.
- Mở raw Sysmon Event ID 1 để kiểm tra process, user, host, command line và thời gian.
- Xác minh đây có phải script quản trị hoặc automation hợp lệ không.
- Xác định activity chỉ xuất hiện trên một host hay nhiều host.

## 2. Respond

### Investigation

- Kiểm tra parent/child process của PowerShell.
- Tìm các Sysmon Event ID 1 liên quan trong cùng timeline.
- Kiểm tra network connection và file/process được tạo liên quan.
- Kiểm tra persistence như Task Scheduler hoặc WMI nếu có dấu hiệu phù hợp.
- Lưu command/script, process và timeline liên quan.

### Containment & Eradication

Nếu xác nhận malicious:

- Isolate endpoint.
- Disable account nếu nghi credential bị compromise.
- Block IP/domain liên quan nếu cần.
- Xóa script, file hoặc persistence liên quan và reset credential khi cần.

### Escalation

Nâng mức độ nếu có download/dynamic execution, persistence, credential access, lateral movement hoặc activity trên nhiều host.

## 3. Recover

- Khôi phục endpoint về trạng thái an toàn và đưa trở lại mạng khi đã xác nhận sạch.
- Theo dõi Sysmon và các log endpoint/network liên quan sau xử lý.

## 4. Improve / Lessons Learned

- Ghi kết luận True Positive / False Positive và action đã thực hiện.
- Lưu evidence chính: Sysmon Event ID 1, command line, user, host, process và network activity liên quan.
- Tuning detection hoặc whitelist script/path/hash đã xác minh nếu cần.
