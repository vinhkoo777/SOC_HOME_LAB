# Brute Force RDP (Windows)

**MITRE ATT&CK:** T1110.003: Password Spraying  
**Tool:** Crowbar  
**Target:** Windows Client (192.168.188.40)  

## 1. Attack Scenario
Attacker dùng Hydra từ Kali Linux (192.168.188.20) brute-force SSH vào Linux Client, thử nhiều password từ wordlist rockyou.txt. 

> (Với điều kiện là trên Ubuntu Client phải có OpenSSH. Ta qua máy ubuntu client dùng lệnh `ssh -v` để kiểm tra thử nếu chưa có thì sử dụng `sudo apt install openssh-client` để cài đặt)

**Giả sử attacker đã có được username của Ubuntu_Client**

```bash
sudo hydra -l conmeo -P /usr/share/wordlists/rockyou.txt ssh://192.168.188.50  
```

<img width="1602" height="289" alt="image" src="https://github.com/user-attachments/assets/cc284351-0723-476c-9f2c-6f36573abf19" />

Thì như ta thấy kẻ tấn công đã thu được passwotd của user conmeo là `12345678`. Khi này attacker sẻ sử dụng `ssh` để login vào. 

```bash
ssh conmeo@192.168.188.50 
```
Thì như ta thấy attacker đã login thành công. 

<img width="666" height="330" alt="image" src="https://github.com/user-attachments/assets/cb05e1c3-ce77-4063-b934-2c25b998a8c6" />

Và attacker đã chiếm quyền hệ thống bây giờ attacker có thể làm bất cứ thứ gì ví dụ đọc document bí mật tên là `meowwwww.txt`

<img width="787" height="112" alt="image" src="https://github.com/user-attachments/assets/cf35a56b-3847-4504-8859-7b856fb14ec2" />

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
1. Block IP `192.168.188.20` trên pfSense Firewall
2. Kiểm tra có login thành công nào không (`Accepted password`)
3. Reset password + bật key-based authentication
4. Review lại SSH config (`/etc/ssh/sshd_config`)
