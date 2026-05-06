# Use Case: Brute Force

## Metadata

| Trường | Giá trị |
|--------|---------|
| **Use Case ID** | UC-WIN-000 |
| **Nền tảng** | linux |
| **Mức độ nghiêm trọng** | <!-- Critical / High / Medium / Low --> |

---

## 1. Mô Tả Kịch Bản

<!-- Mô tả ngắn gọn: kẻ tấn công làm gì, mục tiêu là gì, tại sao nguy hiểm -->

---

## 2. MITRE ATT&CK

| Trường | Giá trị |
|--------|---------|
| **Tactic** | <!-- Credential Access / Privilege Escalation / ... --> |
| **Technique** | <!-- T1XXX – Tên technique --> |
| **Sub-technique** | <!-- T1XXX.00X – Tên sub-technique (nếu có) --> |
| **Tham khảo** | https://attack.mitre.org/techniques/TXXXX/ |

---

## 3. Môi Trường Lab

| Vai trò | IP | Ghi chú |
|---------|----|---------|
| **Attacker** | 192.168.188.20 | Kali Linux |
| **Target** | 192.168.188.50 | Ubuntu Linux |
| **SIEM** | 192.168.188.10 | Splunk |

---

## 4. Mô Phỏng Tấn Công (Attack Simulation)

> Thực hiện trên máy Kali Linux `192.168.188.20`

### Bước 1: <!-- Tên bước -->

```bash
# Mô tả ngắn bước này làm gì
<lệnh thực tế>
```

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

> Thêm/bớt bước tùy kịch bản

---

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
