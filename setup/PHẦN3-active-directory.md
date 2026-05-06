# Active Directory Setup

Trong lab này, tôi triển khai một Domain Controller sử dụng Windows Server để quản lý user và authentication trong môi trường doanh nghiệp mô phỏng. Đồng thời sử dụng BadBlood để populate AD với user, group, OU giả lập để nhằm tạo môi trường giống doanh nghiệp thật.

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

<img width="531" height="563" alt="image" src="https://github.com/user-attachments/assets/8684feeb-0d55-4a67-bdf1-b3329932b414" />

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

**Bước 6:** Tại **Feature** để nguyên và nhấn **Next** để tiếp tục.

<img width="1014" height="709" alt="image" src="https://github.com/user-attachments/assets/86f4ca55-2855-4eec-bd0f-c5badc921a27" />

**Bước 7:** Tại AD DS ta nhấn **Next**

<img width="1011" height="653" alt="image" src="https://github.com/user-attachments/assets/dab35684-0885-4e8d-807b-9cf9a96fdfa8" />

**Bước 7:** Kiểm tra lại cấu hình, sau đó nhấn **Install**.

<img width="1100" height="744" alt="image" src="https://github.com/user-attachments/assets/224597be-9127-407e-b190-91ff47824e7f" />

<img width="1075" height="722" alt="image" src="https://github.com/user-attachments/assets/f3dc246b-9311-48e2-94df-1eef35fc65ab" />

**Bước 8:** Sau khi cài xong, nhấn vào **biểu tượng cờ** → chọn **Promote this server to a domain controller**.

<img width="1286" height="561" alt="image" src="https://github.com/user-attachments/assets/5dd63c59-d56e-4665-8aac-b1ace2cbc226" />

**Bước 9:** Chọn **Add a new forest** → nhập Root domain name là `siem.lab` → nhấn **Next**.

<img width="940" height="527" alt="image" src="https://github.com/user-attachments/assets/a93231d6-6c2f-42d7-90f7-aec361febeaa" />

**Bước 10:** Đặt mật khẩu DSRM (ví dụ: `conmeo@1234`) → nhấn **Next** → giữ mặc định các bước còn lại → nhấn **Install**.

<img width="990" height="713" alt="image" src="https://github.com/user-attachments/assets/a72d1a9b-b5f1-4bd0-9bc4-5c07882e8293" />

> Máy sẽ tự động khởi động lại sau khi cài đặt xong. Domain Controller đã sẵn sàng.

## Phase 6: Cài Đặt BadBlood

**Snapshot trước khi bắt đầu phase này để có thể rollback nếu có sự cố.**

BadBlood sẽ tự động tạo hàng trăm user, group, OU giả lập để giúp môi trường lab giống với thực tế doanh nghiệp hơn.

**Bước 1:** Truy cập repo [BadBlood trên GitHub](https://github.com/davidprowe/badblood) -> nhấn **Code** -> **Download ZIP**.

<img width="1918" height="752" alt="image" src="https://github.com/user-attachments/assets/0887d3d0-3512-4b1a-93cc-3f476f80c3be" />

**Bước 2:** Giải nén file ZIP. Mở **PowerShell** và di chuyển vào thư mục vừa giải nén.

<img width="1182" height="757" alt="image" src="https://github.com/user-attachments/assets/09625d55-0020-4ed0-afe5-2b597e6e87d7" />

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

**Bước 4:** Khi được hỏi, gõ `badblood` và nhấn **enter**. Script sẽ bắt đầu chạy và populate AD.

<img width="1804" height="827" alt="image" src="https://github.com/user-attachments/assets/ccdd50f3-0afc-439c-8a23-431a0e7f794e" />

> Sau khi hoàn tất, AD sẽ được populate với nhiều user, group và OU giả lập. Môi trường lab đã sẵn sàng để thực hành phát hiện tấn công.

## Phase 7: Cài đặt Splunk Universal Forwarder

**Bonus:** Các bước tới sẽ có 1 bước sẽ cần dùng sysmon nên ta tiến hành cài sysmon [tại đây](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

## Bước 1: Tải file cài đặt Splunk Universal Forwarder

Tải file Splunk Universal Forwarder [ở đây](https://www.splunk.com/en_us/download/universal-forwarder.html?locale=en_us).( Sẽ cần phải đăng kí và đăng nhập tài khoản ). Xong rồi nhấn vào **Download Now** (ở đây tui dùng host để tải)

<img width="1439" height="598" alt="image" src="https://github.com/user-attachments/assets/b6f6470d-cb2c-4b63-b942-20c7d9744f7c" />

Sao khi download xong rồi ta sẽ copy file đó vào trong máy ảo.

### Bước 2: Cài đặt Splunk Universal Forwarder vào máy

Đầu tiên ta sẽ tick vào **check this box to accecpt the Liense Argeement**. Rồi nhấn **Next**.

<img width="802" height="536" alt="image" src="https://github.com/user-attachments/assets/e087d863-7b0b-4ea0-9672-1bd22bc2b53d" />

Ở đây tôi sẽ để **Username** là conmeo và tôi sẽ để mật khẩu được tạo random. Tiếp đó nhấn Next.

<img width="848" height="529" alt="image" src="https://github.com/user-attachments/assets/cabfe348-9ac5-4380-bc13-11f39314177c" />

Tại **Deployment Server** do tôi không có nên tôi sẽ để trống ở phần này.

<img width="733" height="535" alt="image" src="https://github.com/user-attachments/assets/625b7076-bb7c-4cd7-ab2f-a7315dcffa88" />

**Receiving Indexer** là phần quan trọng nhất. Tại đây tui sẽ để IP là 192.168.188.10 sẽ là IP của máy ảo Ubuntu Server đang cài splunk. Và port là 9997. Trước khi nhấn **Next**. Ta sẽ cần phải cấu hình vài thứ trong firewall rule và trên splunk.

<img width="666" height="573" alt="image" src="https://github.com/user-attachments/assets/e76e5fe2-77d9-4286-87bf-7c4e7f6d618b" />

Đầu tiên trên firewall trước. Ta vào **Windows Defender Firewall** -> **Advanced Settings**

<img width="702" height="594" alt="image" src="https://github.com/user-attachments/assets/5accdd70-2395-4612-bc10-fdd6b62a1fc6" />

Ta bấm vào **Outbounds Rules**. Xong rồi ấn tiếp **New Rule...**

<img width="704" height="652" alt="image" src="https://github.com/user-attachments/assets/cc71949f-d4a0-479f-b095-9f59730ea0df" />

Ở đây ta sẽ thêm rule theo chương trình. Xong rồi ấn **Next**

<img width="700" height="636" alt="image" src="https://github.com/user-attachments/assets/86e506e8-f2c0-45c2-8aec-e1e45e1fb617" />

Tại đây để `C:\Program Files\SplunkUniversalForwarder\bin\splunkd.exe`. Rồi **Next**.

Chọn **Allow the connection** rồi nhấn next. 

<img width="700" height="575" alt="image" src="https://github.com/user-attachments/assets/a62ba5b5-f1e6-4c4b-93da-cbfccd85847e" />

Tick hết cả 3. Và nhấn **Next**.

<img width="703" height="584" alt="image" src="https://github.com/user-attachments/assets/57e6f614-1bf0-4843-ba9b-38d76a669404" />

Cuối dùng là đặt tên role và Nhấn **Finish** Để hoàn thành.

<img width="694" height="583" alt="image" src="https://github.com/user-attachments/assets/9d1fd713-b7fe-4d83-85e3-82a2168d3827" />

Sau khi xong bước trên ta vào Splunk trên Ubuntu Server. Đầu tiên vào **Setting** -> Tiếp đó **Forwarding and receiving**

<img width="1906" height="847" alt="image" src="https://github.com/user-attachments/assets/e8fc3bb1-616e-475a-9f4d-766535e62ca7" />

Tại **Configure Receiving** ta bấm vào **Add New**. Nhập Port là 9997 và ấn **Save**

<img width="1916" height="867" alt="image" src="https://github.com/user-attachments/assets/b17f5d81-a8bc-4761-8f1e-1f6a2152ab12" />

Bây giờ ta sẽ quay lại với phần cài đặt. Ở đây tui sẽ nhấn next để tiếp tục ( **Phải xong ở trên trước khi nhấn next** )

<img width="666" height="573" alt="image" src="https://github.com/user-attachments/assets/b1145060-d869-4f32-b0a0-a78c8a0f0d27" />

Cuối cùng là nhấn **install** để tải. 

<img width="602" height="522" alt="image" src="https://github.com/user-attachments/assets/e32462bb-bf65-4d40-b4c0-6d455d8460a5" />

### Bước 3: Cấu hình inputs.conf để gửi log về Splunk

Đầu tiên ta vào `C:\Program Files\SplunkUniversalForwarder\etc\apps\SplunkUniversalForwarder\local` khi này ta tạo một file mới đặt tên là `inputs.conf`. Với nội dung.

```
# ==============================
#     WINDOWS EVENT LOGS
# ==============================

[WinEventLog://Security]
disabled = 0
index = main
sourcetype = WinEventLog:Security
renderXml = 1
checkpointInterval = 5
evt_resolve_ad_obj = 1
start_from = oldest

[WinEventLog://System]
disabled = 0
index = main
sourcetype = WinEventLog:System
renderXml = 1
checkpointInterval = 5

[WinEventLog://Application]
disabled = 0
index = main
sourcetype = WinEventLog:Application
renderXml = 1

# ==============================
# POWERSHELL AND EXECUTION
# ==============================

[WinEventLog://Microsoft-Windows-PowerShell/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:PowerShell
renderXml = 1

[WinEventLog://Microsoft-Windows-TaskScheduler/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:TaskScheduler

[WinEventLog://Microsoft-Windows-WMI-Activity/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:WMI-Activity

# ==============================
# SECURITY CONTROLS
# ==============================

[WinEventLog://Microsoft-Windows-AppLocker/EXE and DLL]
disabled = 0
index = main
sourcetype = WinEventLog:AppLocker

[WinEventLog://Microsoft-Windows-Windows Defender/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:Defender

# ==============================
# NETWORK AND ACCESS
# ==============================

[WinEventLog://Microsoft-Windows-Windows Firewall With Advanced Security/Firewall]
disabled = 0
index = main
sourcetype = WinEventLog:Firewall

[WinEventLog://Microsoft-Windows-TerminalServices-LocalSessionManager/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:RDP

[WinEventLog://Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:RDP

[WinEventLog://Microsoft-Windows-DNS-Client/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:DNS

# ==============================
# ACTIVE DIRECTORY 
# ==============================

[WinEventLog://Directory Service]
disabled = 0
index = main
sourcetype = WinEventLog:DirectoryService

[WinEventLog://Microsoft-Windows-Kerberos/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:Kerberos

[WinEventLog://Microsoft-Windows-Kerberos-Key-Distribution-Center/Operational]
disabled = 0
index = main
sourcetype = WinEventLog:KDC

# ==============================
# SYSMON 
# ==============================

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = main
sourcetype = XmlWinEventLog:Sysmon
renderXml = 1
checkpointInterval = 5
start_from = newest
current_only = 0

# ==============================
# FILE MONITOR 
# ==============================

[monitor://C:\Windows\System32\drivers\etc\hosts]
disabled = 0
index = main
sourcetype = WinFile:hosts
followTail = 1

# ==============================
# SPLUNK INTERNAL 
# ==============================

[monitor://$SPLUNK_HOME\var\log\splunk\splunkd.log]
index = _internal

[monitor://$SPLUNK_HOME\var\log\splunk\metrics.log]
index = _internal

# ==============================
# PERFORMANCE METRICS
# ==============================

[perfmon://CPU]
object = Processor
counters = % Processor Time
instances = _Total
interval = 60
index = main

[perfmon://Memory]
object = Memory
counters = Available MBytes
interval = 60
index = main

[perfmon://LogicalDisk]
object = LogicalDisk
counters = % Free Space
instances = *
interval = 60
index = main
```
Khi này sẽ cần phải restart lại Splunk Universal Forwarder. Ta sẽ mở **Powershell** với quyền admin. Rồi ta thực hiện các lệnh.

```
cd "C:\Program Files\SplunkUniversalForwarder\bin"
./splunk restart
```
<img width="843" height="367" alt="image" src="https://github.com/user-attachments/assets/fd82bc36-a315-4e86-9387-b9713b73d960" />

### Bước 4: Kiểm tra lại 

Tui sẽ SPL thử. Và như ta đã thấy ta đã thành công.

<img width="1917" height="882" alt="image" src="https://github.com/user-attachments/assets/c2328a1e-3107-402d-818c-71126a548dfe" />

## Một số lỗi có thể xảy ra

Trong quá trình mới khởi động lên tôi bắt gặp lỗi bị treo ở đây. Cách fix là bạn hãy tắt pfSense chừng nào tới bước cấu hình IP tĩnh rồi hẳng mở pfSense.

<img width="1191" height="798" alt="Screenshot 2026-05-02 204806" src="https://github.com/user-attachments/assets/484a9b10-f88b-41e3-80ea-9462d15e22a7" />

