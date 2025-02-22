# Pickle Rick

***Question 1: Nguyên liệu đầu tiên mà Rick cần là gì?***

=> Truy cập website: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image214.png?raw=true)

=> Xem source page:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image215.png?raw=true)

=> Tìm được một username: ***R1ckRul3s***

=> Thử sử dụng ***nmap*** kiểm tra các cổng mở trên web

- Thêm tùy chọn ***-sV***: Thu thập thông tin service đang chạy trên các port.

- Thêm tùy chọn ***-sC***: Chạy các script mặc định để thu thập thêm thông tin chi tiết.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image216.png?raw=true)

=> Xem file ***robots.txt***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image217.png?raw=true)

=> Tìm được chuỗi ***Wubbalubbadubdub***.

=> Thử truy cập ***login.php*** xem website có trang login viết bằng PHP không:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image218.png?raw=true)

=> Thử login với ***username=R1ckRul3s*** và ***password=Wubbalubbadubdub***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image219.png?raw=true)

=> Login thành công, điều hướng đến ***/portal.php***. 

=> Thử nhập ***ls;***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image220.png?raw=true)

=> Trong Burp Suite, Intercept request khi nhập ***ls;***:

![img][https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image221.png?raw=true]

=> Tìm được một chuỗi base64, thử giải mã base64 -> base64 -> base64... cho đến khi thu được: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image222.png?raw=true)

=> Thu được chuỗi ***rabbit hole***.

=> Thử đọc file có tên khả nghi: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image223.png?raw=true)

=> Không đọc được. Thử truy cập file .txt này thẳng trên URL: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image224.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image225.png?raw=true)

=> Thu được chuỗi ***mr.meeseek hair***

=> Hoặc dùng lệnh ***less*** thay ***cat*** để đọc file trực tiếp: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image226.png?raw=true)

=> less so với cat:

- less cho phép scroll page, cat in toàn bộ nội dung 1 lần
dễ sử dụng và hiệu quả, đặc biệt với file lớn

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image227.png?raw=true)

***Question 2: What is the second ingredient in Rick’s potion?***

=> Thử đọc file clue.txt: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image228.png?raw=true)

=> Hệ thống gợi ý là tìm trong file system.

=> Thử nhập lệnh ***cd /home;pwd;ls;***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image229.png?raw=true)

=> Tiếp tục thử truy cập thư mục ***risk***:

    cd /home/rick;pwd;ls;

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image230.png?raw=true)

=> Tiếp tục thử truy cập thư mục second ingredients: 

    cd /home/rick/second ingredients;pwd;ls;

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image231.png?raw=true)

=> không truy cập được

=> Có vẻ second ingredients không phải 1 thư mục.

=> Thử lệnh ***cat /home/rick/second ingredients;***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image232.png?raw=true)

=> không xem được.

=> Thử lệnh ***less /home/rick/second ingredients;***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image233.png?raw=true)

Thử xác định second ingredients là 1 string do tên file second ingredients chứa khoảng trắng. Nếu không đặt trong dấu nháy, shell sẽ hiểu second và ingredients là hai file riêng biệt, dẫn đến lỗi. Dùng lệnh:

    cat /home/rick/’second ingredients’;

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image234.png?raw=true)

=> Không được. Thử lệnh ***less /home/rick/’second ingredients’;***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image235.png?raw=true)

=> Tìm được chuỗi ***1 jerry tear***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image236.png?raw=true)

***Question 3: What is the last and final ingredient?***

Tiếp tục dựa vào gợi ý từ file clue.txt, tìm kiếm trong thư mục hệ thống.

=> Ý tưởng lần này là thư mục ***/root***. Thử liệt kê các file trong ***/root***:

    sudo ls /root;

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image237.png?raw=true)

=> Dùng lệnh sudo ***less /root/3rd.txt*** để đọc nội dung file này:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image238.png?raw=true)

=> Tìm được nguyên liệu cuối.







