# Brute Force – SSH (Linux)

**MITRE ATT&CK:** T1110.001 – Password Guessing  
**Tool:** Hydra  
**Target:** Linux Client (192.168.188.50)

---

## 1. Attack Scenario
Attacker dùng Hydra từ Kali Linux (192.168.188.20) brute-force SSH 
vào Linux Client, thử nhiều password từ wordlist rockyou.txt.

```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.188.50 -t 4
```

## 2. Detection Rule (SPL)

```spl
index=linux sourcetype=syslog "Failed password"
| rex field=_raw "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip, user
| where count > 5
| sort -count
```
**Alert condition:** > 5 failed attempts trong vòng 1 phút từ cùng 1 IP

## 3. Log Evidence

## 4. Dashboard
[Screenshot Splunk dashboard]

## 5. Response
1. Block IP `192.168.188.20` trên pfSense Firewall
2. Kiểm tra có login thành công nào không (`Accepted password`)
3. Reset password + bật key-based authentication
4. Review lại SSH config (`/etc/ssh/sshd_config`)

