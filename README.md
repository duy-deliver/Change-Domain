# HƯỚNG DẪN ĐỔI DOMAIN QUẢN TRỊ OFFICE365
## Cách 1: Sử dụng trực tiếp website
> Update sau 
## Cách 2: Sử dụng PowerShell

1.Kết nối đến Azure AD bằng câu lệnh ***Connect-msolservice***, sau đó nhập tài khoản

2.Xuất tất cả các user hiện tại với 
> Get-MsolUser | Select-Object UserPrincipalName | Export-Csv c:\Users.csv.

3.Một file ***.csv*** sẽ được tạo trong ổ đĩa C với cột ***UserPrincipalName***.

4.Thêm một cột mới ***EmailAddress***. với EmailAddress là domain thầy cô muốn chuyển sang ( ví dụ M365EDU220079.OnMicrosoft.com sang minhho.click )

Tham khảo công thức Excel
```=CONCATENATE(LEFT( A3, FIND("@",A3)-1),"@minhho.click")```

<img src="https://user-images.githubusercontent.com/65065691/210331799-eae1cffe-b05f-4cbf-865c-8231015f9f10.png" width="350"/>

6.Tạo một sp1 scipt dưới đây và chạy script
```
       $Userstodatabase = import-csv c:\Users.csv

       foreach ($Record in $Userstodatabase)
       {
       $upn = $record.userprincipalname
       $email = $record.emailaddress
       Write-host Setting $upn to $email
       Set-MSOLUserPrincipalName -UserPrincipalName $upn -NewUserPrincipalName $email
       Write-Host ".."
       }
```
7. Đổi Domain thành công
<img src="https://user-images.githubusercontent.com/65065691/210332276-ef8b5d09-c251-4f4b-b0fe-40583ad61d52.png" width="350"/>

*Lưu ý*:

1.Cần double-check lại ở trang Portal xem user đã được đổi domain chưa

2.Cần đổi domain của group user để được đồng bộ. Xem nhanh bằng cách: Domain -> chọn Domain -> Teams and Groups 
