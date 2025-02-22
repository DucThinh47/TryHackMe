# Pickle Rick

***Question 1: Nguyên liệu đầu tiên mà Rick cần là gì?***

=> Truy cập website: 

![img](214)

=> Xem source page:

![img](215)

=> Tìm được một username: ***R1ckRul3s***

=> Thử sử dụng ***nmap*** kiểm tra các cổng mở trên web

- Thêm tùy chọn ***-sV***: Thu thập thông tin service đang chạy trên các port.

- Thêm tùy chọn ***-sC***: Chạy các script mặc định để thu thập thêm thông tin chi tiết.

![img](216)

=> Xem file ***robots.txt***:

![img](217)

=> Tìm được chuỗi ***Wubbalubbadubdub***.

=> Thử truy cập ***login.php*** xem website có trang login viết bằng PHP không:

![img](218)

=> Thử login với ***username=R1ckRul3s*** và ***password=Wubbalubbadubdub***:

![img](219)

=> Login thành công, điều hướng đến ***/portal.php***. 

=> Thử nhập ***ls;***:

![img](220)

=> Trong Burp Suite, Intercept request khi nhập ***ls;***:

![img][221]

=> Tìm được một chuỗi base64, thử giải mã base64 -> base64 -> base64... cho đến khi thu được: 

![img](222)

=> Thu được chuỗi ***rabbit hole***.

=> Thử đọc file có tên khả nghi: 

![img](223)

=> Không đọc được. Thử truy cập file .txt này thẳng trên URL: 

![img](224)

![img](225)

=> Thu được chuỗi ***mr.meeseek hair***

=> Hoặc dùng lệnh ***less*** thay ***cat*** để đọc file trực tiếp: 

![img](226)

=> less so với cat:

- less cho phép scroll page, cat in toàn bộ nội dung 1 lần
dễ sử dụng và hiệu quả, đặc biệt với file lớn

![img](227)

***Question 2: What is the second ingredient in Rick’s potion?***

=> Thử đọc file clue.txt: 

![img](228)

=> Hệ thống gợi ý là tìm trong file system.

=> Thử nhập lệnh ***cd /home;pwd;ls;***

![img](229)

=> Tiếp tục thử truy cập thư mục ***risk***:

    cd /home/rick;pwd;ls;

![img](230)

=> Tiếp tục thử truy cập thư mục second ingredients: 

    cd /home/rick/second ingredients;pwd;ls;

![img](231)

=> không truy cập được

=> Có vẻ second ingredients không phải 1 thư mục.

=> Thử lệnh ***cat /home/rick/second ingredients;***:

![img](232)

=> không xem được.

=> Thử lệnh ***less /home/rick/second ingredients;***

![img](233)

Thử xác định second ingredients là 1 string, dùng lệnh:

    cat /home/rick/’second ingredients’;

![img](234)

=> Không được. Thử lệnh ***less /home/rick/’second ingredients’;***

![img](235)

=> Tìm được chuỗi ***1 jerry tear***.

![img](236)

***Question 3: What is the last and final ingredient?***

Tiếp tục dựa vào gợi ý từ file clue.txt, tìm kiếm trong thư mục hệ thống.

=> Ý tưởng lần này là thư mục ***/root***. Thử liệt kê các file trong ***/root***:

    sudo ls /root;

![img](237)

=> Dùng lệnh sudo ***less /root/3rd.txt*** để đọc nội dung file này:

![img](238)

=> Tìm được nguyên liệu cuối.







