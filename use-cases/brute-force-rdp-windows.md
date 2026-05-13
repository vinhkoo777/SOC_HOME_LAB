# Brute Force RDP (Windows)

**MITRE ATT&CK:** T1110.003: Password Spraying  
**Tool:** hydra  
**Target:** Windows Client (192.168.188.40)  

## 1. Attack Scenario

Attacker sử dụng hydra brute force dịch vụ RDP trên 1 host dưới đây là windows client. Với username là alex và list password , sử dựng 1 thread.

**Trong đây giả sử attacker đã thu được username**

```bash
sudo hydra -V -f -l alex -P '/home/kuga/rockyou.txt' rdp://192.168.188.40:3389  
```

Sau khi thực hiện xong lệnh trên Hydra sẽ bắt đầu thử từng mật khẩu cho đến khi kiếm được password khớp với password của user alex.

<img width="879" height="537" alt="image" src="https://github.com/user-attachments/assets/724d1604-db6a-411f-9ed7-2fa034dafb0d" />

Và như hình dưới cho ta thấy Hydra đã thành công trong việc tìm thấy được password khớp với password của alex.

<img width="845" height="118" alt="image" src="https://github.com/user-attachments/assets/fdd9861b-ada5-49a7-ba01-2c429489f628" />

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
