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

## phaase 2: Thiết kế và cấu hình mạng 

### Bước 1: Thiết kế mạng cho Lab 

Cấu trúc lab của tui sẽ theo cấu trúc như thế này.



