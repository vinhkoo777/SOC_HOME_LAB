# Kerberoasting Active Directory

**MITRE ATT&CK:** T1558.003 - Steal or Forge Kerberos Tickets: Kerberoasting  
**Tool:** python3-impacket  
**Target:**  Windows AD (192.168.188.30)  

## 1. Attack Scenario

Trong kịch bản này, attacker sử dụng tài khoản domain hợp lệ để enumerate các service account có SPN (Service Principal Name) trong Active Directory. Sau đó attacker yêu cầu Kerberos TGS (Ticket Granting Service) tickets từ Domain Controller nhằm trích xuất Kerberos ticket hashes phục vụ cho quá trình offline password cracking.

```bash
impacket-GetUserSPNs siem.lab/ALEXANDRA_PUGH:'Password123!' -dc-ip 192.168.188.30 -request
```
**Trong đó**

`siem.lab/john:'Password123!'`

- Sử dụng tài khoản domain john để authenticate vào Active Directory.

`-dc-ip 192.168.188.30`

- Chỉ định Domain Controller mục tiêu.

`-request`

- Yêu cầu Kerberos TGS tickets cho các service account có SPN và xuất Kerberos ticket hashes phục vụ Kerberoasting.

Và attacker đã thực hiện thành công. Khi này Domain Controller sinh ra nhiều sự kiện Event ID 4769 từ địa chỉ IP `192.168.188.20` tức là máy attacker.

<img width="1166" height="811" alt="image" src="https://github.com/user-attachments/assets/37fa6717-7617-454e-b13e-34cbf4c5d12b" />

## 2. Detection Rule (SPL)

```spl
index=* host="DC01" "<EventID>4769</EventID>" 
| rex field=_raw "<Data Name='IpAddress'>::ffff:(?<src_ip>\d+\.\d+\.\d+\.\d+)</Data>"
| rex field=_raw "<Data Name='ServiceName'>(?<user>[a-zA-Z0-9._-]+)</Data>"
| rex field=_raw "<Data Name='TicketEncryptionType'>0x(?<type>[0-9A-Fa-f]+)</Data>"
| stats count by src_ip user TicketEncryptionType
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

<img width="1905" height="790" alt="image" src="https://github.com/user-attachments/assets/98dc6f68-87ef-425c-a763-22c7ae32abc0" />

