# Active Directory Setup (Domain Controller)

Trong lab này, tôi triển khai 1 Domain Controller sử dụng Windows Server để quản lý user và authentication trong môi trường doanh nghiệp mô phỏng cũng như sử dụng badBlood để populate AD với user, group, OU giả lập mục đích là tạo môi trường giống doanh nghệp thật

## Phase 1: Chuẩn bị (Preparation)
- Chuẩn bị môi trường trước khi cài AD

### Install Windows Server
Phiên bản ở đây tui sẽ sử dụng là Windows Server 2025 

- Bước 1: Tải ISO image của [Window Server](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2025) trên web chính chủ. Ta chọn phiên bản theo đánh dấu trong hình

![Anh_1](../images/Windows_Server_1.png)

- Bước 2: Ở đây tôi sẽ sử dụng VMware để tiến hành tạo máy ảo và cài đặt Windows Server nhằm phục vụ quá trình dựng Domain Controller trong lab.

  - Đầu tiên ta bấm vào New Virtual Machine để tạo máy ảo mới.
    
    <img width="943" height="560" alt="Anh_2" src="https://github.com/user-attachments/assets/7efb5d32-2a64-453c-b157-2b81342416e7" />
  
  - Tiếp theo đó ta sẽ chọn Typical.
    
    <img width="488" height="530" alt="image" src="https://github.com/user-attachments/assets/51f1442f-647e-476e-b3d1-af858d1c52bf" />

  - Chọn vào mục thứ 3.
    
    Ảnh

  - Tiếp đó để config giống như trong hình.
    
    <img width="609" height="611" alt="image" src="https://github.com/user-attachments/assets/d893bf05-3ddf-4528-b5d0-3fa52ff80429" />

  - Đặt tên cho máy ảo và nơi để lưu máy ảo sau đó nhấn next.
    
    <img width="613" height="613" alt="image" src="https://github.com/user-attachments/assets/dd4faaa6-23ed-4643-b23c-959f46c4e877" />

  - Ở đây tui sẽ để là 100gb.
 
    <img width="688" height="622" alt="image" src="https://github.com/user-attachments/assets/570627bb-815e-4834-a4bb-4c05518db169" />
    
  - Ta thấy rằng nó sẽ hiện thông tin về máy ảo của ta định cài ở đây tui sẽ muốn cấu hình thêm vài thứ nữa nên chọn Customize Hardware.
    
    <img width="718" height="617" alt="image" src="https://github.com/user-attachments/assets/3f87722a-7964-46bc-a049-0f0067ebe6cc" />

  - Tôi sẽ cấu hình memory là 4.9GB và Processors là 2.
   
    <img width="317" height="280" alt="image" src="https://github.com/user-attachments/assets/5c5d3183-96f8-47ae-83d2-b938101f9fd1" />

  - Ta qua mục CD/DVD (SATA). Bên phần connection ta chọn mục 2 và tiến hành browers đến file IOS Windows Server.

    <img width="886" height="376" alt="image" src="https://github.com/user-attachments/assets/7155dc78-8b8b-45d6-bb2a-41692ada6655" />

- Bước 3: Tiến hành vào máy ảo.

  - Mấy bước đầu cứ next next next :). Tại mục Select Image ta chọn phiên bản thứ 4. Sau đó nhấn next. Và sau đó accecpt các điều kiện.
    
    <img width="1222" height="852" alt="image" src="https://github.com/user-attachments/assets/9a220331-f4c3-49d8-bc28-e2e916a49303" />

  - Khi đó ta tiến hành create partition và tiến hành nhấn next để cài đặt. 
 
    <img width="1274" height="820" alt="image" src="https://github.com/user-attachments/assets/5dd01ba5-29af-4ca0-9409-e8d5fb31ad1d" />

  - Sau khi cài một lúc thì nó sẽ hiện giao diện giống như hình dưới và ta sẽ cần nhập mật khẩu mà ta muốn tạo. Sau đó ấn Next.
    
    <img width="1261" height="879" alt="image" src="https://github.com/user-attachments/assets/68648a44-871c-4889-951e-48e3e4040658" />

  - Sau khi cài xong ta sẽ được giao diện giống như vậy. (Sau khi cài xong nên tạo 1 snapshot để phòng chuyện gì xảy ra)
    
    <img width="1027" height="763" alt="image" src="https://github.com/user-attachments/assets/61cd3755-faf7-459d-b4e9-e22ad5fedc64" />

### Static IP Configuration

Domain Controller cần IP tĩnh để đảm bảo DNS và authentication hoạt động ổn đinh. 

- Bước 1: Vào Control Panel → Network and Sharing Center
- Bước 2: Chọn Change adapter settings
- Bước 3: Chuột phải vào Ethernet → Properties
- Bước 4: Chọn IPv4 → Properties

Cấu hình:

<img width="472" height="547" alt="image" src="https://github.com/user-attachments/assets/4eaa10c5-6aac-462d-900c-a9810b57e72c" />
 

### Hostname Change

Tại đây ta sẽ đổi hostname sang DC01. 

- Bước 1: Ta chuyển Local Server

<img width="958" height="481" alt="image" src="https://github.com/user-attachments/assets/eb61c9ba-8dba-46f4-9367-36b4810d0c55" />

- Bước 2: Tiếp đó ta ấn vào Computer Name. Và sau đó ấn vào change.

<img width="1028" height="722" alt="image" src="https://github.com/user-attachments/assets/c4e530a8-3dde-4a85-b5d1-e734eff81812" />

- Bước 3: Khi này tôi sẽ muốn đổi sang DC01 và nhấn okey. Máy sẽ yêu cầu ta khởi động lại để áp dụng hostname mới.

<img width="537" height="571" alt="image" src="https://github.com/user-attachments/assets/ec7c708e-68ba-4ab1-951b-2be6a7fd54eb" />

- Bước 4: Tiến hành kiểm tra lại và thấy rằng hostname đã được thay đổi.

<img width="1247" height="427" alt="image" src="https://github.com/user-attachments/assets/b915d18b-bb52-4cbe-9558-2b68b0fa1b49" />

## Phase 2: Cài đặt Domain Controller

- Bước 1: Ở màn hình chính ta chọn Add roles and features.
  
<img width="1670" height="448" alt="image" src="https://github.com/user-attachments/assets/c279696b-fa50-4f5b-9d33-c8054bd41b07" />

- Bước 2: Trước khi tiếp tục ta nên kiểm tra các yêu cầu dưới đây nếu đủ các yêu cầu rồi ta có thể nhấn Next để tiếp tục.
  
<img width="950" height="606" alt="image" src="https://github.com/user-attachments/assets/1a5e8070-42be-4f60-8dd4-c63640bc3a9a" />

- Bước 3: Chọn Role-Based or feature-based installation.

<img width="1198" height="757" alt="image" src="https://github.com/user-attachments/assets/9d631928-406b-49fe-bd5e-406e97915711" />

- Bước 4: Tiếp đó nhấn Next.

<img width="1142" height="786" alt="image" src="https://github.com/user-attachments/assets/0893c5c0-9d02-4d16-a6a5-fe793b0a4915" />

- Bước 5: khi này ta sẽ tick vào Active Directory Domain Service. Sau đó nhấn vào Add featues. Cuối cùng là nhấn Next cho tới bước confirm luôn.

<img width="1190" height="788" alt="image" src="https://github.com/user-attachments/assets/4283f171-90d5-4fea-859e-1751770412e0" />

- Bước 6: Ta check cấu hình lại khi đã thấy okey ta sẽ nhấn install.

<img width="1100" height="744" alt="image" src="https://github.com/user-attachments/assets/224597be-9127-407e-b190-91ff47824e7f" />

<img width="1075" height="722" alt="image" src="https://github.com/user-attachments/assets/f3dc246b-9311-48e2-94df-1eef35fc65ab" />


    





    

    

    




  



