# SIEM - Splunk Set up

Đây là phần thứ hai của chuỗi SOC_HOME_LAB (by k0g4). Sau khi có network foundation từ pfSense, bước tiếp theo ta sẽ sử dụng Splink làm SIEM - Nơi thu nhập và phân tích toàn bộ log từ lab.

## Phase 1: Chuẩn bị

### Bước 1: Tải iso image ubuntu sever 

- Ta có thể tải ubuntu server [tại đây](https://ubuntu.com/download/server)
- Xong rồi ta ấn vào download để tải Ubuntu server 26.04 LTS.

<img width="940" height="618" alt="image" src="https://github.com/user-attachments/assets/b6ee1bb0-5f0d-44eb-93c6-53713a6b21bf" />

### Bước 2: Tiến hành tạo máy ảo Ubuntu Server 

Đầu tiên ta sẽ vào File -> New virtual machine 

<img width="341" height="338" alt="image" src="https://github.com/user-attachments/assets/4a7984ff-01c4-4be4-a8fa-dcbcc899520a" />

Xong rồi ta sẽ chọn Typical. Xong rồi nhấn next.

<img width="500" height="546" alt="image" src="https://github.com/user-attachments/assets/6faac794-2371-4a3a-b2b0-57bba043ed9f" />

Tại New Virtual Machine Wizard tui sẽ chọn installer disc image file (iso). Xong rồi tui sẽ Browse tới file iso image. Xong rồi nhấn next.

<img width="493" height="541" alt="image" src="https://github.com/user-attachments/assets/8373474c-38a6-47c7-afb9-a6492b8d88ca" />

Tiếp theo bạn sẽ cần đặt tên máy ảo và chọn nơi mà máy ảo sẽ được lưu.

<img width="497" height="531" alt="image" src="https://github.com/user-attachments/assets/865cae5d-ccc5-45c2-a6d2-1cd16dbb549f" />

Tôi sẻ để 60gb và nhấn next.

<img width="505" height="540" alt="image" src="https://github.com/user-attachments/assets/c677cdcd-f12c-46cd-a7f2-63d637e115f4" />

Ta sẽ cần Customize Hardware. Để chỉnh Network Adapter sang vmNet1 (Host-only). Xong rồi ta nhấn close để hoàn thành xong phần chuẩn bị. Và nhấn Finish.

<img width="895" height="897" alt="image" src="https://github.com/user-attachments/assets/ea8bad8d-b99c-4141-9c5a-b9561f5dfbb5" />


