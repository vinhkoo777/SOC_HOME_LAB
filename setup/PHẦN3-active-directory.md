# Active Directory Setup

Trong lab này, tôi triển khai một Domain Controller sử dụng Windows Server để quản lý user và authentication trong môi trường doanh nghiệp mô phỏng. Đồng thời sử dụng BadBlood để populate AD với user, group, OU giả lập để nhằm tạo môi trường giống doanh nghiệp thật.

## Mục Lục

## Mục Lục

- [Phase 1: Chuẩn bị](#phase-1-chuẩn-bị)
- [Phase 2: Cài đặt Windows Server](#phase-2-cài-đặt-windows-server)
- [Phase 3: Cấu hình IP tĩnh](#phase-3-cấu-hình-ip-tĩnh)
- [Phase 4: Đổi Hostname](#phase-4-đổi-hostname)
- [Phase 5: Cài đặt Domain Controller](#phase-5-cài-đặt-domain-controller)
- [Phase 6: Cài đặt BadBlood](#phase-6-cài-đặt-badblood)
- [Phase 7: Cài đặt Splunk Universal Forwarder](#phase-7-cài-đặt-splunk-universal-forwarder)
- [Một số lỗi có thể xảy ra](#một-số-lỗi-có-thể-xảy-ra)

## Phase 1: Chuẩn bị

### Bước 1: Cài đặt ISO image Windows Server

Phiên bản sử dụng: **Windows Server 2025**

Tải Windows Server 2025 [tại đây](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2025). Chọn phiên bản theo đánh dấu trong hình.

<img width="1575" height="852" alt="image" src="https://github.com/user-attachments/assets/7896cb52-8520-481a-bea7-023b203b7e63" />

### Bước 2: Tạo máy ảo windows server 

Ta chọn **File -> New Virtual Machine**

<img width="341" height="342" alt="image" src="https://github.com/user-attachments/assets/473c1045-494c-4550-968a-473d735d1a52" />

Chọn **Typical**.

<img width="488" height="530" alt="image" src="https://github.com/user-attachments/assets/51f1442f-647e-476e-b3d1-af858d1c52bf" />

Tại đây tôi sẽ chọn **I will install the operating system later**. Nhấn next để tiếp tục.

<img width="492" height="530" alt="image" src="https://github.com/user-attachments/assets/e19d529a-d1fd-40b6-9e0d-eb755bc5564f" />

Phần **Guest operating system** tôi sẽ để là **Microsoft Windows** và **Version** là **Windows Server 2025** Xong rồi nhấn next để tiếp tục. 

<img width="490" height="532" alt="image" src="https://github.com/user-attachments/assets/f021da40-aba1-41f2-bad8-50a79425da81" />

Tiếp đó ta đặt tên cho máy ảo và truyền đường dẫn nơi mà sẽ lưu máy ảo đó vào.

<img width="498" height="535" alt="image" src="https://github.com/user-attachments/assets/e4aa4629-e51d-4739-b55d-be616a528754" />

Ở **Specify Disk Capacity** tôi sẽ để **disk size** là 100gb và nhấn next để tiếp tục.

<img width="495" height="531" alt="image" src="https://github.com/user-attachments/assets/1a534924-5d92-42ca-ab22-016cf413731f" />

Tại đây ta bấm vào **Customize Hardware**.

<img width="498" height="532" alt="image" src="https://github.com/user-attachments/assets/6b59e773-04a4-4145-b84a-68885e986b0b" />

Đầu tiên ta vào **CD/DVD (SATA)** và truyền đường dẫn file ISO image windows server vào.

<img width="896" height="436" alt="image" src="https://github.com/user-attachments/assets/bfb5898f-bcce-479b-827b-035c038f38c0" />

Tiếp đó tui sẽ chỉnh **Memory** là 4gb và **Processors** là 2

<img width="890" height="895" alt="image" src="https://github.com/user-attachments/assets/79bbc33a-d9a5-4fd5-aeca-44f401f20cde" />

Ta nhấn close để hoàn thành. 

<img width="505" height="536" alt="image" src="https://github.com/user-attachments/assets/0c515610-6c7b-4e0b-a49e-452a6e1c0b5f" />

Và nhấn finish để hoàn tất quá trình tạo máy ảo.

## Phase 3: Cài đặt Windows server 

Khởi động máy ảo. 

<img width="1632" height="872" alt="image" src="https://github.com/user-attachments/assets/0afc067f-1ed9-404e-a261-10bba93f8536" />

Tại đây tui sẽ để mặc định và nhấn **Next** để tiếp tục

<img width="952" height="756" alt="image" src="https://github.com/user-attachments/assets/48288917-63dd-4785-9289-27e22805ea7d" />

Mục **Keyboard settings** tôi sẽ để là US và nhấn **Next**.

<img width="726" height="566" alt="image" src="https://github.com/user-attachments/assets/1de1d3de-12e4-48ed-b9c5-d5550369b20b" />

Ở **Select setup option** chọn **Install Windows Server** và tick dấu tick vào và nhấn **Next** để tiếp tục. 

<img width="727" height="567" alt="image" src="https://github.com/user-attachments/assets/d764fbcc-5c69-4080-8090-e157f78592d9" />

Tại mục **Select Image** chọn phiên bản thứ 4 -> sau đó nhấn **Next** -> và Accept điều khoản.

<img width="1222" height="852" alt="image" src="https://github.com/user-attachments/assets/9a220331-f4c3-49d8-bc28-e2e916a49303" />

Nhấn vào **Create Partition** để tạo phân vùng mới.

<img width="837" height="650" alt="image" src="https://github.com/user-attachments/assets/4847394a-1150-4f62-8604-3cb7e639529a" />

Tiếp đó nhấn **Apply**

<img width="760" height="610" alt="image" src="https://github.com/user-attachments/assets/4e4bc25b-8b2b-4dd5-a577-fe5b19839c01" />

Xong rồi chọn partition có type là **Primary**. Rồi nhấn **next** 

<img width="712" height="557" alt="image" src="https://github.com/user-attachments/assets/2ecdffc3-1369-45a4-b934-d5907fbafc58" />

Tại đây nhấn **install**

<img width="913" height="661" alt="image" src="https://github.com/user-attachments/assets/7ef658c7-4ff3-4d3b-a438-acbb800d027b" />

Sau khi cài đặt xong, Tại **Customize settings**, nhập mật khẩu Administrator và nhấn **Finish**.

<img width="932" height="620" alt="image" src="https://github.com/user-attachments/assets/43a2fdd6-933e-42b9-b4d5-b0351d052811" />

Cài đặt hoàn tất. (Khi này nên tạo 1 bảng snapshot để phòng có chuyện gì xảy ra)

<img width="1027" height="763" alt="image" src="https://github.com/user-attachments/assets/61cd3755-faf7-459d-b4e9-e22ad5fedc64" />

## Phase 3: Cấu hình IP tĩnh

Đầu tiên ta tắt máy đi xong rồi chuột phải vào máy ảo chọn **setting**

<img width="882" height="921" alt="image" src="https://github.com/user-attachments/assets/b7b5150e-c8f7-4e43-a8b5-63d52fe6d1e1" />

Xong rồi vào mục **Network Adapter** và đổi sang VMnet1 (Host-only). Nhấn **ok** để lưu lại.

<img width="882" height="921" alt="image" src="https://github.com/user-attachments/assets/897b32c3-b776-42d1-a7f6-2d9401c6f111" />

Ta mở máy rồi vào **Control Panel** -> **Network and Internet** -> **Network and Sharing Center**

Xong rồi ta bấm vào **Ethernet0**

<img width="791" height="585" alt="image" src="https://github.com/user-attachments/assets/3539af3e-d980-4bcb-bda4-1610a1da0016" />

Chọn **propertise**

<img width="365" height="452" alt="image" src="https://github.com/user-attachments/assets/f2c8c41d-0590-4625-9246-2a6ffd316c8f" />

Nhấn vào Internet Protocol Version 4 (TCP/IPv4)

<img width="357" height="462" alt="image" src="https://github.com/user-attachments/assets/bea6f572-c5ce-45b0-b615-af3150c4c311" />

Cấu hình như ảnh dưới

<img width="392" height="417" alt="image" src="https://github.com/user-attachments/assets/c11a25af-604b-46e1-823e-3bc8d74cdc19" />

Xong rồi ta khởi động lại máy.

## Phase 4: Đổi Hostname

**Bước 1:** Vào **Server Manager** -> chọn **Local Server**.

<img width="958" height="481" alt="image" src="https://github.com/user-attachments/assets/eb61c9ba-8dba-46f4-9367-36b4810d0c55" />

**Bước 2:** Nhấn vào **Computer Name** -> nhấn **Change**.

<img width="1028" height="722" alt="image" src="https://github.com/user-attachments/assets/c4e530a8-3dde-4a85-b5d1-e734eff81812" />

**Bước 3:** Đổi tên thành `DC01` -> nhấn **OK**. Máy sẽ yêu cầu khởi động lại để áp dụng.

<img width="537" height="571" alt="image" src="https://github.com/user-attachments/assets/ec7c708e-68ba-4ab1-951b-2be6a7fd54eb" />

**Bước 4:** Sau khi restart, kiểm tra lại hostname đã được cập nhật thành công.

<img width="1247" height="427" alt="image" src="https://github.com/user-attachments/assets/b915d18b-bb52-4cbe-9558-2b68b0fa1b49" />

---

## Phase 5: Cài Đặt Domain Controller

**Bước 1:** Tại màn hình chính Server Manager, chọn **Add roles and features**.

<img width="1670" height="448" alt="image" src="https://github.com/user-attachments/assets/c279696b-fa50-4f5b-9d33-c8054bd41b07" />

**Bước 2:** Kiểm tra các yêu cầu. Nếu đủ điều kiện, nhấn **Next**.

<img width="950" height="606" alt="image" src="https://github.com/user-attachments/assets/1a5e8070-42be-4f60-8dd4-c63640bc3a9a" />

**Bước 3:** Chọn **Role-Based or feature-based installation**.

<img width="1198" height="757" alt="image" src="https://github.com/user-attachments/assets/9d631928-406b-49fe-bd5e-406e97915711" />

**Bước 4:** Nhấn **Next** để tiếp tục.

<img width="596" height="487" alt="image" src="https://github.com/user-attachments/assets/58ad7715-1059-4a5f-a865-4aca9ec2fdac" />

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

## Phase 6: Cài Đặt BadBlood

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

## Phase 7: Cài đặt Splunk Universal Forwarder

## Một số lỗi có thể xảy ra

Trong quá trình mới khởi động lên tôi bắt gặp lỗi bị treo ở đây. Cách fix là bạn hãy tắt pfSense chừng nào tới bước cấu hình IP tĩnh rồi hẳng mở pfSense.

<img width="1191" height="798" alt="Screenshot 2026-05-02 204806" src="https://github.com/user-attachments/assets/484a9b10-f88b-41e3-80ea-9462d15e22a7" />

