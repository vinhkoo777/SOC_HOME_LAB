# Active Directory Setup

Trong lab này, tôi triển khai một **Domain Controller** sử dụng Windows Server để quản lý user và authentication trong môi trường doanh nghiệp mô phỏng. Đồng thời sử dụng **BadBlood** để populate AD với user, group, OU giả lập — nhằm tạo môi trường giống doanh nghiệp thật.

## Mục Lục

- [Phase 1: Chuẩn bị môi trường](#phase-1-chuẩn-bị-môi-trường)
- [Phase 2: Cài đặt Domain Controller](#phase-2-cài-đặt-domain-controller)
- [Phase 3: Cài đặt BadBlood](#phase-3-cài-đặt-badblood)

## Phase 1: Chuẩn bị Môi Trường

### 1.1 — Cài đặt Windows Server

Phiên bản sử dụng: **Windows Server 2025**

**Bước 1: Tải ISO image**

Tải Windows Server 2025 [tại đây](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2025). Chọn phiên bản theo đánh dấu trong hình.

<img width="1575" height="852" alt="image" src="https://github.com/user-attachments/assets/7896cb52-8520-481a-bea7-023b203b7e63" />

**Bước 2: Tạo máy ảo trên VMware**

Nhấn vào **New Virtual Machine** để tạo máy ảo mới.

<img width="943" height="560" alt="image" src="https://github.com/user-attachments/assets/7efb5d32-2a64-453c-b157-2b81342416e7" />

Chọn **Typical**.

<img width="488" height="530" alt="image" src="https://github.com/user-attachments/assets/51f1442f-647e-476e-b3d1-af858d1c52bf" />

Chọn vào mục thứ 3, sau đó cấu hình theo hình bên dưới.

<img width="609" height="611" alt="image" src="https://github.com/user-attachments/assets/d893bf05-3ddf-4528-b5d0-3fa52ff80429" />

Đặt tên cho máy ảo và chọn nơi lưu, sau đó nhấn **Next**.

<img width="613" height="613" alt="image" src="https://github.com/user-attachments/assets/dd4faaa6-23ed-4643-b23c-959f46c4e877" />

Dung lượng ổ đĩa để **100 GB**.

<img width="688" height="622" alt="image" src="https://github.com/user-attachments/assets/570627bb-815e-4834-a4bb-4c05518db169" />

Nhấn **Customize Hardware** để cấu hình thêm. Đặt **Memory: 4.9 GB**, **Processors: 2**.

<img width="317" height="280" alt="image" src="https://github.com/user-attachments/assets/5c5d3183-96f8-47ae-83d2-b938101f9fd1" />

Vào **CD/DVD (SATA)** tiếp đó chọn mục 2 → browse đến file ISO Windows Server.

<img width="886" height="376" alt="image" src="https://github.com/user-attachments/assets/7155dc78-8b8b-45d6-bb2a-41692ada6655" />

**Bước 3: Cài đặt Windows Server**

Khởi động máy ảo. Các bước đầu nhấn **Next** liên tục.

Tại mục **Select Image** chọn phiên bản thứ 4 -> sau đó nhấn **Next** -> và Accept điều khoản.

<img width="1222" height="852" alt="image" src="https://github.com/user-attachments/assets/9a220331-f4c3-49d8-bc28-e2e916a49303" />

Tạo partition và nhấn **Next** để tiến hành cài đặt.

<img width="1274" height="820" alt="image" src="https://github.com/user-attachments/assets/5dd01ba5-29af-4ca0-9409-e8d5fb31ad1d" />

Sau khi cài đặt xong, nhập mật khẩu Administrator và nhấn **Next**.

<img width="1261" height="879" alt="image" src="https://github.com/user-attachments/assets/68648a44-871c-4889-951e-48e3e4040658" />

Cài đặt hoàn tất. (nên tạo snapshot để phòng hờ có chuyện gì đó xảy ra)

<img width="1027" height="763" alt="image" src="https://github.com/user-attachments/assets/61cd3755-faf7-459d-b4e9-e22ad5fedc64" />

### 1.2 — Cấu hình Static IP

Domain Controller cần IP tĩnh để DNS và authentication hoạt động ổn định.

1. Vào **Control Panel** -> **Network and Internet** -> **Network and Sharing Center**

Xong rồi ta bấm vào **Ethernet0**

<img width="791" height="585" alt="image" src="https://github.com/user-attachments/assets/3539af3e-d980-4bcb-bda4-1610a1da0016" />

2. Chọn **propertise**

<img width="365" height="452" alt="image" src="https://github.com/user-attachments/assets/f2c8c41d-0590-4625-9246-2a6ffd316c8f" />

3. Nhấn vào Internet Protocol Version 4 (TCP/IPv4)

<img width="357" height="462" alt="image" src="https://github.com/user-attachments/assets/bea6f572-c5ce-45b0-b615-af3150c4c311" />

Cấu hình như ảnh dưới :

<img width="392" height="417" alt="image" src="https://github.com/user-attachments/assets/c11a25af-604b-46e1-823e-3bc8d74cdc19" />

### 1.3 — Đổi Hostname thành DC01

**Bước 1:** Vào **Server Manager** -> chọn **Local Server**.

<img width="958" height="481" alt="image" src="https://github.com/user-attachments/assets/eb61c9ba-8dba-46f4-9367-36b4810d0c55" />

**Bước 2:** Nhấn vào **Computer Name** -> nhấn **Change**.

<img width="1028" height="722" alt="image" src="https://github.com/user-attachments/assets/c4e530a8-3dde-4a85-b5d1-e734eff81812" />

**Bước 3:** Đổi tên thành `DC01` -> nhấn **OK**. Máy sẽ yêu cầu khởi động lại để áp dụng.

<img width="537" height="571" alt="image" src="https://github.com/user-attachments/assets/ec7c708e-68ba-4ab1-951b-2be6a7fd54eb" />

**Bước 4:** Sau khi restart, kiểm tra lại hostname đã được cập nhật thành công.

<img width="1247" height="427" alt="image" src="https://github.com/user-attachments/assets/b915d18b-bb52-4cbe-9558-2b68b0fa1b49" />

---

## Phase 2: Cài Đặt Domain Controller

**Bước 1:** Tại màn hình chính Server Manager, chọn **Add roles and features**.

<img width="1670" height="448" alt="image" src="https://github.com/user-attachments/assets/c279696b-fa50-4f5b-9d33-c8054bd41b07" />

**Bước 2:** Kiểm tra các yêu cầu. Nếu đủ điều kiện, nhấn **Next**.

<img width="950" height="606" alt="image" src="https://github.com/user-attachments/assets/1a5e8070-42be-4f60-8dd4-c63640bc3a9a" />

**Bước 3:** Chọn **Role-Based or feature-based installation**.

<img width="1198" height="757" alt="image" src="https://github.com/user-attachments/assets/9d631928-406b-49fe-bd5e-406e97915711" />

**Bước 4:** Nhấn **Next** để tiếp tục.

<img width="1142" height="786" alt="image" src="https://github.com/user-attachments/assets/0893c5c0-9d02-4d16-a6a5-fe793b0a4915" />

**Bước 5:** Tick vào **Active Directory Domain Services** → nhấn **Add Features** → nhấn **Next** cho đến bước Confirm.

<img width="1190" height="788" alt="image" src="https://github.com/user-attachments/assets/4283f171-90d5-4fea-859e-1751770412e0" />

**Bước 6:** Kiểm tra lại cấu hình, sau đó nhấn **Install**.

<img width="1100" height="744" alt="image" src="https://github.com/user-attachments/assets/224597be-9127-407e-b190-91ff47824e7f" />

<img width="1075" height="722" alt="image" src="https://github.com/user-attachments/assets/f3dc246b-9311-48e2-94df-1eef35fc65ab" />

**Bước 7:** Sau khi cài xong, nhấn vào **biểu tượng cờ** → chọn **Promote this server to a domain controller**.

<img width="1286" height="561" alt="image" src="https://github.com/user-attachments/assets/5dd63c59-d56e-4665-8aac-b1ace2cbc226" />

**Bước 8:** Chọn **Add a new forest** → nhập Root domain name là `siem.lab` → nhấn **Next**.

<img width="940" height="527" alt="image" src="https://github.com/user-attachments/assets/a93231d6-6c2f-42d7-90f7-aec361febeaa" />

**Bước 9:** Đặt mật khẩu DSRM (ví dụ: `conmeo@1234`) → nhấn **Next** → giữ mặc định các bước còn lại → nhấn **Install**.

<img width="990" height="713" alt="image" src="https://github.com/user-attachments/assets/a72d1a9b-b5f1-4bd0-9bc4-5c07882e8293" />

> Máy sẽ tự động khởi động lại sau khi cài đặt xong. Domain Controller đã sẵn sàng.

## Phase 3: Cài Đặt BadBlood

> **Snapshot trước khi bắt đầu phase này** để có thể rollback nếu có sự cố.

BadBlood sẽ tự động tạo hàng trăm user, group, OU giả lập — giúp môi trường lab giống với thực tế doanh nghiệp hơn.

**Bước 1:** Truy cập repo [BadBlood trên GitHub](https://github.com/davidprowe/badblood) → nhấn **Code** → **Download ZIP**.

<img width="1918" height="752" alt="image" src="https://github.com/user-attachments/assets/0887d3d0-3512-4b1a-93cc-3f476f80c3be" />

**Bước 2:** Giải nén file ZIP. Mở **PowerShell** và di chuyển vào thư mục vừa giải nén.

<img width="950" height="606" alt="image" src="https://github.com/user-attachments/assets/530f36d5-5098-4759-8956-09feaefc7f7e" />

**Bước 3:** Chạy lần lượt các lệnh sau:

```powershell
# Cấp quyền thực thi script
Set-ExecutionPolicy Unrestricted -Scope Process -Force

# Unblock toàn bộ file trong thư mục
Get-ChildItem . -Recurse | Unblock-File

# Import module Active Directory
Import-Module ActiveDirectory

# Chạy BadBlood
.\Invoke-BadBlood.ps1
```

<img width="1315" height="571" alt="image" src="https://github.com/user-attachments/assets/61a0a5b3-589d-48e7-b8bf-f12c6b9adae9" />

**Bước 4:** Khi được hỏi, nhấn **Enter** và gõ `badblood`. Script sẽ bắt đầu chạy và populate AD.

<img width="1447" height="652" alt="image" src="https://github.com/user-attachments/assets/41953a61-aaff-471c-b631-b540217755b5" />

> Sau khi hoàn tất, AD sẽ được populate với nhiều user, group và OU giả lập. Môi trường lab đã sẵn sàng để thực hành phát hiện tấn công.
