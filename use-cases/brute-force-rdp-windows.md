# Brute Force RDP (Windows)

**MITRE ATT&CK:** T1110.003: Password Spraying  
**Tool:** Crowbar  
**Target:** Windows Client (192.168.188.40)  

## 1. Attack Scenario

Attacker sử dụng Crowbar brute force dịch vụ RDP trên 1 host dưới đây là windows client. Với username là alex và list password , sử dựng 1 thread.

**Trong đây giả sử attacker đã thu được username**

```bash
sudo hydra -l alex -P '/home/kuga/Desktop/common_pass.txt'  rdp://192.168.188.40:3389  
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
1. Block IP `192.168.188.20` trên pfSense Firewall
2. Kiểm tra có login thành công nào không (`Accepted password`)
3. Reset password + bật key-based authentication
4. Review lại SSH config (`/etc/ssh/sshd_config`)
