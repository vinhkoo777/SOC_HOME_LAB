# Linux Client Setup 

Đây là phần thứ năm của chuỗi SOC_HOME_LAB (by k0g4). Phần này sẽ đưa một máy Linux Client vào hệ thống và cài đặt Splunk Universal Forwarder để thu thập log từ endpoint gửi về SIEM. Linux Client sẽ đóng vai trò nạn nhân trong môi trường lab. Nơi các cuộc tấn công từ Kali được thực thi và dấu vết được ghi nhận, forward về Splunk để phân tích.

## Mục lục 

## Phase 1: Chuẩn bị

### Bước 1: Tải ISO image Ubuntu 26.04 LTS

Bạn có thể tải IOS image Uubuntu 26.04 LTS [tại đây](https://ubuntu.com/download/desktop). Sau khi nhấp vào link nhấn **download** để tải. 

<img width="1767" height="557" alt="image" src="https://github.com/user-attachments/assets/d532c7c5-b367-49bf-990e-17ac9927a932" />

### Bước 2 Chuẩn bị máy ảo Ubuntu

Đầu tiên vào File -> New Virtual Machine -> Xong rồi chọn Typical.

Tại **Guest Operating System Installion** tui sẽ chọn option Installer disc image file(iso). Tiếp đó browse tới file iso của tôi. Xong rồi ta nhấn next để tiếp tục.

<img width="493" height="538" alt="image" src="https://github.com/user-attachments/assets/d394bed9-6c39-458d-bac3-f06ccf283e87" />

Tại **Easy Install information** tui sẽ nhập trước username, password để khi tới bước cài đặt ta sẽ cần phải làm điều này nữa. Sau khi nhập xong ta nhấn next.

<img width="492" height="527" alt="image" src="https://github.com/user-attachments/assets/53ba394c-c26e-47d2-b211-b7a5032401e6" />

Tại **Name The Virtual Machine** tôi sẽ đặt tên và chọn thư mục chứa máy ảo của tôi. Sau đó nhấn next.

<img width="497" height="540" alt="image" src="https://github.com/user-attachments/assets/2cea28d5-9e79-4ccb-8ca3-6c389e262b50" />

Ở **Specify Disk Capacity** tui sẽ để là 70gb. Sau đó nhấn next để tiếp tục.

<img width="500" height="528" alt="image" src="https://github.com/user-attachments/assets/2c22c75f-e28e-420f-96c8-cf52243cd1f8" />

Tại đây tui sẽ để nguyên. **Ta sẽ chưa cần Customize hardware để đổi adapter sang VMnet1 (Host-only). Sau khi cài xong ta sẽ làm điều đó sau.**. Tiếp đó nhấn Finish để tiếp tục.

<img width="496" height="536" alt="image" src="https://github.com/user-attachments/assets/7aec2526-588a-4c89-8893-1b904be950b3" />

## Phase 2: Cài đặt Ubuntu

