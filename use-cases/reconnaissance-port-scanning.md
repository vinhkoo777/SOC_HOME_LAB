# Reconnaissance Port Scanning

**MITRE ATT&CK:** T1046 - Network Service Discovery  
**Tool:** Nmap  
**Target:** Windows Client (192.168.188.40), Linux Client (192.168.188.50)  

## 1. Attack Scenario
 
Attacker sử dụng Nmap để phát hiện các máy chủ, các cổng và dịch vụ đang mở kèm phiên bản của nó. Nhận dạng hệ điều hành...  

Bây giờ giả sử attacker chạy lệnh dưới. Bây giờ ở đây tôi sẽ thực hiện việc scan trên cả 2 máy đó là máy Ubuntu Client và Windows Client

**Ưindows client**

```bash
nmap -sC -sV -F -v 192.168.188.40
```

kết quả trong hình cho ta thấy rằng là Nmap đã scan được port 3389 thì đó là chính là port của **RDP** đang mở. Việc này giúp cho attacker có thể thử brute force để chiếm quền điều khiển máy. Và Nmap cũng cung cấp cho ta các thông tin khác nữa.

<img width="1012" height="781" alt="image" src="https://github.com/user-attachments/assets/1f40b157-524f-412f-b164-f6861e185397" />

**Ubuntu client**

```bash
nmap -sC -sV -F -v 192.168.188.50
```

Kết quả dưới hình này thì cho ta thấy Ubuntu đang mở port 22 thì port đó chính là port của dịch vụ **ssh**. Attacker cũng có thể dùng hydra để brute force thử.

<img width="1095" height="788" alt="image" src="https://github.com/user-attachments/assets/7b043115-0f12-446d-9f5f-64d9fffdaee4" />

## 2. Detection Rule (SPL)

```spl
index=* host="DESKTOP-GDHTT5E" source="WinEventLog:Security" "<EventID>4625</EventID>"
| rex field=_raw "<Data Name='IpAddress'>(?<src_ip>\d+\.\d+\.\d+\.\d+)</Data>"
| rex field=_raw "<Data Name='TargetUserName'>(?<user>[a-zA-Z0-9._-]+)</Data>"
| stats count by src_ip user
| Where count > 5 
| sort - count
```

**Alert condition:** 

## 3. Log Evidence

## 4. Dashboard

## 5. Response
