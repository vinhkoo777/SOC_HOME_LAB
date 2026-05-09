# Use Case: Brute Force

## Metadata

| Trường | Giá trị |
|--------|---------|
| **Use Case ID** | UC-LINUX-000 |
| **Nền tảng** | linux |

---

## 1. Mô Tả Kịch Bản

Kẻ tấn công sử dụng **Brute Force** để truy cập vào tài khoản khi không biết mật khẩu là gì hoặc khi đã thu được các giá trị hash của mật khẩu. Khi không có thông tin về mật khẩu của nhiều tài khoản, kẻ tấn công có thể đoán mật khẩu một cách có hệ thống bằng cách lập lại và thử nghiệm liên tục.

Trong **Use Case** này attack đang muốn tìm cách vào tài khoản users conmeo của máy Ubuntu Linux. Và kẻ tấn công sử dụng hydra (công cụ brute force mật khẩu.)

---

## 2. MITRE ATT&CK

| Trường | Giá trị |
|--------|---------|
| **Tactic** | Credential Access |
| **Technique** | T1110 |
| **Tham khảo** | https://attack.mitre.org/techniques/TXXXX/ |

---

## 3. Môi Trường Lab

| Vai trò | IP | Ghi chú |
|---------|----|---------|
| **Attacker** | 192.168.188.20 | Kali Linux |
| **Target** | 192.168.188.50 | Ubuntu Linux |
| **SIEM** | 192.168.188.10 | Splunk |
| **Firewall/Router** | 192.168.188.2 | pfSense |

---

## 4. Mô Phỏng Tấn Công 

Bây giờ tôi sẽ thực hiện tấn công brute force. Máy thực hiện là kali linux có ip là `192.168.188.20`

### Bước 1: Hydra 

**Hydra** là một phần mềm cho phép tôi thực hiện brute force tốc độ cao 

```bash
hydra -l conmeo -P '/usr/share/wordlists/rockyou.txt' ssh://192.168.188.50
```

Giải thích về các flag được sài 
- `-l` : Thử login với user được truyền vào flag
- `-P` : Sử dụng một list mật khẩu có sẳn
- 
Và truyền vào một ssh server đã chuẩn bị sẳn. Ở đây tôi sẽ chuẩn bị máy Ubuntu của tôi có up là 192.168.188.50

### Bước 2: <!-- Tên bước -->

```bash
# Mô tả ngắn bước này làm gì
<lệnh thực tế>
```

### Bước 3: <!-- Tên bước -->

```bash
# Mô tả ngắn bước này làm gì
<lệnh thực tế>
```

## 5. Log Evidence

> Những gì Splunk thu thập được sau khi tấn công xảy ra

### 5.1 Nguồn Log

| Nguồn | Máy | Vị trí |
|-------|-----|--------|
| <!-- vd. linux_secure --> | <!-- 192.168.188.50 --> | <!-- /var/log/auth.log --> |
| <!-- vd. wineventlog --> | <!-- 192.168.188.40 --> | <!-- Windows Security Log --> |

### 5.2 Log Mẫu

```
(Dán log thực tế thu được từ Splunk vào đây)
```

### 5.3 Các Trường Quan Trọng

| Trường | Giá trị | Ý nghĩa |
|--------|---------|---------|
| `field_1` | `...` | <!-- Mô tả --> |
| `field_2` | `...` | <!-- Mô tả --> |
| `field_3` | `...` | <!-- Mô tả --> |

---

## 6. Phát Hiện (Detection)

### 6.1 SPL Query

```spl
(Dán SPL query phát hiện kịch bản này vào đây)
```

### 6.2 Alert Kích Hoạt

| Tên Alert | Rule ID | Thời điểm kích hoạt |
|-----------|---------|-------------------|
| <!-- Tên alert trong Splunk --> | <!-- DR-XXX-000 --> | <!-- Mô tả khi nào alert bắn --> |

### 6.3 Kết Quả Trên Splunk

<!-- Mô tả hoặc screenshot những gì hiển thị trên Splunk dashboard sau khi tấn công -->

---
