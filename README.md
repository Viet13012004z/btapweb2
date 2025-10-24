
Bài tập 02: Lập trình web. NGUYỄN HOÀNG VIỆT K225480106074
==============================
NGÀY GIAO: 19/10/2025
==============================
DEADLINE: 26/10/2025
==============================
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.
2. NỘI DUNG BÀI TẬP:
2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Đỗ Duy Cốp` thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
2.7. Nhận xét bài làm của mình:
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
- đã hiểu cách frond-end tương tác với back-end ra sao?
==============================
TIÊU CHÍ CHẤM ĐIỂM:
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ
==============================
GHI CHÚ:
1. yêu cầu trên cài đặt trên ổ D, nếu máy ko có ổ D có thể linh hoạt chuyển sang ổ khác, path khác.
2. có thể thực hiện trực tiếp trên máy tính windows, hoặc máy ảo
3. vì csdl là tuỳ ý: sv cần mô tả rõ db chứa dữ liệu gì, nhập nhiều dữ liệu test có nghĩa, json trả về sẽ có dạng như nào cũng cần mô tả rõ.
4. có thể xây dựng nhiều API cùng cơ chế, khác tính năng: như tìm kiếm, thêm, sửa, xoá dữ liệu trong DB.
5. bài làm phải có dấu ấn cá nhân, nghiêm cấm mọi hình thức sao chép, gian lận (sẽ cấm thi nếu bị phát hiện gian lận).
6. bài tập thực làm sẽ tốn nhiều thời gian, sv cần chứng minh quá trình làm: save file `readme.md` mỗi khoảng 15-30 phút làm : lịch sử sửa đổi sẽ thấy quá trình làm này!
7. nhắc nhẹ: github ko fake datetime được.
8. sv được sử dụng AI để tham khảo.
==============================
DEADLINE: 26/10/2025
==============================
./.

# 1.Cài đặt Apache web server:

# Vào cmd với quyền admin để dừng :

<img width="1094" height="630" alt="image" src="https://github.com/user-attachments/assets/7716a252-ab11-4e19-b8d6-a449e09229d0" />

Lên web tải apache24 về sau đó nén:

<img width="872" height="303" alt="image" src="https://github.com/user-attachments/assets/8448d363-be04-41c5-a26d-601f0c43a342" />

# Các bước cấu hình:

# Tạo website với domain tên của mình: 

<img width="708" height="687" alt="image" src="https://github.com/user-attachments/assets/0dc12601-0a43-40a3-8491-360df7775a97" />

# Bên trong folder này sẽ có file index.html để mình code vào đó để hiển thị lên web 

<img width="872" height="598" alt="image" src="https://github.com/user-attachments/assets/d4701d85-29e1-414f-8c07-14ebb0cdf752" />

<img width="1103" height="437" alt="image" src="https://github.com/user-attachments/assets/0ec0acda-075f-4d1d-a169-d85bcd99fc99" />

# Đây là cấu hình trong Virtual host:

<img width="1020" height="803" alt="image" src="https://github.com/user-attachments/assets/acf5dc0c-e6da-4ed5-bab0-10726746f370" />

Và đây là dòng lệnh để bật Virtual host lên( cái này trong cấu hình conf)

<img width="486" height="88" alt="image" src="https://github.com/user-attachments/assets/5fba5c00-2f73-49bd-8a5c-923bc5b4ccb8" />

# Đây là kết quả hiện thị lên web:

<img width="1859" height="630" alt="image" src="https://github.com/user-attachments/assets/9404ae13-ceca-4458-9062-76cd6c14a1d5" />

# 2. Cài đặt nodejs và nodered => Dùng làm backend:

# Đây là các file sau khi làm theo hướng dẫn 

<img width="974" height="650" alt="image" src="https://github.com/user-attachments/assets/fbfbd0ac-28d6-4c9c-b8c2-3bf6dd2c3ae2" />

# Sau khi cài thành công sẽ hiện ra ntn

<img width="1309" height="635" alt="image" src="https://github.com/user-attachments/assets/f30b3618-ea71-494a-9d1a-f538e795f56a" />

# Giao diện Nodejs

<img width="1914" height="1010" alt="image" src="https://github.com/user-attachments/assets/d82cc8ec-f3c3-4fef-b8f6-bc16353559c3" />

# Sau khi cấu hình lại trong note và tải hết thư viện 

<img width="1916" height="950" alt="image" src="https://github.com/user-attachments/assets/e1ce613e-3464-44a2-bac4-505cd73d81db" />

# 2.5. Tạo Api back-end bằng nodejs

<img width="847" height="670" alt="image" src="https://github.com/user-attachments/assets/00dc7065-2c50-4549-a624-281f584132e7" />

# node http in, http response

<img width="634" height="862" alt="image" src="https://github.com/user-attachments/assets/71d3f494-4360-4a7f-9be0-413f8d02f455" />

<img width="703" height="831" alt="image" src="https://github.com/user-attachments/assets/8c3f7338-9610-49d5-9ecd-ab88461a1bf4" />

# node mssql 

<img width="648" height="846" alt="image" src="https://github.com/user-attachments/assets/f5c4075b-686c-42f9-bcef-69792a0ca637" />

<img width="629" height="824" alt="image" src="https://github.com/user-attachments/assets/c1b39a0f-49f6-4eaa-9542-361109846931" />

# logic flow 

<img width="1252" height="307" alt="image" src="https://github.com/user-attachments/assets/9e0b3e42-b757-46bb-a62b-a38e9083ca76" />

# http://localhost:1880/timkiem?q=mì Khi nhập đường dẫn này để test api sẽ hiện ra giao diện như sau:

<img width="1439" height="930" alt="image" src="https://github.com/user-attachments/assets/7b31ded3-ce44-4377-8b77-28b589caea2f" />

# Front-end
# Tạo 3 file ra như hình

<img width="1330" height="535" alt="image" src="https://github.com/user-attachments/assets/4014fd81-9815-4faf-aa85-7dc01177604f" />

# Giao diện Front-end và truy vấn về back-end

<img width="1919" height="1013" alt="image" src="https://github.com/user-attachments/assets/1d0153eb-3e44-42cc-b89c-65a819e4e1db" />

# Kết luận
# Sau quá trình làm bài tập của thầy và dược thầy hướng dẫn tỉ mỉ chi tiết từng bước em đã rút ra những điều sau:
- Hiểu quá trình cài đặt phần mềm và thư viện
   + Qua quá trình thực hành, em đã nắm được các bước cài đặt Node.js, Node-RED và Apache trên Windows.
   + Hiểu cách thêm các thư viện cần thiết để tương tác với SQL và xử lý HTTP request/response.
   + Nhờ đó, môi trường phát triển back-end và front-end đã được chuẩn bị đầy đủ và hoạt động ổn định.
- Hiểu cách sử dụng Node-RED để tạo API back-end
   + Em đã biết cách tạo HTTP In node để nhận request, kết nối với MySQL node để truy vấn dữ liệu, và sử dụng HTTP Response node để trả kết quả.
   + Nắm được cách truyền dữ liệu từ front-end lên Node-RED qua GET/POST, xử lý JSON payload và trả về dữ liệu đúng format.
   + Biết cách debug Node-RED khi API chưa hoạt động, ví dụ 404 hoặc lỗi dữ liệu.
- Hiểu cách front-end tương tác với back-end
   + Em đã viết HTML, CSS, JS để tạo form nhập liệu, gửi request đến API Node-RED và nhận JSON trả về.
   + Biết cách xử lý dữ liệu nhận được: lọc theo từ khóa, hiển thị theo bảng hoặc div, xử lý trường hợp không có kết quả.
   + Hiểu rõ flow dữ liệu: người dùng nhập → JS gửi request → Node-RED truy vấn SQL → trả JSON → JS hiển thị kết quả.
