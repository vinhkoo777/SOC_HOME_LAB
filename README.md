# SOC Home Lab

> Made by **K0g4** with love <3333

Project thực hành SOC/Blue Team trong môi trường lab, sử dụng Splunk để thu thập và phân tích log, xây dựng detection rule, mô phỏng các kỹ thuật tấn công và thực hành điều tra, xây dựng playbook.

## Mô hình lab

| Máy | IP | Vai trò |
|---|---|---|
| pfSense | `192.168.188.2` | Gateway, Firewall |
| Splunk | `192.168.188.10` | SIEM |
| Kali Linux | `192.168.188.20` | Máy mô phỏng tấn công |
| Domain Controller | `192.168.188.30` | Active Directory |
| Windows Client | `192.168.188.40` | Windows Endpoint |
| Linux Client | `192.168.188.50` | Linux Endpoint |

Mạng lab sử dụng VMware Host-Only `192.168.188.0/24`

## Detection hiện có

Các SPL rule nằm trong [`detections/`](detections/README.md).

| Detection | MITRE ATT&CK | 
|---|---|
| SSH Brute Force | T1110.001 |
| RDP Brute Force | T1110.001 |
| Suspicious PowerShell | T1059.001 |
| AS-REP Roasting | T1558.004 | 

## Use Case

| Use Case | Mô tả | 
|---|---|
| [SSH Brute Force trên Linux](use-cases/brute-force-ssh-linux.md) | Phát hiện nhiều lần đăng nhập SSH thất bại |
| [RDP Brute Force trên Windows](use-cases/brute-force-rdp-windows.md) | Phát hiện brute force RDP bằng Windows Event Log |
| [Suspicious PowerShell](use-cases/suspicious-powershell.md) | Phát hiện PowerShell có dấu hiệu đáng ngờ |
| [AS-REP Roasting](use-cases/as-rep-roasting.md) | Hunt yêu cầu TGT bất thường trên Domain Controller |


## Incident Response Playbook

Các playbook sử dụng cùng một cấu trúc xử lý. Xem [playbook](playbooks/README.md).

| Playbook | Nội dung |
|---|---|
| [Brute Force](playbooks/brute-force-response.md) | Điều tra và xử lý brute force |
| [Suspicious PowerShell](playbooks/suspicious-powershell-response.md) | Điều tra PowerShell đáng ngờ |
| [AS-REP Roasting](playbooks/as-rep-roasting-response.md) | Điều tra AS-REP Roasting |

## Cài đặt lab

1. [pfSense](setup/PHẦN1-pfsense.md)
2. [Splunk SIEM](setup/PHẦN2-siem-setup.md)
3. [Active Directory](setup/PHẦN3-active-directory.md)
4. [Kali Linux](setup/PHẦN4-kali-linux.md)
5. [Linux Client](setup/PHẦN5-linux-client.md)
6. [Windows Client](setup/PHẦN6-windows-client.md)
7. [Domain And Log](setup/PHẦN7-Domain-And-Log.md)
