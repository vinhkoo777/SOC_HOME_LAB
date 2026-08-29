# AS-REP Roasting

**MITRE ATT&CK:** T1558.004 - AS-REP Roasting  
**Tool:** Impacket `GetNPUsers.py`  
**EventID:** 4768  
**Target:** Domain Controller `192.168.188.30`

## 1. Attack Scenario

AS-REP Roasting nhắm vào tài khoản Active Directory đã tắt Kerberos pre-authentication. Domain trong lab được populate bằng **BadBlood**, nên thay vì tạo riêng account test, audit domain hiện có để tìm account đã sẵn bị tắt pre-auth:

```
Get-ADUser -LDAPFilter "(userAccountControl:1.2.840.113556.1.4.803:=4194304)" -Properties userAccountControl |
    Select-Object SamAccountName,userAccountControl
```

Chọn 1 account thường trong kết quả làm target, ví dụ `LORAINE_CASTILLO`.

Account chọn được có thể bị BadBlood set password hết hạn (`PasswordExpired=True`), khiến KDC trả `KDC_ERR_KEY_EXPIRED` thay vì AS-REP. Reset password trước khi test:

```
Set-ADAccountPassword -Identity "LORAINE_CASTILLO" -Reset -NewPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force)
Set-ADUser -Identity "LORAINE_CASTILLO" -ChangePasswordAtLogon $false
```

Trước khi mô phỏng attack, kiểm tra Splunk đã nhận Event ID 4768 từ Domain Controller:

```
index=main source="WinEventLog:Security" "<EventID>4768</EventID>"
```

<img width="1632" height="889" alt="image" src="https://github.com/user-attachments/assets/3c5b9e30-b5d8-4adb-bcd7-1d1f707fe55b" />

Từ Kali, tạo `users.txt` chứa account đã chọn, dùng Impacket `GetNPUsers.py` để request AS-REP:

```
echo "LORAINE_CASTILLO" > users.txt
python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py siem.lab/ -dc-ip 192.168.188.30 -usersfile users.txt -no-pass -format hashcat
```

Và dưới đây là kết quả

<img width="1395" height="200" alt="image" src="https://github.com/user-attachments/assets/be786f3b-effb-44f6-82ee-e7ab204ffbbe" />

## 2. Detection Rule (SPL)

```
index=main source="WinEventLog:Security" "<EventID>4768</EventID>"
| rex field=_raw "<Data Name='TargetUserName'>(?<user>[^<]+)</Data>"
| rex field=_raw "<Data Name='IpAddress'>(?<src_ip>[^<]+)</Data>"
| rex field=_raw "<Data Name='PreAuthType'>(?<preauth_type>[^<]+)</Data>"
| rex field=_raw "<Data Name='TicketEncryptionType'>(?<ticket_encryption_type>[^<]+)</Data>"
| where preauth_type="0" AND ticket_encryption_type="0x17"
| table host src_ip user preauth_type ticket_encryption_type
| sort - _time
```

Và như ta thấy đoạn query trên thực hiện thành công.

<img width="1629" height="549" alt="image" src="https://github.com/user-attachments/assets/94a67bf9-c685-4658-b327-ce547c2c5d64" />

## 3. Log Evidence

Ảnh trên cho thấy Splunk đã ghi nhận **Windows Security Event ID 4768** tại thời điểm thực hiện bài test AS-REP Roasting.

<img width="1605" height="909" alt="image" src="https://github.com/user-attachments/assets/ab168d32-a6ed-4ee8-9a3a-d0e5ec2371e8" />

Các trường quan trọng trong raw event gồm:

- `TargetUserName = ORATNE_CASTILLO`: tài khoản được yêu cầu Kerberos Authentication Ticket.
- `IpAddress = ::ffff:192.168.188.20`: địa chỉ IP của máy gửi request Kerberos.
- `TicketEncryptionType = 0x17`: ticket sử dụng kiểu mã hóa RC4-HMAC.
- `PreAuthType = 0`: request không sử dụng Kerberos pre-authentication.

## 4. Dashboard

<img width="1898" height="957" alt="image" src="https://github.com/user-attachments/assets/6a1dfa2a-1e33-461e-af9f-0060e9046908" />

