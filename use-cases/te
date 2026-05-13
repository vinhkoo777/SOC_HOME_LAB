# Dashboard Structure Template

## Dashboard Overview

Template dashboard chuẩn dùng cho các use case SOC như:

* Kerberoasting
* Port Scan Detection
* SSH Brute Force
* RDP Brute Force
* PowerShell Activity

---

# Dashboard Layout

```text id="1f67n2"
+------------------------------------------------+
| Total Alerts | Top Attacker | Severity         |
+------------------------------------------------+
| Timeline Activity                              |
+------------------------------------------------+
| Top Targets | Top Ports | Top Actions          |
+------------------------------------------------+
| Raw Events Table                               |
+------------------------------------------------+
```

---

# 1. Alert Summary (Single Value)

## Mục đích

Hiển thị tổng số event đáng ngờ được phát hiện bởi detection rule.

## SPL

```spl id="93l10u"
<your_detection_rule>
| stats count
```

## Visualization

* Single Value

---

# 2. Timeline Activity

## Mục đích

Hiển thị số lượng sự kiện theo thời gian nhằm xác định thời điểm cuộc tấn công diễn ra.

## SPL

```spl id="ylwn2g"
<your_detection_rule>
| timechart count
```

## Visualization

* Line Chart

---

# 3. Top Attacker IP

## Mục đích

Xác định địa chỉ IP tạo nhiều sự kiện đáng ngờ nhất.

## SPL

```spl id="az7p5j"
<your_detection_rule>
| top src_ip
```

## Visualization

* Bar Chart

---

# 4. Top Targeted Accounts / Hosts

## Mục đích

Hiển thị tài khoản hoặc hệ thống bị nhắm mục tiêu nhiều nhất.

## SPL

```spl id="nfe0i6"
<your_detection_rule>
| top user
```

hoặc:

```spl id="jlwmpl"
<your_detection_rule>
| top dest_ip
```

## Visualization

* Bar Chart

---

# 5. Top Targeted Ports

## Mục đích

Xác định các cổng mạng bị nhắm mục tiêu nhiều nhất.

## SPL

```spl id="jlwmqm"
<your_detection_rule>
| top dest_port
```

## Visualization

* Pie Chart

---

# 6. Top Actions

## Mục đích

Hiển thị các hành động phổ biến trong log như:

* blocked
* allowed
* failed
* success

## SPL

```spl id="jlwmrn"
<your_detection_rule>
| stats count by action
```

## Visualization

* Pie Chart

---

# 7. Raw Events Table

## Mục đích

Hiển thị log chi tiết phục vụ investigation.

## SPL

```spl id="jlwms0"
<your_detection_rule>
| table _time src_ip dest_ip user action signature
```

## Visualization

* Table

---

# 8. Severity Panel

## Mục đích

Phân loại mức độ nghiêm trọng của cảnh báo.

## SPL

```spl id="jlwmt1"
<your_detection_rule>
| eval severity=case(
count>100,"high",
count>50,"medium",
true(),"low"
)
| stats count by severity
```

## Visualization

* Pie Chart

---

# Recommended Color Scheme

| Severity | Color  |
| -------- | ------ |
| High     | Red    |
| Medium   | Orange |
| Low      | Yellow |
| Success  | Green  |

---

# Notes

* Sử dụng cùng một dashboard structure cho mọi use case để giữ tính nhất quán.
* Dashboard nên tập trung vào:

  * visibility
  * detection
  * investigation
  * alert prioritization
* Tránh sử dụng quá nhiều panel gây rối dashboard.

---

# Example Use Cases

| Use Case            | ATT&CK ID |
| ------------------- | --------- |
| Kerberoasting       | T1558.003 |
| SSH Brute Force     | T1110     |
| RDP Brute Force     | T1110     |
| Port Scan Detection | T1046     |
| PowerShell Activity | T1059.001 |
