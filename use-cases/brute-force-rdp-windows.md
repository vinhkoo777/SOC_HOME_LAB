# Brute Force RDP (Windows)

**MITRE ATT&CK:** T1110.003: Password Spraying  
**Tool:** hydra  
**Target:** Windows Client (192.168.188.40)  

## 1. Attack Scenario

Attacker sử dụng hydra brute force dịch vụ RDP trên 1 host dưới đây là windows client. Với username là alex và list password , sử dựng 1 thread.

**Trong đây giả sử attacker đã thu được username**

```bash
sudo hydra -t 1 -V -f -l alex -P '/home/kuga/Desktop/common_pass.txt' rdp://192.168.188.40:3389  
```

Sau khi thực hiện xong lệnh trên Hydra sẽ bắt đầu thử từng mật khẩu cho đến khi kiếm được password khớp với password của user alex.

<img width="1246" height="223" alt="image" src="https://github.com/user-attachments/assets/5340836f-4e59-4d87-8075-f7ee07319150" />


## 2. Detection Rule (SPL)

```spl
index=* host="DESKTOP-GDHTT5E" source="WinEventLog:Security" "<EventID>4625</EventID>"
| rex field=_raw "<Data Name='IpAddress'>(?<src_ip>\d+\.\d+\.\d+\.\d+)</Data>"
| rex field=_raw "<Data Name='TargetUserName'>(?<user>[a-zA-Z0-9._-]+)</Data>"
| stats count by src_ip user
| Where count > 5 
| sort - count
```
**Alert condition:** > 5 failed attempts trong vòng 1 phút từ cùng 1 IP

## 3. Log Evidence


## 4. Dashboard

## 5. Response
1. Block IP `192.168.188.20` trên pfSense Firewall
2. Kiểm tra có login thành công nào không (`Accepted password`)
3. Reset password + bật key-based authentication
4. Review lại SSH config (`/etc/ssh/sshd_config`)
