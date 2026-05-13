# SOC Home Lab

**Lab an ninh mạng thực hành, mô phỏng các kịch bản tấn công thực tế và phát hiện tự động bằng hệ thống SIEM tự triển khai.**

## Mục Lục

- [Tổng quan](#tổng-quan)
- [Kiến trúc hệ thống](#kiến-trúc-hệ-thống)
- [Thiết lập môi trường](#thiết-lập-môi-trường)
- [Use Cases](#use-cases)

## Tổng Quan

SOC home lab mô phỏng môi trường doanh nghiệp vừa và nhỏ, bao gồm tường lửa, miền Active Directory, các máy đầu cuối Linux/Windows và máy tấn công — tất cả chạy trên **VMware Workstation (Host-Only Network)**.

**Mục tiêu thực hành:**

- Thu thập, tương quan và cảnh báo log bằng Splunk
- Mô phỏng tấn công thực tế
- Viết và tinh chỉnh detection rule tùy chỉnh bằng SPL (Splunk Processing Language)
- Xây dựng quy trình ứng phó sự cố bảo mật

## Kiến Trúc Hệ Thống

```
┌──────────────────────────────────────────────────────────────────────────────────────────┐
│                                  Host Computer                                           │
│  ┌────────────────────────────────────────────────────────────────────────────────────┐  │
│  │                    VM Workstation Host-Only Network                                │  │
│  │                          192.168.188.0/24                                          │  │
│  │                                                                                    │  │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐  ┌────────────┐│  │
│  │  │ pfSense  │  │  Splunk  │  │   Kali   │  │    AD    │  │Windows │  │   Linux    ││  │
│  │  │ Firewall │  │   SIEM   │  │ Attacker │  │  Domain  │  │ Client │  │   Client   ││  │
│  │  │ .188.2   │  │ .188.10  │  │ .188.20  │  │ .188.30  │  │ .188.40│  │  .188.50   ││  │
│  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘  └────────┘  └────────────┘│  │
│  │                                                                                    │  │
│  └────────────────────────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────────────────────┘

```

| Thành phần | IP | Vai trò |
|---|---|---|
| **pfSense Firewall** | 192.168.188.2 | Tường lửa, phân vùng mạng, giám sát lưu lượng |
| **Splunk SIEM** | 192.168.188.10 | Thu thập log, tìm kiếm SPL, cảnh báo, dashboard |
| **Kali Linux** | 192.168.188.20 | Máy tấn công để mô phỏng các mối đe dọa |
| **AD Domain Controller** | 192.168.188.30 | Active Directory + Splunk Universal Forwarder |
| **Windows Client** | 192.168.188.40 | Máy đầu cuối Windows + Splunk Universal Forwarder |
| **Linux Client** | 192.168.188.50 | Máy đầu cuối Linux + Splunk Universal Forwarder |

## Thiết Lập Môi Trường

Hướng dẫn từng bước để tái tạo lab từ đầu:

| # | Thành phần | Hướng dẫn |
|---|---|---|
| 1 | Tường lửa | [Thiết lập pfSense](setup/PHẦN1-pfsense.md) |
| 2 | Hệ thống SIEM | [Thiết lập Splunk](setup/PHẦN2-siem-setup.md) |
| 3 | Máy chủ tên miền | [Thiết lập Active Directory](setup/PHẦN3-active-directory.md) |
| 4 | Máy tấn công | [Thiết lập Kali Linux](setup/PHẦN4-kali-linux.md) |
| 5 | Máy đầu cuối Linux | [Thiết lập Linux Client](setup/PHẦN5-linux-client.md) |
| 6 | Máy đầu cuối Windows | [Thiết lập Windows Client](setup/PHẦN6-windows-client.md) |

> **Lưu ý:** pfSense phải được khởi động **trước tiên** trước khi cấu hình IP tĩnh trên bất kỳ máy nào. DHCP của VMware đã bị tắt — pfSense tại `192.168.188.2` là gateway duy nhất của mạng.

## Use Cases

Mỗi use case bao gồm đầy đủ: mô phỏng tấn công → detection rule (SPL) → bằng chứng log → ứng phó sự cố.

| Use Case | MITRE ATT&CK | Tài liệu |
|---|---|---|


**Cấu trúc mỗi use case:**

1. **Attack Scenario:** mô tả kịch bản, công cụ sử dụng 
2. **Detection Rule (SPL):** truy vấn Splunk, ngưỡng cảnh báo
3. **Log Evidence:** những gì Splunk ghi lại được
4. **Dashboard:** screenshot trực quan hóa
