# Windows Client Setup 

Đây là phần thứ sáu của chuỗi SOC_HOME_LAB (by k0g4). Phần này cũng sẽ giống như phần trước nhưng lần này ta sẽ đưa máy windows vào hệ thống và cài đặt Splunk Universal Forwarder để thu thập log từ endpoint gửi về SIEM. Windows sẽ đóng vai trò nạn nhân trong môi trường lab. Nơi các cuộc tấn công từ Kali được thực thi và dấu vết được ghi nhận, forward về Splunk để phân tích.

## Mục lục 

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
