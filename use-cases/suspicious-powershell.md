# Suspicious PowerShell Execution

**MITRE ATT&CK:** T1059.001 - PowerShell  
**Tool:** PowerShell  
**EventID:** Sysmon Event ID 1 
**Target:** Windows Client / AD Domain Controller

## 1. Attack Scenario

Mục tiêu của use case là phát hiện các lệnh PowerShell có dấu hiệu đáng ngờ thường gặp trong quá trình tấn công.

Use case này sử dụng **Sysmon Event ID 1 (Process Create)** làm telemetry chính.

### Test 1 - EncodedCommand

```
$text = 'Write-Host "TEST"'
$b64 = [Convert]::ToBase64String([Text.Encoding]::Unicode.GetBytes($text))
powershell.exe -NoProfile -EncodedCommand $b64
```

### Test 2 - ExecutionPolicy Bypass

```
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "Write-Host 'TEST'"
```

### Test 3 - Invoke-Expression

```
powershell.exe -NoProfile -Command "$cmd='Write-Host `"TEST`"'; Invoke-Expression $cmd"
```

### Test 4 - Base64 Decode

```
powershell.exe -NoProfile -Command "[Text.Encoding]::UTF8.GetString([Convert]::FromBase64String('U09DLUxBQiBURVNU'))"
```

### Test 5 - Invoke-WebRequest

Lệnh chỉ gọi localhost để tạo telemetry, không tải payload thật.

```
powershell.exe -NoProfile -Command "Invoke-WebRequest 'http://127.0.0.1:65535/payload.ps1' -UseBasicParsing"
```

### Test 6 - Hidden PowerShell

```
powershell.exe -NoProfile -WindowStyle Hidden -Command "Start-Sleep 2"
```

## 2. Detection Rule

Đây là rule giúp xác định các đoạn command đáng ngờ trong PowerShell command line như EncodedCommand, -enc, FromBase64String, Invoke-WebRequest, Invoke-Expression, ExecutionPolicy Bypass và WindowStyle Hidden.

```
index=main sourcetype="XmlWinEventLog:Sysmon" "<EventID>1</EventID>"
("EncodedCommand" OR "-enc" OR "FromBase64String" OR "Invoke-WebRequest" OR "Invoke-Expression" OR "ExecutionPolicy Bypass" OR "WindowStyle Hidden")
```

Ảnh dưới đây cho ta thấy rằng câu Query thành công. 

<img width="1910" height="958" alt="image" src="https://github.com/user-attachments/assets/71a7080d-f10b-4333-866e-e5321642dc1d" />

## 3. Log Evidence

Ở đây tôi chọn một log tiêu biểu:

<img width="1442" height="257" alt="image" src="https://github.com/user-attachments/assets/ff18dec9-406e-41be-bf40-9701012c4fbe" />

Có thể thấy **Sysmon Event ID 1** ghi nhận `powershell.exe` được thực thi với `Invoke-WebRequest` và thực hiện request tới URL `/payload.ps1`.

## 4. Dashboard

<img width="1684" height="912" alt="image" src="https://github.com/user-attachments/assets/b7da9c73-571c-4dca-8d2e-9d35bda6a594" />

