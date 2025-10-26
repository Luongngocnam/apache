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
<img width="1413" height="782" alt="image" src="https://github.com/user-attachments/assets/e88ae84d-ef25-41be-a415-64b249f5b97d" />

## 2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
- Ok. Đã có sẵn csdl QLSanPham với thông tin: ip=127.0.0.1, port=1433, username=sa, password=********, db_name=SamPham,
- table_name: MaSP, P, Gia.
## 2.4. Cài đặt thư viện trên nodered
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
  Ấn nút 3 gạch góc trên bên phải ==>Manager palette => chọn tab Install ==> Tìm kiếm thư viện cần tìm
  <img width="913" height="897" alt="image" src="https://github.com/user-attachments/assets/b7d16652-57f5-4958-8d28-2cb8083897fc" />
- Sửa file `D:\nodejs\nodered\work\settings.js` : 
 <img width="909" height="275" alt="image" src="https://github.com/user-attachments/assets/98d4db13-60a0-41df-9362-721a765cbada" />
 với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php

khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
<img width="1912" height="1025" alt="image" src="https://github.com/user-attachments/assets/c7c92a62-39e3-409c-be62-86df0d9b08f0" />

## 2.5. tạo api back-end bằng nodered
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  5.   có thể thêm node `debug` để quan sát giá trị trung gian.
   <img width="1081" height="373" alt="image" src="https://github.com/user-attachments/assets/22e99614-ce86-435c-91b1-57ce032d77ee" />
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=Thit
  <img width="1919" height="983" alt="image" src="https://github.com/user-attachments/assets/6d8c3a30-bc80-47f0-858d-2d5b903b95b8" />

  ## 2.6. Tạo giao diện front-end
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\luongngocnam`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, luongngocnam.js và luongngocnam.css
<img width="1406" height="774" alt="image" src="https://github.com/user-attachments/assets/289152e5-5f50-4615-a93f-35e20cfe8008" />


- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/96289863-4381-421a-a8e3-d4de39f31516" />

## 2.7. Nhận xét bài làm của mình
Quá trình bạn đã thực hiện cho thấy sự hiểu biết toàn diện về kiến trúc ứng dụng web cơ bản (Front-end, Back-end, Database) và các công cụ liên quan. Dưới đây là nhận xét tổng hợp về quá trình học tập và thực hành của bạn, trình bày dưới dạng đoạn văn mạch lạc:

Quá trình làm bài đã giúp em củng cố kiến thức một cách toàn diện về chu trình phát triển ứng dụng. Em đã nắm vững cách cài đặt và cấu hình các phần mềm cốt lõi: Apache được thiết lập như một Web Server nội bộ để phục vụ các file Front-end tĩnh, trong khi Node.js tạo môi trường cho Node-RED hoạt động như máy chủ API. Việc hiểu rõ cách Apache phục vụ nội dung qua thư mục htdocs và cách Node-RED mặc định chạy trên cổng 1880 đã làm rõ mối liên hệ giữa các thành phần trên cùng một máy tính. Về sử dụng Node-RED để tạo API Back-end, em đã thành thạo việc thiết lập luồng xử lý bằng giao diện kéo thả: từ việc định nghĩa endpoint bằng HTTP In và nhận tham số GET qua URL, sử dụng Function để xử lý logic, đến việc kết nối MSSQL để truy vấn dữ liệu thực, và cuối cùng dùng HTTP Response để trả về kết quả dưới dạng JSON. Đặc biệt, việc tự test API trực tiếp qua trình duyệt đã giúp xác nhận API hoạt động độc lập. Cuối cùng, qua việc xây dựng giao diện Front-end (HTML/CSS/JS), em đã hiểu rõ cách Front-end tương tác với Back-end theo quy trình chuẩn: file JavaScript chịu trách nhiệm fetch() dữ liệu từ API Node-RED, gửi kèm tham số tìm kiếm, nhận về phản hồi JSON, và sau đó xử lý dữ liệu này để hiển thị kết quả một cách trực quan trên giao diện người dùng. Tóm lại, em đã thành công trong việc xây dựng một hệ thống hoạt động đầy đủ, thể hiện sự hiểu biết rõ ràng về vai trò và sự liên kết giữa từng thành phần trong mô hình Client-Server.
==============================
# TIÊU CHÍ CHẤM ĐIỂM
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ
==============================
# GHI CHÚ
1. yêu cầu trên cài đặt trên ổ D, nếu máy ko có ổ D có thể linh hoạt chuyển sang ổ khác, path khác.
2. có thể thực hiện trực tiếp trên máy tính windows, hoặc máy ảo
3. vì csdl là tuỳ ý: sv cần mô tả rõ db chứa dữ liệu gì, nhập nhiều dữ liệu test có nghĩa, json trả về sẽ có dạng như nào cũng cần mô tả rõ.
4. có thể xây dựng nhiều API cùng cơ chế, khác tính năng: như tìm kiếm, thêm, sửa, xoá dữ liệu trong DB.
5. bài làm phải có dấu ấn cá nhân, nghiêm cấm mọi hình thức sao chép, gian lận (sẽ cấm thi nếu bị phát hiện gian lận).
6. bài tập thực làm sẽ tốn nhiều thời gian, sv cần chứng minh quá trình làm: save file `readme.md` mỗi khoảng 15-30 phút làm : lịch sử sửa đổi sẽ thấy quá trình làm này!
7. nhắc nhẹ: github ko fake datetime được.
8. sv được sử dụng AI để tham khảo.
  

  






