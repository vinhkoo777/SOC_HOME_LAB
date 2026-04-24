# Active Directory Setup (Domain Controller)

Trong lab này, tôi triển khai 1 Domain Controller sử dụng Windows Server để quản lý user và authentication trong môi trường doanh nghiệp mô phỏng cũng như sử dụng badBlood để populate AD với user, group, OU giả lập mục đích là tạo môi trường giống doanh nghệp thật

## Giai đoạn 1: Chuẩn bị (Preparation)
- Chuẩn bị môi trường trước khi cài AD

### Install Windows Server
Phiên bản ở đây tui sẽ sử dụng là Windows Server 2022 

- Bước 1: Tải file IOS của [Window Server](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2025) trên web chính chủ. Ta chọn phiên bản theo đánh dấu trong hình

![Anh_1](../images/Windows_Server_1.png)

- Bước 2: Ở đây tôi sẽ sử dụng VMware để tiến hành tạo máy ảo và cài đặt Windows Server nhằm phục vụ quá trình dựng Domain Controller trong lab.

  - Đầu tiên ta bấm vào New Virtual Machine để tạo máy ảo mới.
    
    <img width="943" height="560" alt="Anh_2" src="https://github.com/user-attachments/assets/7efb5d32-2a64-453c-b157-2b81342416e7" />
  
  - Tiếp theo đó ta sẽ chọn Typical.
    
    <img width="488" height="530" alt="image" src="https://github.com/user-attachments/assets/51f1442f-647e-476e-b3d1-af858d1c52bf" />

  - Chọn vào mục thứ 3.
  - 
    <img width="847" height="752" alt="image" src="https://github.com/user-attachments/assets/673a531a-ba30-4888-9156-dcabd63f8925" />

  - Tiếp đó để config giống như trong hình.
    
    <img width="609" height="611" alt="image" src="https://github.com/user-attachments/assets/d893bf05-3ddf-4528-b5d0-3fa52ff80429" />

  - Đặt tên cho máy ảo và nơi để lưu máy ảo sau đó nhấn next.
    
    <img width="613" height="613" alt="image" src="https://github.com/user-attachments/assets/dd4faaa6-23ed-4643-b23c-959f46c4e877" />

  - Ở đây tui sẽ để là 100gb.
 
    <img width="688" height="622" alt="image" src="https://github.com/user-attachments/assets/570627bb-815e-4834-a4bb-4c05518db169" />
    
  - Ta thấy rằng nó sẽ hiện thông tin về máy ảo của ta định cài ở đây tui sẽ muốn cấu hình thêm vài thứ nữa nên chọn Customize Hardware.
    
    <img width="718" height="617" alt="image" src="https://github.com/user-attachments/assets/3f87722a-7964-46bc-a049-0f0067ebe6cc" />

  - Tôi sẽ cấu hình memory là 4.9GB và Processors là 2
   
    <img width="317" height="280" alt="image" src="https://github.com/user-attachments/assets/5c5d3183-96f8-47ae-83d2-b938101f9fd1" />

  - Ta qua mục CD/DVD (SATA). Bên phần connection ta chọn mục 2 và tiến hành browers đến file IOS Windows Server.

    <img width="886" height="376" alt="image" src="https://github.com/user-attachments/assets/7155dc78-8b8b-45d6-bb2a-41692ada6655" />



 
  

    





    

    

    




  



