# Kerberoasting Active Directory

**MITRE ATT&CK:** T1558.003 - Steal or Forge Kerberos Tickets: Kerberoasting  
**Tool:** python3-impacket
**Target:**  Windows AD (192.168.188.30)

## 1. Attack Scenario

Giả sử kẻ tấn công thực hiện đoạn lệnh dưới. Giải thích lệnh là nó sẽ dùng tài khoản john đăng nhập vào domain corp.local, tìm các service account có SPN, rồi yêu cầu Kerberos service tickets từ DC, và xuất hash để phục vụ Kerberoasting attack.

```bash
impacket-GetUserSPNs siem.lab/john:'Password123!' -dc-ip 192.168.188.30 -request
```

## 2. Detection Rule (SPL)

```spl
index=* host="ubuntu" source="/var/log/auth.log" "Failed password"
| rex field=_raw "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| rex field=_raw "for (?<user>[a-zA-Z0-9._-]+)"
| stats count by src_ip user
| Where count > 5
| sort - count
```
**Alert condition:** > 5 failed attempts trong vòng 1 phút từ cùng 1 IP

## 3. Log Evidence


## 4. Dashboard


## 5. Response
