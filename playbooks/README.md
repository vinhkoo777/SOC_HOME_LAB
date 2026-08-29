# Incident Response Playbooks

Các playbook trong thư mục này tui dựa theo **NIST SP 800-61 Rev.3**. Phần xử lý incident tập trung vào **Detect > Respond > Recover**, tiếp đó ghi nhận **Improvement / Lessons Learned**.

- **Detect:** xác minh detection/event, false positive và phạm vi ban đầu.
- **Respond:** triage, điều tra, containment, eradication và escalation.
- **Recover:** khôi phục và theo dõi sau xử lý.
- **Improve:** ghi kết luận, lessons learned và tuning detection.

## Playbook hiện có

| Playbook | Detection |
|---|---|
| [Brute Force](brute-force-response.md) | SSH/RDP Brute Force |
| [Suspicious PowerShell](suspicious-powershell-response.md) | Suspicious PowerShell Execution |
| [AS-REP Roasting](as-rep-roasting-response.md) | Possible AS-REP Roasting |
