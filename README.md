# BÀI TẬP 02 - LẬP TRÌNH WEB
# LƯƠNG NGỌC NAM - K225480106013
## NGÀY GIAO: 19/10/2025
## DEADLINE: 26/10/2025
==============================
# I. YÊU CẦU
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.

# II.NỘI DUNG BÀI TẬP
## 2.1. Cài đặt Apache web server
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server:
  Truy cập: https://www.apachelounge.com/download/
  Chọn file httpd-2.4.65-250724-win64-VS17.zip để tải về. 
- Giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf

    Sửa các chỗ:
    Define SRVROOT "E:/DataAutoBackup/LEARN-CODE/PhatTrienUngDungTrenNenWeb"
    
    ServerName localhost:80

    Include conf/extra/httpd-vhosts.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
    
    <VirtualHost *:80>
        ServerAdmin admin@luongngocnam.com
        DocumentRoot "E:/DataAutoBackup/LEARN-CODE/PhatTrienUngDungTrenNenWeb/luongngocnam"
        ServerName luongngocnam.com
        ErrorLog "logs/luongngocnam-error.log"
        CustomLog "logs/luongngocnam-access.log" common
    
        <Directory "D:/Apache24/luongngocnam">
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>
    </VirtualHost>
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:/Apache24/luongngocnam` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Lương Ngọc Nam` thì tạo website với domain là fullname ko dấu, liền nhau: `luongngocnam.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
  
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/60df3cb7-59c7-46b8-b3d5-96ca1a622b35" />

## 2.2. Cài đặt nodejs và nodered => Dùng làm backend
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
  <img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/3b71172f-9db7-4934-b833-ffdb535d1802" />


- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\`
  + tạo file ":\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/8baa6cb0-142d-41b2-987c-03c841ba8022" />
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/7e449b52-692c-4fb3-8784-6958dd0a4de4" />


