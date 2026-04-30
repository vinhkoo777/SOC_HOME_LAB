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

Tôi sẽ để 60gb và nhấn next.

<img width="505" height="540" alt="image" src="https://github.com/user-attachments/assets/c677cdcd-f12c-46cd-a7f2-63d637e115f4" />

Ta sẽ cần Customize Hardware. Để chỉnh Network Adapter sang NAT (Tí nữa ta sẽ chỉnh sang VMnet1 (host-only) sau ). Xong rồi ta nhấn close để hoàn thành xong phần chuẩn bị. Và nhấn Finish.

<img width="895" height="897" alt="image" src="https://github.com/user-attachments/assets/ea8bad8d-b99c-4141-9c5a-b9561f5dfbb5" />

## Phase 2: Cài đặt Ubuntu server.

Ta nhấn enter để tiến hành cài đặt ubuntu server.

Tại đây ta chọn ngôn ngữ và tui sẽ để là tiếng anh. Ta nhấn enter để xác nhận.

<img width="972" height="507" alt="image" src="https://github.com/user-attachments/assets/b3cdb356-c4e1-49ca-850e-bce24a6a1c0e" />

Tiếp đó là cấu hình keyboard layout ở đây tui sẽ để mặc định và nhấn enter.

<img width="1278" height="467" alt="image" src="https://github.com/user-attachments/assets/bdfff16a-9ee7-4257-b8b8-a7e447df8156" />

Ở đây tui cũng sẽ để mặc định và tiếp tục nhấn enter tiếp.

<img width="1175" height="543" alt="image" src="https://github.com/user-attachments/assets/65b3169f-1f91-46cd-9872-056a5cc2e438" />

Tiếp tục enter.

<img width="1344" height="864" alt="image" src="https://github.com/user-attachments/assets/28f4db89-6404-42f6-987a-2e6c09274890" />

Nhấn enter.

<img width="1385" height="825" alt="image" src="https://github.com/user-attachments/assets/bed3a152-4b34-4087-b47b-e477c6dea547" />

Tại đây Ubuntu Server đang test Mirror. Nhấn enter để tiếp tục .

<img width="1367" height="843" alt="Screenshot 2026-04-30 205633" src="https://github.com/user-attachments/assets/f5e31ced-fd4e-4000-909a-5b918a8725f4" />

Ta sẽ dùng option use entire disk. Xong rồi sử dụng các nút mũi tên duy chuyển xuống done. Rồi nhấn enter.

<img width="1395" height="853" alt="image" src="https://github.com/user-attachments/assets/2b191297-6f79-4821-a13c-d974c5619581" />

Kiểm tra thông tin. Thấy okey rồi ta sẽ nhấn enter.

<img width="1344" height="818" alt="image" src="https://github.com/user-attachments/assets/5cc09c40-a34c-4650-939b-72777489f034" />

Ở đây ta confirm bằng cách nhấn vào continue. Để tiến hành xác nhân.

<img width="771" height="297" alt="image" src="https://github.com/user-attachments/assets/d010d34c-e7f5-4de0-be4d-1a995fb20520" />

Tại profile configuration. Ta tiến hành set up profile của chúng ta. Xong rồi nhấn enter để xác nhận.

<img width="1283" height="796" alt="image" src="https://github.com/user-attachments/assets/acd585df-eed0-4006-9cf1-14edd7ed56b7" />

Trong ssh configuration ta sẽ cần tick vào Install OpenSSH server. Xong rồi kéo xuống done rồi enter để tiếp tục.

<img width="1377" height="828" alt="image" src="https://github.com/user-attachments/assets/d1d591e1-5ad1-459a-8717-4f10792d72ee" />

Trong mục này ta sẽ không tải snap nào hết kéo xuống done và tiến hành cài đặt.

<img width="1308" height="832" alt="image" src="https://github.com/user-attachments/assets/07551ab9-968d-464e-91c3-fbd002c0127c" />

Sau khi cài đặt xong ta tiến hành reboot.

<img width="1293" height="846" alt="image" src="https://github.com/user-attachments/assets/150eb0b4-1a27-43ff-8515-b137e2217d3c" />

Tại đây ta sẽ cần chuột phải vào máy ảo Ubuntu Server chọn setting -> Xong rồi vào mục CD/DVD (Sata) rồi uncheck connect at power on. Nhấn ok để hoàn tất. Xong rồi tiếp đó ta sẽ nhấn enter để tiến hành quá trình vào ubuntu server.

<img width="1091" height="631" alt="image" src="https://github.com/user-attachments/assets/82795537-718d-417d-a8e6-c9deb801c91a" />

**Sau khi xong các bước trên ta nên tắt ubuntu server. Tiến hành đổi Network Adaptor sang VMnet1 (Host-Only). Và cuối cùng là tạo 1 bản snap shut để backup lại phòng khi có lỗi xảy ra.**

## Phase 2: Cấu hình IP tĩnh.

### Bước 1: Check IP 

Đầu tiên ta sẽ check ip hiện tại của ta

```bash
ip addr show
```

<img width="1028" height="299" alt="image" src="https://github.com/user-attachments/assets/b0a242a0-d9f4-4bf0-9feb-5f9c259a3255" />

Do tui mới đổi sang VMnet1(Host-only) nên ta sẽ cần tự config IP tĩnh.

### Bước 2: edit netplan configuration.

Tiếp theo đó sử nhập lệnh. Để edit netplan configuration.

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

### Bước 3: Thay đổi content trong config đó

Tại đây ta sẽ nhập giống như vậy. 

```bash
network:
  version: 2
  ethernets:
    ens33:
      addresses:
        - 192.168.188.10/24
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
      routes:
        - to: default
          via: 192.168.188.2
```

### Bước 4: Áp dụng config

Xong rồi ta sẽ apply config trên bằng cách nhập.
```bash
sudo netplan apply
```

## Bước 5: Tiến hành confirm 
Tại đây tui sẽ sử dụng lệnh dưới để kiểm tra IP của tui đã được config đúng hay chưa.
```bash
ip adđr show ens33
```

<img width="716" height="151" alt="image" src="https://github.com/user-attachments/assets/7b0d3cdd-6a55-4f99-a1e2-4a248dcdcb80" />

Thì ta thấy đó là những gì tui muốn. Tiến hành sử dụng tiếp lệnh ping. 
```bash
ping -c 3 8.8.8.8
```

Thì như trên hình thì ta đã thấy ta đã ping thành công. Và việc cấu hình IP tĩnh của chúng ta đã hoàn thành bây giờ ta sẽ qua phase tiếp theo.

<img width="481" height="95" alt="image" src="https://github.com/user-attachments/assets/079a75d0-4c1d-4483-9580-bf023bae491d" />


