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

Tại **Easy Install information** tui sẽ nhập trước username, password.

<img width="492" height="527" alt="image" src="https://github.com/user-attachments/assets/53ba394c-c26e-47d2-b211-b7a5032401e6" />

Tại **Name The Virtual Machine** tôi sẽ đặt tên và chọn thư mục chứa máy ảo của tôi. Sau đó nhấn next.

<img width="497" height="540" alt="image" src="https://github.com/user-attachments/assets/2cea28d5-9e79-4ccb-8ca3-6c389e262b50" />

Ở **Specify Disk Capacity** tui sẽ để là 70gb. Sau đó nhấn next để tiếp tục.

<img width="500" height="528" alt="image" src="https://github.com/user-attachments/assets/2c22c75f-e28e-420f-96c8-cf52243cd1f8" />

Tại đây tui sẽ để nguyên. **Ta sẽ chưa cần Customize hardware để đổi adapter sang VMnet1 (Host-only). Sau khi cài xong ta sẽ làm điều đó sau.**. Tiếp đó nhấn Finish để tiếp tục.

<img width="496" height="536" alt="image" src="https://github.com/user-attachments/assets/7aec2526-588a-4c89-8893-1b904be950b3" />

## Phase 2: Cài đặt Ubuntu

Tại **Choose your language** tui để English và nhấn next.

<img width="1291" height="793" alt="image" src="https://github.com/user-attachments/assets/cce5d053-9b7b-4b2e-9f1c-317845e06ada" />

Ở **Accessibility** ta sẽ để mặc định và nhấn next.

<img width="955" height="678" alt="image" src="https://github.com/user-attachments/assets/836036bc-0be2-4a72-812a-04e5a52d2fff" />

Tại phần **Keyboard layout** tôi sẽ để là English (US). Xong rồi nhấn next để tiếp.

<img width="1282" height="802" alt="image" src="https://github.com/user-attachments/assets/db06192e-0181-4a6b-95b3-2e28433c7da2" />

Ở **Internet connection** tui sẽ chọn. **Do not connect to the internet**. 

<img width="1287" height="797" alt="image" src="https://github.com/user-attachments/assets/2bea840c-b054-4a21-92fe-952f38008250" />

Tại đây tôi sẽ muốn **Install Ubuntu** luôn. Nên tôi nhấn next để tiếp tục.

<img width="962" height="668" alt="image" src="https://github.com/user-attachments/assets/56ca111f-2b64-457c-aad5-254d49df5d18" />

Mục **Type of installation** sẽ chọn **Interactive Installation** tiếp đó nhấn next để tiếp tục.

<img width="961" height="668" alt="image" src="https://github.com/user-attachments/assets/fb6a815c-65df-47a1-80a4-4a3675f048da" />

Ở **Application** tôi sẽ chọn **Default selection**. Để cài đặt các ứng dụng mặc định cần thiết. Sau đó nhấn next để tiếp tục.

<img width="1272" height="753" alt="image" src="https://github.com/user-attachments/assets/ecd149d3-cf26-4f8c-8725-ea1f7ca7294b" />

Tại **Optimize Your compuiter** tôi sẽ để mặc định và nhấn next để tiếp tục. 

<img width="1282" height="802" alt="image" src="https://github.com/user-attachments/assets/078f1d73-5a66-4235-a80d-7b3d2c70f6a5" />

Ở **Disk setup** tui sẽ chọn option **Erase disk and install ubuntu** sau đó nhấn next.

<img width="1286" height="803" alt="image" src="https://github.com/user-attachments/assets/6d5c1356-4d40-435b-ac72-6fecb6e54eec" />

Mục **Encryption and file system** ta chọn **No encryption**. Tiếp đó nhấn next.

<img width="1283" height="803" alt="image" src="https://github.com/user-attachments/assets/1836afcc-8396-4bea-a71f-679b7d2fe8a8" />

Tại phần **Create your account** tui sẽ để như ở bước lúc mới tạo máy ảo Ubuntu. Rồi nhấn next.

<img width="1282" height="802" alt="image" src="https://github.com/user-attachments/assets/fc029500-175a-4f06-b609-afc89c0ebdcd" />

Tại đây bạn sẽ chọn time zone của bạn. Rồi nhấn next để tiếp tục.

<img width="1287" height="792" alt="image" src="https://github.com/user-attachments/assets/c7e640d0-2411-4526-a39a-40add17ce754" />

Ở đây ta sẽ xem lại các lựa chọn của ta sau khi thấy okey thì nhấn **install** để cài đặt. Và đợi 1 lúc cho ubuntu cài đặt xong.

<img width="1293" height="810" alt="image" src="https://github.com/user-attachments/assets/d819904f-d156-4758-baef-a9fd7decf379" />

Sau khi cài xong ta nhấn restart để khởi động lại.

<img width="1280" height="797" alt="image" src="https://github.com/user-attachments/assets/a4e2175a-f023-47e3-b863-f045424f4b9b" />


