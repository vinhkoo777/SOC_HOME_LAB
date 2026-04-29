# pfSense Set Up 

Đây là phần đầu tiên của chuỗi SOC_HOME_LAB (by k0g4). Trong phần đầu tiên này tôi sẽ tiến hành setup pfSense. Đóng vai trò như **firewall/router** kiểm tra toàn bộ traffic mạng giữa các máy trong lab trước khi đưa vào SIEM phân tích.

## Phase 1: Chuẩn bị 

### Bước 1: Tiến hành tải IOS image pfSense

Có thể tải IOS image pfSense [tại đây](https://www.pfsense.org/download/). 

<img width="1110" height="230" alt="image" src="https://github.com/user-attachments/assets/ff6c7c59-ccb1-47a4-bd3f-0b5e9d6f87ff" />

Sau khi tải xong ta sẽ được 1 file nén có đuôi .gz . Ta tiến hành giải nén ra.

<img width="1112" height="162" alt="image" src="https://github.com/user-attachments/assets/e66c7c8e-38b0-47a5-b96d-78a7366b405a" />

### Bước 2: Chuẩn bị máy ảo 

Tại đây tui sẽ sử dụng VM Workstation. Bạn có thể tải [tại đây](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion). 

<img width="1919" height="877" alt="image" src="https://github.com/user-attachments/assets/d8c2c948-a367-4bc5-a8dd-d90f4dc55c3a" />

## Phase 2: Thiết kế và cấu hình mạng 

### Bước 1: Thiết kế mạng cho Lab 

Trước tiên, mình cần xác định dải IP của mạng Host-Only. Để làm điều này, vào Edit → Virtual Network Editor.

Tại đây, ta có thể xem thông tin subnet address mà VMware cấp cho mạng Host-Only.

<img width="1112" height="532" alt="image" src="https://github.com/user-attachments/assets/7c806f86-f101-43f4-a537-5307e0bddcfd" />

Trong trường hợp này, Subnet Address là 192.168.188.0/24.

<img width="697" height="102" alt="image" src="https://github.com/user-attachments/assets/d0d69817-c6ae-4071-a991-3e2985295e5c" />

Điều này có nghĩa là toàn bộ các máy ảo trong lab sẽ sử dụng địa chỉ IP thuộc dải 192.168.188.x .

- Network Address: 192.168.188.0
- Broadcast Address: 192.168.188.255
- Network Address: 192.168.188.1 - 192.168.188.254

Nên dựa vào dãi mạng mà ta có thể sử dụng được. Tôi set up cấu trúc lab như sau.

```
┌──────────────────────────────────────────────────────────────────────────────────────────┐
│                                  Host Computer                                           │
│  ┌────────────────────────────────────────────────────────────────────────────────────┐  │
│  │                    VM Workstation Host-Only Network                                │  │
│  │                          192.168.188.0/24                                          │  │
│  │                                                                                    │  │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐  ┌────────────┐│  │
│  │  │ pfSense  │  │  Splunk  │  │   Kali   │  │    AD    │  │Windows │  │   Linux    ││  │
│  │  │ Firewall │  │   SIEM   │  │ Attacker │  │ Domain   │  │ Client │  │   Client   ││  │
│  │  │ .188.1   │  │ .188.10  │  │ .188.20  │  │ .188.30  │  │ .188.40│  │  .188.50   ││  │
│  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘  └────────┘  └────────────┘│  │
│  │                                                                                    │  │
│  └────────────────────────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────────────────────┘
```

**Network Segments:**
- Management Network: 192.168.188.0/24 (Host-Only)
- Internet Access: NAT (Dùng để update và tải)

## Phase 3: Tạo máy ảo pfSense 

### Bước 1: Tiến hành tạo máy ảo pfSense

Ta vào File -> New Virtual Machine...

<img width="372" height="347" alt="image" src="https://github.com/user-attachments/assets/b545254d-ccf3-4387-bb9c-7232e17cc897" />

Tiếp đó chọn Typical xong rồi nhấn next.

<img width="505" height="547" alt="image" src="https://github.com/user-attachments/assets/b218276c-db3c-4d0d-bdbb-4cbe0ae89771" />

Tại New Virtual Machone Wizard. Ta chọn Option Installer disc image file(iso). Và browse tới file ios image pfSense khi nãy đã tải. 

<img width="501" height="536" alt="image" src="https://github.com/user-attachments/assets/060cd8bd-5a8f-4301-87c2-3341ad6dbce5" />

Tại đây đặt tên máy ảo như trên hình và lưu ở trong thư mục chứa các máy ảo liên quan đến project này. Sau đó nhấn next để tiếp tục.

<img width="543" height="563" alt="image" src="https://github.com/user-attachments/assets/b2edbb99-9809-441f-ac98-dbcf8361be85" />

Tại New Virtual Machine Wizard tui sẽ để mặc định. Và nhấn next để tiếp tục.

<img width="502" height="550" alt="image" src="https://github.com/user-attachments/assets/a570127a-9de0-4c68-8725-049748a1a5f5" />

Tại đây tui sẽ ấn vào Customize Hardware... . Để cấu hình thêm vài thứ nữa.

<img width="510" height="546" alt="image" src="https://github.com/user-attachments/assets/492ee891-af75-40d2-b06a-aa1fb51314c3" />

Tại đây tui sẽ để Memory là 1GB.

<img width="891" height="887" alt="image" src="https://github.com/user-attachments/assets/c40015b8-8ed1-4bf6-a3b4-bdb0eb410594" />

Tiếp đó tui sẽ thêm 1 Network adapter. Bằng cách bấm Add -> chọn Network Adapter -> Finish.

<img width="906" height="840" alt="image" src="https://github.com/user-attachments/assets/7bb80d49-38b2-4ac6-a5c3-cb1ea57616ac" />

Tại đây Network Adapter đầu tiên của tui là NAT và cái thứ hai sẽ là Host-only. Và ta nhấn close để thoát ra.

<img width="378" height="270" alt="image" src="https://github.com/user-attachments/assets/79e39147-4f2b-4e70-a5fd-eac51644fa4c" />

Khi này ta sẽ nhấn Finish để hoàn thành.

<img width="647" height="632" alt="image" src="https://github.com/user-attachments/assets/255dcc41-4da3-48b0-b5f6-a1a8752ad09a" />



