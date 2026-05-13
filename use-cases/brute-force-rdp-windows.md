# Brute Force RDP 

**MITRE ATT&CK:** T1110.003: Password Spraying  
**Tool:** Hydra  
**Target:** Windows Client (192.168.188.40)  

## 1. Attack Scenario

Attacker sử dụng hydra brute force dịch vụ RDP trên 1 host dưới đây là windows client. Với username là alex và list password đã chuẩn bị sẳng.

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

Thì như ta thấy trong hình dưới. Đây là EventID 4625 nghĩa là khi có lượt đăng nhập thất bại thì Event này sẽ được gọi lên. Ta cũng thấy nó cũng chứa IP là `192.168.188.20` thì đây là IP của máy kali của ta.

<img width="1573" height="800" alt="image" src="https://github.com/user-attachments/assets/b9c0dc4a-9d5a-43f5-a8ab-b2d3ea0257bf" />

Tiếp theo đó tôi sẽ Query EventID 4624 với logon type là 10. Thì ý nghĩa là lượt đăng nhập thành công trong RDP. Mục đích của tôi là xem liệu attacker có đăng nhập thành công hay chưa. Thì như trong hình ta đã thấy IP của máy kali nghĩa là attacker đã đăng nhập thành công.

<img width="1916" height="938" alt="image" src="https://github.com/user-attachments/assets/7d5b41b2-b421-4aae-b2db-98eaa79288d4" />

## 4. Dashboard

<img width="1909" height="719" alt="image" src="https://github.com/user-attachments/assets/3257993e-fafa-4b2f-8d11-76fa7f3abfb6" />

Trên đây là Dashboard theo dỗi số lần đăng nhập thành công và thất bại. Cũng như là khoảng thời gian của các lần đăng nhập thất bại.
