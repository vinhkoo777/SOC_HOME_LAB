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

Đầu tiên ta sẽ cần mở VM ware với quyền adminstrator

<img width="419" height="648" alt="image" src="https://github.com/user-attachments/assets/33357d5d-6953-4e56-847e-022f8cd0e3af" />

Tiếp theo mình cần xác định dải IP của mạng Host-Only. Để làm điều này, vào Edit → Virtual Network Editor.

Tại đây, ta có thể xem thông tin subnet address mà VMware cấp cho mạng Host-Only.

<img width="1112" height="532" alt="image" src="https://github.com/user-attachments/assets/7c806f86-f101-43f4-a537-5307e0bddcfd" />

<img width="693" height="91" alt="image" src="https://github.com/user-attachments/assets/9088d055-b147-479b-813a-3f5213179123" />

Trong trường hợp này, Subnet Address là 192.168.188.0/24. Tại Vmnet1. Ta cũng sẽ cần phải uncheck Use local DHCP service to distribute IP address to VMs -> Sau đó nhấn apply (Vì chút nữa ta sẽ cấu tình tĩnh IP.)

<img width="697" height="662" alt="image" src="https://github.com/user-attachments/assets/51d1d831-fdeb-48b9-b00c-365d7ebe02af" />

Nên trong lab này sẽ sử dụng địa chỉ IP thuộc dải 192.168.188.x .

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
│  │  │ Firewall │  │   SIEM   │  │ Attacker │  │  Domain  │  │ Client │  │   Client   ││  │
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

Tại đây Network Adapter đầu tiên của tui là NAT và cái thứ hai sẽ là Vmnet1(Host-only). Và ta nhấn close để thoát ra.

<img width="883" height="636" alt="image" src="https://github.com/user-attachments/assets/7ae5aade-48ec-4383-bfba-f6968e7289f8" />

Khi này ta sẽ nhấn Finish để hoàn thành.

<img width="647" height="632" alt="image" src="https://github.com/user-attachments/assets/255dcc41-4da3-48b0-b5f6-a1a8752ad09a" />

## Phase 4: Cài đặt pfSense

### Bước 1: Chấp nhận điều khoản.

Ta sẽ nhấn enter để chấp nhận.

<img width="945" height="552" alt="image" src="https://github.com/user-attachments/assets/40197dd8-ff18-499c-8e88-e61bf430c6a6" />

### Bước 2: Cài pfSense.

Ta sẽ select Install và nhấn Enter.

<img width="817" height="487" alt="image" src="https://github.com/user-attachments/assets/e28c8621-6fb7-4a93-8fc6-ed0301c3dc52" />

Tại WAN interfacface Assignment and Congiguration. Em0 sẽ là Network Adapter đầu tiên của tui ở đây là NAT nên tui sẽ chọn vào. ( Lưu ý nếu chọn Em1 ở đây là VMnet1 (Host-only) thì nó sẽ khá kì kiểu WAN mà thành nội bộ ấy =)) )

<img width="712" height="398" alt="image" src="https://github.com/user-attachments/assets/91e12664-1d90-4a55-82bd-11e87e96f2d8" />

Tại đây ta sẽ nhấn okey để tiếp tục.

<img width="833" height="542" alt="image" src="https://github.com/user-attachments/assets/73802b46-d194-4924-be6e-2952154d5c5c" />

Tại LAN interfacface Assignment and Congiguration chọn Em1 ở đây là Network Adapter thứ của tui ở đây là VMnet1 (Host-only)

<img width="705" height="408" alt="image" src="https://github.com/user-attachments/assets/8d2c2778-5e2f-4ea7-a313-cf4ccd56c1b4" />

Tại đây tui sẽ tắt DHCP và đặt IP address là 192.168.188.1. Sau đó tui nhấn enter.

<img width="810" height="492" alt="image" src="https://github.com/user-attachments/assets/57bfa3b3-5161-4dc2-bcc2-4cf77968c768" />

Tui sẽ ấn enter tiếp.

<img width="705" height="397" alt="image" src="https://github.com/user-attachments/assets/2623a222-73e2-4481-b7fb-27938bc16dbd" />

Ở đây tui sẽ đợi pfSense check kết nối. Sau khi check xong thì sẽ có bản thông báo hiện ra. Ấn enter để tiếp tục.

<img width="707" height="460" alt="image" src="https://github.com/user-attachments/assets/a94006e4-47d8-4992-93a7-57c3def1f33a" />

Tại Installation Option ta ấn enter tiếp

<img width="690" height="475" alt="image" src="https://github.com/user-attachments/assets/518b74be-57b8-4254-a4b1-bedcdd5dabbc" />

Ta sẽ enter cho đến khi tới bước Confirnmation rồi ta nhấn enter.

<img width="710" height="547" alt="image" src="https://github.com/user-attachments/assets/8771db39-edde-4e48-bf67-115f35243535" />

Tại đây ta sẽ chọn phiên bản mà ta muốn cài rồi ấn enter.

<img width="713" height="426" alt="image" src="https://github.com/user-attachments/assets/d6a5e916-e0c5-45a3-993c-89e487c9ec04" />

Bây giờ ta sẽ đợi quá trình cài đặt hoàn tất.

<img width="711" height="528" alt="image" src="https://github.com/user-attachments/assets/98e772b8-0335-414f-8bea-00df8e53e852" />
