# Kali Linux Setup 
 
Đây là phần thứ tư của chuỗi SOC_HOME_LAB (by k0g4). Sau khi có network foundation từ pfSense. Tại phần này tôi sẽ triển khai Kali Linux trong VMware Workstation nhằm mục đích giả lập tấn công và tạo log trong hệ thống SIEM. Kali Linux sẽ đóng vai trò Attacker Machine trong môi trường lab.

## Mục Lục

- [Phase 1: Chuẩn bị](#phase-1-chuẩn-bị)
- [Phase 2: Cài đặt Kali Linux](#phase-2-cài-đặt-kali-linux)
- [Phase 3: Sau khi cài đặt Kali Linux](#phase-3-sau-khi-cài-đặt)


## Phase 1: Chuẩn bị 

### Bước 1: Tải ISO Image Kali Linux

Truy cập trang chủ của [Kali Linux](https://www.kali.org/get-kali/#kali-installer-images) 

<img width="1908" height="932" alt="image" src="https://github.com/user-attachments/assets/8801741d-376a-49e4-8d84-6239dc8ee9f6" />

### Bước 3: Tạo máy ảo Kali Linux.

Đầu tiên ta chọn **File -> New Virtual Machine ...**

<img width="342" height="337" alt="image" src="https://github.com/user-attachments/assets/8b570b17-c040-47a0-8045-36db4b29bf40" />

Tiếp đó ta chọn **Typical (Recommended)**.

<img width="490" height="532" alt="image" src="https://github.com/user-attachments/assets/5f5fe372-409c-4b18-a16d-c5dab7d22dff" />

Ở đây tui sẽ chọn option **i will install the operating system later**

<img width="495" height="530" alt="image" src="https://github.com/user-attachments/assets/ac56d571-4c7d-494b-b6d6-b34baf24018a" />

Tiếp theo tôi sẽ đặt tên máy ảo và chọn thư mục sẽ lưu máy ảo đó vào.

<img width="497" height="531" alt="image" src="https://github.com/user-attachments/assets/44b8313f-9c1c-4f4c-acb6-74fd785b6260" />

Tại **Specify Disk Capacity** tôi sẽ để là 90gb và nhấn next để tiếp tục.

<img width="492" height="530" alt="image" src="https://github.com/user-attachments/assets/51a20a1d-fdee-4654-b2b6-c696fc33e025" />

Bước này tui sẽ phải đổi network adapter sang VMnet1 (Host-Only) và mount ỗ đĩa chứa file hệ điều hành kali linux. Nên ta tiến hành bấm vào Customize Hardware.

<img width="492" height="528" alt="image" src="https://github.com/user-attachments/assets/82352e3b-526e-4863-a8fd-96c4c2a03fcf" />

Ta sẽ nhấp vào mục CD/DVD (SATA). Tại góc bên tay phải ở optione **use ISO image file** ta sẽ browse đến file iso kali linux.

<img width="885" height="390" alt="image" src="https://github.com/user-attachments/assets/83224696-793c-4047-8cb0-9e57a04323f0" />

Khi cấu hình xong rồi ta nhấn vào Close để thoát ra. 

<img width="357" height="283" alt="image" src="https://github.com/user-attachments/assets/93f37e74-c549-4fd9-b6f0-70fa67ce0f03" />

Và nhấn Finish để hoàn thành.

<img width="490" height="532" alt="image" src="https://github.com/user-attachments/assets/98b082eb-6413-4f67-a491-c3c1217a1110" />

## Phase 2: Cài đặt Kali Linux 

### Bước 1: Chọn chế độ cài đặt

Khi boot từ ISO, màn hình GRUB hiện ra. Chọn Graphical Install để có giao diện thân thiện, dễ thao tác hơn so với Text Install.

<img width="717" height="633" alt="image" src="https://github.com/user-attachments/assets/b5dc2b05-33c0-4d72-ad07-2d6b4f23a315" />

### Bước 2: Chọn ngôn ngữ

Tại bước này ta sẽ chọn ngôn ngữ tại đây tôi sẽ để tiếng anh. Do khi có các thông báo lỗi thì các tài liệu hướng dẫn thường bằng tiếng anh nên ta có thể dễ tìm kiếm khi gặp sự cố.

<img width="808" height="613" alt="image" src="https://github.com/user-attachments/assets/c37b4406-26b6-4923-bea6-c30855c43cd5" />

### Bước 3: Chọn Location

Để mặc định hoặc muốn timezone chính xác. Nhưng mà tại đây tui sẽ để mặc định 

<img width="803" height="538" alt="image" src="https://github.com/user-attachments/assets/55215aee-f107-4d0c-b6ad-980278ffb60b" />

### Bước 4: Cấu hình Keyboard Layout

Tôi cũng sẽ để mặc định keyboard layout theo mặc định.

<img width="813" height="656" alt="image" src="https://github.com/user-attachments/assets/fa6d2f03-4e73-4db2-93a7-df550930478d" />

### Bước 5: Cấu hình mạng

Do tại đây tui đã đặt network adapter sang VMnet(host-only) nên vẫn chưa có mạng nên sẽ có thông báo trên. Ta chọn continue để tiếp tục 

<img width="798" height="593" alt="image" src="https://github.com/user-attachments/assets/6afab3f1-9f07-49fa-85e7-97d28b9536e1" />

Tại đây ta sẽ chọn optine **Do not configure the network at this time** xong rồi nhấn tiếp tục.

<img width="797" height="590" alt="image" src="https://github.com/user-attachments/assets/44ffa045-361e-45ae-b25f-e3f094d98485" />

### Bước 6: Đặt Hostname

Tại đây tôi sẽ để hostname là kali. Hostname này cũng sẽ xuất hiện trong log, giúp nhận diện máy tấn công khi phân tích trong SIEM 

<img width="797" height="602" alt="image" src="https://github.com/user-attachments/assets/f777f87b-8391-49f3-955d-9723c3c33844" />

### Bước 7: Tạo User và Password

Tại bước này tui sẽ để user tên là kuga. Xong rồi ta sẽ ấn continue.

<img width="800" height="601" alt="image" src="https://github.com/user-attachments/assets/726b13ae-62d0-4466-b630-243d0db58640" />

Tại đây tui cũng sẽ nhập lại user name và ấn continue.

<img width="796" height="602" alt="image" src="https://github.com/user-attachments/assets/ab2a78df-861c-47c4-bdfd-b00317c57bce" />

Tiếp đó ta sẽ đặt mật khẩu xong rồi ta nhấn continue để tiếp tục quá trình cài đặt.

<img width="802" height="602" alt="image" src="https://github.com/user-attachments/assets/8c2704a0-3448-455c-9575-568936cb11f6" />

### Bước 8: Cấu hình đồng hồ (Clock)

Cấu hình đồng hồ tôi vẫn sẽ để mặc định. 

<img width="800" height="608" alt="image" src="https://github.com/user-attachments/assets/23530f97-ba37-4a35-9a98-5e092f17580a" />

### Bước 9: Phân vùng ổ đĩa 

Chọn Guided - use entire disk. Phù hợp cho VM lab vì toàn bộ disk ảo chỉ dành riêng cho Kali.

<img width="921" height="762" alt="image" src="https://github.com/user-attachments/assets/1932203a-cf8a-4910-8401-9f0450a9b82d" />

### Bước 10: Cấu hình phân vùng

Tại đây tôi sẽ chọn All files in one partition. Sau đó nhấn continue.

<img width="793" height="597" alt="image" src="https://github.com/user-attachments/assets/0d2732a6-c305-4692-8a07-50ddc420492a" />

### Bước 11: Xác nhận phân vùng

Tại đây ta cũng sẽ nhấn tiếp continue.

<img width="802" height="607" alt="image" src="https://github.com/user-attachments/assets/a2e7a3cf-d441-4a5c-a4ce-4bafc0ee74f6" />


### Bước 12: Xác nhận ghi vào ổ đĩa

Ta tick vào yes và nhấn continue. Và ta sẽ tiến hành đợi kali tải base system.

<img width="846" height="682" alt="image" src="https://github.com/user-attachments/assets/54414618-2a1a-4efe-9b50-2950e802da8e" />

### Bước 13: Chọn Desktop Environment và công cụ

Tại bước này ta có thể tùy chọn thêm gnome hoặc kde nhưng mà tui sẽ để mặc định và ấn continue.

<img width="838" height="675" alt="image" src="https://github.com/user-attachments/assets/d9177008-978d-4208-bcba-35ac05015da1" />

### Bước 14: Cài đặt GRUB Bootloader

Chọn Yes → chọn ổ đĩa `/dev/sda` xong rồi chọn. Continue. GRUB là bootloader mặc định, cần thiết để khởi động hệ thống.

<img width="841" height="635" alt="image" src="https://github.com/user-attachments/assets/1efc1bc4-b2aa-478e-a108-23d5c82fe651" />

<img width="871" height="696" alt="image" src="https://github.com/user-attachments/assets/525454d6-586b-4e1b-aab6-de765efaf784" />

### Bước 15: Hoàn thành

Sau khi xong nhấn continue để reboot lại máy.

<img width="861" height="647" alt="image" src="https://github.com/user-attachments/assets/11e1eb3e-d506-4cf1-a47a-a132c6f177b8" />

## Phase 3: Sau khi cài đặt

### Bước 1: Cấu hình IP tĩnh

Đầu tiên ta dùng lệnh dưới để kiểm tra xem liệu file interfacs có tồn tại hay không.

```bash
cat /etc/network/interfaces
```
<img width="647" height="523" alt="image" src="https://github.com/user-attachments/assets/95735707-f2cd-468f-a3d7-b22f1e2bc540" />

Tiếp đó tôi sẽ sử dụng lệnh dưới để check xem liệu máy kali tui đang sử dụng interface nào. 
```bash
ifconfig
```

<img width="575" height="147" alt="image" src="https://github.com/user-attachments/assets/6111ed74-c2cd-44c8-afc2-d5a1d6b2419a" />

Thì ta thấy rằng kali đang sử dụng eth0 nên ta sẽ cấu hình IP tĩnh trên interface đó. Bằng cách sử dụng lệnh.
```bash
sudo nano /etc/network/interfaces
```
Xong rồi ta sẽ thêm nội dung dưới vào.

```bash
auto eth0
iface eth0 inet static
  address 192.168.188.20
  netmask 255.255.255.0
  gateway 119.168.188.2
  dns-nameservers 8.8.8.8 8.8.4.4
```
<img width="627" height="410" alt="image" src="https://github.com/user-attachments/assets/558763db-079f-438e-af21-2e187fbe3b09" />

Tiếp đó sử dụng.
```bash
sudo systemctl restart networking
```
Bây giờ tui sẽ kiểm tra bằng lệnh ```ifconfig``` và ```ping 8.8.8.8```

<img width="832" height="566" alt="image" src="https://github.com/user-attachments/assets/184d3f15-abaf-4037-8860-787b4216433e" />

Thì tui đã thành công trong việc cấu hình IP tĩnh

### Bước 2: Cập nhập hệ thống 

```bash
sudo apt update && sudo apt upgrade -y
```

<img width="947" height="233" alt="image" src="https://github.com/user-attachments/assets/dc9c8368-ce0d-4921-b7af-323e4bfae80a" />

### Bước 3: Tạo snapshot

Sao khi cập nhập xong ta nên tạo 1 bảng snapshot để có thể backup khi cần thiết. Bằng cách chuột phỉa vào máy ảo. Tiếp đó nhấn Take snapshot và đặt tên theo ta muốn và nhấn Take snapshot

<img width="646" height="367" alt="image" src="https://github.com/user-attachments/assets/191a233f-c8cd-4e8d-bf60-5f2521af8579" />

