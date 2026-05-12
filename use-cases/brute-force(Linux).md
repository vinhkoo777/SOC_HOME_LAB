# Brute Force - SSH (Linux)

## Attack Scenario
Attacker dùng Hydra từ Kali Linux để brute-force SSH vào Linux client.

## MITRE ATT&CK
- Tactic: Credential Access
- Technique: T1110.001 - Brute Force: Password Guessing

## Log Sources
- Linux syslog (`/var/log/auth.log`)
- Splunk Universal Forwarder → index=linux

## Detection Query (SPL)
\```spl
index=linux sourcetype=syslog "Failed password"
| stats count by src_ip, user
| where count > 5
\```

## Alert Condition
> 5 failed attempts trong vòng 1 phút từ cùng 1 IP

## Dashboard
[Screenshot hoặc link dashboard Splunk]

## Result
Detected X attempts từ IP 192.168.x.x, triggered alert thành công.
