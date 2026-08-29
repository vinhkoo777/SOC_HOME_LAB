# Join Domain và bật các log liên quan đến AD

Đây là phần thứ bảy của chuỗi SOC_HOME_LAB (by k0g4). Ở phần này ta sẽ join Windows Client vào domain `siem.lab` và bật các audit log cần thiết trên Active Directory để phục vụ việc giám sát và use case AS-REP Roasting.

## Mục lục

* [Phase 1: Chuẩn bị DNS](#phase-1-chuẩn-bị-dns)
* [Phase 2: Join Windows Client vào Domain](#phase-2-join-windows-client-vào-domain)
* [Phase 3: Bật Audit Policy](#phase-3-bật-audit-policy)
* [Phase 4: Test Log](#phase-4-test-log)

## Phase 1: Chuẩn bị DNS

Trên Windows Client, Ta vào phần cấu hình IPv4 và chỉnh Preferred DNS Server thành IP của Domain Controller:

```
192.168.188.30
```

Sau đó kiểm tra Windows Client có thể kết nối và resolve được `DC01` / `siem.lab`.

<img width="425" height="480" alt="image" src="https://github.com/user-attachments/assets/85567659-4875-4ab3-8d96-973d28c2222c" />

---

## Phase 2: Join Windows Client vào Domain

**Trên Windows Client:**

Đầu tiên ta vào setting.

<img width="1113" height="857" alt="image" src="https://github.com/user-attachments/assets/2aca642b-5cfc-48b9-b78e-f36f1896c77e" />

Ta vào Accounts.

<img width="1138" height="860" alt="image" src="https://github.com/user-attachments/assets/d7f4f15b-22b6-4770-9c7c-f79da2ce8b63" />

Rồi ta vào **Access work or school**.

<img width="1352" height="907" alt="image" src="https://github.com/user-attachments/assets/21055290-0a8b-4cc5-bbca-64cc21371597" />

Ấn vào connect.

<img width="1190" height="847" alt="image" src="https://github.com/user-attachments/assets/50189d1a-e330-49ea-8296-4014557d6767" />

Tiếp theo đó ta ấn vào **join this device to a local Active Directory domain**

<img width="1262" height="885" alt="image" src="https://github.com/user-attachments/assets/2e6975bf-5c00-4f54-934a-42cbc1e88103" />

Ta nhập Domain Name xong rồi ấn Next.

<img width="1255" height="812" alt="image" src="https://github.com/user-attachments/assets/a2a69c70-3e33-432a-bef8-e93862260879" />

Windows sẽ yêu cầu tài khoản có quyền join máy vào domain. Trong lab này tôi sử dụng tài khoản Administrator.

<img width="1207" height="847" alt="Screenshot 2026-08-27 122305" src="https://github.com/user-attachments/assets/5e98751c-aa18-42e5-b66c-164a4ae02daf" />

Chọn loại tài khoản/quyền phù hợp rồi nhấn Next. Windows sẽ bắt ta restart lại máy và sau khi restart xong ta sẽ thành công trong việc join vào domain.

<img width="1223" height="837" alt="Screenshot 2026-08-27 122319" src="https://github.com/user-attachments/assets/f73c1fd6-c010-49a5-9c65-fd0c2b4bfce9" />

Cuối cùng, kiểm tra trong Active Directory Users and Computers để xác nhận Windows Client đã được join vào domain.

<img width="797" height="586" alt="Screenshot 2026-08-27 122350" src="https://github.com/user-attachments/assets/0c277f02-b899-4ecf-9b9e-787165858276" />

## Phase 3: Bật Audit Policy

Trên `DC01`, mở **Group Policy Management** và tạo GPO:

<img width="1411" height="931" alt="image" src="https://github.com/user-attachments/assets/23ab0ffe-eb75-4524-9622-678eb4383645" />

Tiếp theo là đặt tên. Xong rồi ấn Ok để tiếp tục.

```text
SOC-Lab-Audit
```

<img width="1399" height="884" alt="image" src="https://github.com/user-attachments/assets/500a9d13-5c03-40d9-9bba-ae6c4ad38139" />

Link GPO vào domain `siem.lab`. Tiếp đó ta sẽ edit policy. Ta sẽ chuột phải và nhấn vào edit. Và tiến hành bật các audit log ở dưới.

<img width="1394" height="877" alt="image" src="https://github.com/user-attachments/assets/52939c76-dd3c-4d0d-a9f9-574a25f8b196" />

Đi tới: **Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration -> Audit Policies**

Bật các policy sau:

| Category      | Policy                                   | Setting          | Event      |
| ------------- | ---------------------------------------- | ---------------- | ---------- |
| Account Logon | Audit Kerberos Authentication Service    | Success, Failure | 4768       |

<img width="1403" height="781" alt="image" src="https://github.com/user-attachments/assets/814fc608-d50f-4c87-9a76-5c5a297d6b8e" />

### Buộc sử dụng Advanced Audit Policy

Đi tới: **Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Local Policies -> Security Options**

Bật: **Audit: Force audit policy subcategory settings (Windows Vista or later) to override audit policy category settings**

<img width="1193" height="509" alt="image" src="https://github.com/user-attachments/assets/0404cef1-30b4-4c11-84cc-3a3f3d131935" />

Sau đó cập nhật Group Policy bằng `gpupdate /force` hoặc restart máy.

## Phase 4: Test Log

Ta vào splunk và thực hiện câu spl dưới đây.

```
index=main source="WinEventLog:Security" "<EventID>4768</EventID>"
```

Thì như ảnh dưới, Splunk đã nhận được Event ID 4768 từ Domain Controller.

<img width="1902" height="956" alt="image" src="https://github.com/user-attachments/assets/b01a542e-90b4-45b0-96a5-4066be69d044" />

