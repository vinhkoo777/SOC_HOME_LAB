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

**Bonus:** Các bước tới sẽ có 1 bước sẽ cần dùng sysmon nên ta tiến hành cài sysmon [tại đây](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) và [template](https://github.com/swiftonsecurity/sysmon-config)

### Bước 1: Tải file cài đặt Splunk Universal Forwarder

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

Sau khi xong bước trên ta vào Splunk trên Ubuntu Server. Đầu tiên vào **Setting** -> Tiếp đó **Forwarding and receiving** (nếu đã làm xong bước này thì không cần nữa)

<img width="1906" height="847" alt="image" src="https://github.com/user-attachments/assets/e8fc3bb1-616e-475a-9f4d-766535e62ca7" />

Tại **Configure Receiving** ta bấm vào **Add New**. Nhập Port là 9997 và ấn **Save** (nếu đã làm xong bước này thì không cần nữa)

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

