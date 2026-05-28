# SOC Home Lab
> Make by K0g4 with love <3333

**Lab an ninh mạng thực hành, mô phỏng các kịch bản tấn công thực tế và phát hiện tự động bằng hệ thống SIEM tự triển khai.**

## Mục Lục

- [Tổng quan](#tổng-quan)
- [Kiến trúc hệ thống](#kiến-trúc-hệ-thống)
- [Thiết lập môi trường](#thiết-lập-môi-trường)
- [Use Cases](#use-cases)

## Tổng Quan

SOC home lab mô phỏng môi trường doanh nghiệp vừa và nhỏ, bao gồm tường lửa, miền Active Directory, các máy đầu cuối Linux/Windows và máy tấn công tất cả chạy trên **VMware Workstation (Host-Only Network)**.

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
| 1 | Firewall | [Thiết lập pfSense](setup/PHẦN1-pfsense.md) |
| 2 | Hệ thống SIEM | [Thiết lập Splunk](setup/PHẦN2-siem-setup.md) |
| 3 | AD Domain | [Thiết lập Active Directory](setup/PHẦN3-active-directory.md) |
| 4 | Máy tấn công | [Thiết lập Kali Linux](setup/PHẦN4-kali-linux.md) |
| 5 | Linux Endpoint | [Thiết lập Linux Client](setup/PHẦN5-linux-client.md) |
| 6 | Windows Endpoint | [Thiết lập Windows Client](setup/PHẦN6-windows-client.md) |

> **Lưu ý:** pfSense phải được khởi động **trước tiên** trước khi cấu hình IP tĩnh trên bất kỳ máy nào. DHCP của VMware đã bị tắt. PfSense tại `192.168.188.2` là gateway duy nhất của mạng.

## Use Cases

| Use Case | MITRE ATT&CK | Tài liệu |
|---|---|---|
| Brute Force – SSH (Linux) | T1110.001 – Password Guessing | [Xem chi tiết](use-cases/brute-force-ssh-linux.md) |
| Brute Force – RDP (Windows) | T1110.001 – Password Guessing | [Xem chi tiết](use-cases/brute-force-rdp-windows.md) |

**Cấu trúc mỗi use case:**

1. **Attack Scenario** mô tả kịch bản, công cụ sử dụng
2. **Detection Rule (SPL)** truy vấn Splunk
3. **Log Evidence** những gì Splunk ghi lại được
4. **Dashboard** screenshot trực quan hóa
