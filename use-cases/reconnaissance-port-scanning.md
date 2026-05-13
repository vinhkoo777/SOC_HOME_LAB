# Reconnaissance Port Scanning

**MITRE ATT&CK:** T1046 - Network Service Discovery
**Tool:** Nmap
**Target:** Windows Client (192.168.188.40), Linux Client (192.168.188.50)

## 1. Attack Scenario

Attacker sử dụng hydra brute force dịch vụ RDP trên 1 host dưới đây là windows client. Với username là alex và list password đã chuẩn bị sẳng.

**Trong đây giả sử attacker đã thu được username**

```bash
sudo hydra -V -f -l alex -P '/home/kuga/rockyou.txt' rdp://192.168.188.40:3389  
```

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
