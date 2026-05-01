# SOC_HOME_LAB 

> Đây là lab an ninh mạng thực hành (by k0g4), mô phỏng các kịch bản tấn công thực tế và phát hiện tự động bằng hệ thống SIEM tự triển khai.

![Nền tảng](https://img.shields.io/badge/Nền_tảng-pfSense%20%7C%20Windows%20AD%20%7C%20Kali%20Linux-blue)
![SIEM](https://img.shields.io/badge/SIEM-Splunk-orange)
![Trạng thái](https://img.shields.io/badge/Trạng_thái-Đang_hoạt_động-brightgreen)
![Giấy phép](https://img.shields.io/badge/Giấy_phép-MIT-lightgrey)

---

## 📋 Mục Lục

- [Tổng quan](#tổng-quan)
- [Kiến trúc hệ thống](#kiến-trúc-hệ-thống)
- [Thiết lập môi trường Lab](#thiết-lập-môi-trường-lab)
- [Quy tắc phát hiện](#quy-tắc-phát-hiện)
- [Kịch bản tấn công](#kịch-bản-tấn-công)
- [Kỹ năng thể hiện](#kỹ-năng-thể-hiện)
- [Bắt đầu sử dụng](#bắt-đầu-sử-dụng)

---

## Tổng Quan

Dự án xây dựng một **Trung tâm Vận hành Bảo mật (SOC) hoàn chỉnh** để thực hành:

- Thu thập, tương quan và cảnh báo log bằng **Splunk**
- Mô phỏng các cuộc tấn công thực tế (brute-force, leo thang đặc quyền, di chuyển ngang)
- Viết và tinh chỉnh các quy tắc phát hiện tùy chỉnh bằng **SPL (Splunk Processing Language)**
- Quy trình ứng phó sự cố bảo mật

Môi trường lab mô phỏng một doanh nghiệp vừa và nhỏ, gồm tường lửa, miền Active Directory, các máy đầu cuối Linux/Windows và máy tấn công.

---

## Kiến Trúc Hệ Thống

```
┌──────────────────────────────────────────────────────┐
│                    Mạng Lab                          │
│                                                      │
│  [pfSense Firewall]  ←──  [Kali Linux (Kẻ tấn công)] │
│         │                                            │
│  ┌──────┴───────────────────────────┐                │
│  │         Mạng nội bộ             │                 │
│  │                                  │                │
│  │  [Splunk Server]  [AD Server]    │                │
│  │  [Linux Client]   [Win Client]   │                │
│  └──────────────────────────────────┘                │
└──────────────────────────────────────────────────────┘
```

| Thành phần | Vai trò |
|------------|---------|
| **pfSense** | Tường lửa, phân vùng mạng, giám sát lưu lượng |
| **Splunk** | Thu thập log, tìm kiếm SPL, cảnh báo, dashboard trực quan |
| **Windows Server (AD)** | Bộ điều khiển miền Active Directory |
| **Kali Linux** | Máy tấn công để mô phỏng các mối đe dọa |
| **Linux Client** | Máy đầu cuối chạy Splunk Universal Forwarder (Ubuntu/Debian) |
| **Windows Client** | Máy đầu cuối chạy Splunk Universal Forwarder (Windows 10/11) |

---

## Thiết Lập Môi Trường Lab

Hướng dẫn từng bước để tái tạo môi trường lab từ đầu:

| # | Thành phần | Hướng dẫn |
|---|------------|-----------|
| 1 | Tường lửa | [Thiết lập pfSense](setup/PHẦN1-pfsense.md) |
| 2 | Hệ thống SIEM | [Thiết lập Splunk](setup/PHẦN2-siem-setup.md) |
| 3 | Máy chủ tên miền | [Thiết lập Active Directory](setup/PHẦN3-active-directory.md) |
| 4 | Máy tấn công | [Thiết lập Kali Linux](setup/PHẦN4-kali-linux.md) |
| 5 | Máy đầu cuối Linux | [Thiết lập Linux Client](setup/PHẦN5-linux-client.md) |
| 6 | Máy đầu cuối Windows | [Thiết lập Windows Client](setup/PHẦN6-windows-client.md) |

---

## Quy Tắc Phát Hiện

Các quy tắc phát hiện tùy chỉnh bằng SPL, phân loại theo hệ điều hành:

### 🐧 Linux
| Quy tắc | Mô tả |
|---------|-------|
| [SSH Brute Force](detection-rules/linux/ssh-bruteforce.md) | Phát hiện các lần đăng nhập SSH thất bại liên tiếp |

### 🪟 Windows
| Quy tắc | Mô tả |
|---------|-------|
| [Tấn công Brute Force](detection-rules/windows/brute-force.md) | Phát hiện dò mật khẩu và phun mật khẩu (password spraying) |
| [Leo thang đặc quyền](detection-rules/windows/privilege-escalation.md) | Phát hiện các nỗ lực leo thang đặc quyền cục bộ |

> Mỗi quy tắc bao gồm: logic phát hiện, truy vấn SPL, điều kiện cảnh báo Splunk và hành động ứng phó đề xuất.

---

## Kịch Bản Tấn Công

Mô phỏng tấn công đầu-cuối kèm tài liệu phát hiện và ứng phó:

| Kịch bản | Chiến thuật (MITRE ATT&CK) | Tài liệu |
|----------|---------------------------|----------|
| [Tấn công Brute Force](use-cases/brute.md) | Truy cập thông tin xác thực — T1110 | Toàn bộ luồng: tấn công → cảnh báo → ứng phó |

Mỗi kịch bản bao gồm:
1. **Mô phỏng tấn công** — công cụ và lệnh sử dụng (Kali Linux)
2. **Bằng chứng log** — những gì Splunk ghi lại và phân tích được
3. **Phát hiện** — truy vấn SPL và alert nào kích hoạt
4. **Ứng phó** — các bước ngăn chặn và xử lý sự cố

---

## Kỹ Năng Thể Hiện

- **Bảo mật mạng** — Cấu hình tường lửa, phân vùng mạng (pfSense)
- **Quản trị Splunk** — Triển khai Splunk, cấu hình Universal Forwarder, viết truy vấn SPL
- **Active Directory** — Thiết lập miền, chính sách người dùng/nhóm, giám sát event log
- **Phát hiện mối đe dọa** — Viết quy tắc phát hiện theo khung MITRE ATT&CK
- **Kiểm thử xâm nhập** — Mô phỏng tấn công có kiểm soát với Kali Linux
- **Ứng phó sự cố** — Thu thập bằng chứng, phân loại và ngăn chặn sự cố
- **Quản trị Linux & Windows** — Cấu hình và tăng cường bảo mật máy đầu cuối

---

## Bắt Đầu Sử Dụng

### Yêu cầu hệ thống

- Phần mềm ảo hóa (VMware / VirtualBox / Proxmox)
- Tối thiểu **16 GB RAM**, khuyến nghị **200 GB ổ đĩa**
- Kiến thức mạng cơ bản (subnetting, VLAN)

### Khởi động nhanh

1. Clone repository này:
   ```bash
   git clone https://github.com/k0g4/SOC_HOME_LAB.git
   ```
2. Làm theo các hướng dẫn cài đặt theo thứ tự: pfSense → Splunk → AD → Endpoints
3. Chạy một kịch bản tấn công trong `use-cases/` và kiểm tra phát hiện trên Splunk

---

## Giấy Phép

Dự án này được cấp phép theo [MIT License](LICENSE).

---

*Được xây dựng cho mục đích học tập — thực hành kỹ năng blue team và phát hiện mối đe dọa trong môi trường an toàn, cô lập.*
