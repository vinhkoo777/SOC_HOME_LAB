# Windows Client Setup 

Đây là phần thứ sáu của chuỗi SOC_HOME_LAB (by k0g4). Phần này cũng sẽ giống như phần trước nhưng lần này ta sẽ đưa máy windows vào hệ thống và cài đặt Splunk Universal Forwarder để thu thập log từ endpoint gửi về SIEM. Windows sẽ đóng vai trò nạn nhân trong môi trường lab. Nơi các cuộc tấn công từ Kali được thực thi và dấu vết được ghi nhận, forward về Splunk để phân tích.

## Mục lục 

- [Phase 1: Chuẩn bị](#phase-1-chuẩn-bị)
- [Phase 2: Cài đặt windows 10](#phase-2-cài-đặt-windows-10)
- [Phase 3: cấu hình IP tĩnh](#phase-3-cấu-hình-IP-tĩnh)
- [Phase 4: Cài đặt Splunk Universal Forwarder](#phase-4-Cài-đặt-Splunk-Universal-Forwarder)

## Phase 1: Chuẩn bị 

### Bước 1: Tải ISO image windows 10 

Tại đây tui sẽ sử dụng windows 10 để làm windows client.

Đâu tiên bạn vô [link này](https://www.microsoft.com/en-us/software-download/windows10) để tải.

<img width="950" height="973" alt="image" src="https://github.com/user-attachments/assets/6bb41638-7580-4a4f-a0f7-9024ddfde1a7" />

Thì như ta thấy không có chỗ nào để cho ta tải file ISO hết nên ta sẽ dùng 1 trick.Đầu tiên là nhấn f12 để vào Devtools. Tiếp đó ta nhấn vào như trong hình. 

<img width="951" height="889" alt="image" src="https://github.com/user-attachments/assets/1f2ba1b1-efa4-407f-9f4f-194c551f9a24" />

Tiếp đó F5 lại. Và hình dưới cho ta thấy giờ ta đã có thể tải ISO image windows 10. Và tiến hành tải về.

<img width="355" height="544" alt="image" src="https://github.com/user-attachments/assets/7b83490c-9ed4-4bcf-b375-da9dfdce4676" />

### Bước 2 Chuẩn bị máy ảo

Đầu tiên ta vào **File -> New Virtual Machine -> Typica**l.

Tại đây ta chọn **Option Installler disc image file (iso)** xong ta truyền đường dẫn file ISO vào. Xong rồi nhấn next để tiếp tục.

<img width="492" height="532" alt="image" src="https://github.com/user-attachments/assets/43c09007-cb3d-4332-bb79-4f30c9dbe08e" />

Ở **Easy install information** tui sẽ chọn phiên bản là Window 10 pro. Và đặt username và password. Ta nhấn next xong có thông báo hiện ra chọn yes để tiếp tục.

<img width="497" height="532" alt="image" src="https://github.com/user-attachments/assets/c0f0918b-2a73-438c-b91b-3e636290e12d" />

Tại đây tôi sẽ đặt tên máy ảo và truyền vào đường dẫn lưu máy ảo đó. Sau đó nhấn next để tiếp tục.

<img width="500" height="531" alt="image" src="https://github.com/user-attachments/assets/b3cd5fd6-fa06-4f51-a848-6b49498b8a72" />

Tại phần **Specify Disk Capacity** ở trong này tui sẽ để **disk size** của tui là 70gb và nhấn next.

<img width="497" height="527" alt="image" src="https://github.com/user-attachments/assets/baccd1f6-9e75-4c02-a760-c512ea16ec6e" />

Tại đây tôi sẽ muốn chỉnh Network Adapter sang VMnet1 (host-only). Nên tui sẽ bấm vào **Customize Hard**

<img width="497" height="533" alt="image" src="https://github.com/user-attachments/assets/90242a65-dec0-404e-8d11-f79e7cfaea82" />

Và chỉnh như trong hình. Xong rồi ta nhẫn close

<img width="892" height="903" alt="image" src="https://github.com/user-attachments/assets/1a7f11db-13b7-446a-82e1-75cb40520ece" />

Nhấn finish để tiến hành vào máy ảo và cài đặt.

<img width="491" height="537" alt="image" src="https://github.com/user-attachments/assets/134de290-04b2-4d74-9533-46e5b5c009ae" />

## Phase 2: Cài đặt windows 10 

Khúc này chỉ đợi thôi VMWare đã tự cài cho ta hết rồi. Và đã cài xong windows 10. 

<img width="1914" height="912" alt="image" src="https://github.com/user-attachments/assets/8697025c-eb84-4923-8a3e-204720b1c8cd" />

## Phase 3: Cấu hình IP tĩnh

Đầu tiên Vào **Control Pannel -> Network and internet -> Network and sharing center**

Tiếp đó nhấn vào **Ethernet0**

<img width="792" height="595" alt="image" src="https://github.com/user-attachments/assets/7c201a59-8344-4bb1-9c66-fc8da55d0fa2" />

Xong rồi chọn **Properties**. 

<img width="362" height="448" alt="image" src="https://github.com/user-attachments/assets/ab7d429f-d478-4090-b099-a9a3e4afe918" />

Rồi vào **Internet Protocol Version 4 (TCP/IPv4)** Xong rồi setting giống trong hình. Rồi nhấn **ok** để lưu lại.

<img width="391" height="466" alt="image" src="https://github.com/user-attachments/assets/4488798f-65c1-47e2-b4c3-779159ac0eb4" />

Giờ ta sẽ dùng ```ping 8.8.8.8``` và ```ping 192.168.188.2```.

<img width="959" height="753" alt="image" src="https://github.com/user-attachments/assets/ff36ca1e-1526-43ba-9d6e-413dc93c09c4" />

Và ta đã ping thành công.

## Phase 4: Cài đặt Splunk Universal Forwarder

Trước khi cài đặt Splunk Universal Forwarder. Tui sẽ muốn bạn cập nhập bằng windows update. Khi cập nhập xong ta sẽ tắt máy ảo sau đó chuột phải vào máy ảo chọn Snapshot -> Take snapshot. (Để ta có thể quay lại thời điểm trước khi cài đặt Splunk Universal Forwarder)
