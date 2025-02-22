# OWASP Juice Shop

Các lỗ hổng sẽ đề cập trong ***Juice Shop***: 

1. ***Injection***

2. ***Broken Authentication***

3. ***Sensitive Data Exposure***

4. ***Broken Access Control***

5. ***Cross-Site Scripting XSS***

***Task 2: Let's go on adventure!***

Truy cập Juice Shop 10.10.220.50:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image59.png?raw=true)

***Question 1: What’s the Administrator's email address?***

=> Thử click vào Apple Juice và xem phần review:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image60.png?raw=true)

=> Email của admin: ***admin@juice-sh.op***

***Question 2: What parameter is used for searching? ***

=> Thử click Icon tìm kiếm và tìm ***abc***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image61.png?raw=true)

=> Parameter dùng trong searching là ***q***

***Question 3: What show does Jim reference in his review?***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image62.png?raw=true)

=> Jim đề cập đến ***a replicator***.

Nếu search google ***“replicator”***, sẽ nhận được kết quả cho biết đó là từ một show có tên ***Star Trek***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image63.png?raw=true)

***Task 3: Inject the juice***

Có nhiều loại tấn công ***Injection***, trong số đó là: 

- ***SQL Injection***: SQL Insert là khi attacker nhập 1 truy vấn độc hại hoặc không đúng định dạng để truy xuất hoặc giả mạo data từ Database. Và trong 1 số trường hợp, sẽ login vào account.

- ***Command injection***: Command Injection là khi web application lấy user input và chạy chúng dưới dạng lệnh hệ thống. Attacker có thể giả mạo data này để thực thi các lệnh hệ thống của riêng chúng. Điều này có thể được thấy trong các ứng dụng thực hiện kiểm tra ping bị định cấu hình sai.

- ***Email injection***: Email Injection là 1 lỗ hổng cho phép user độc hại gửi mail mà không có sự cho phép trước của email server. Điều này xảy ra khi attacker thêm data bổ sung vào các trường nhưng server không giải thích chính xác

***Question 1: Log into the administrator account!***

Nhập username và password bất kỳ:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image64.png?raw=true)

Mở Burp Suite và Intercept login request: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image65.png?raw=true)

=> Params trong POST body ở dạng JSON. 

Send request này và nhận được response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image66.png?raw=true)

=> Thử sử dụng payload SQLi để bypass quá trình login. Thay đổi email thành ***‘ or 1=1–-***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image67.png?raw=true)

=> Send request và nhận được response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image68.png?raw=true)

=> Bypass thành công.

***Tại sao điều này lại xảy ra?***

***OR*** trong câu lệnh SQL sẽ trả về ***true*** nếu 1 trong 2 điều kiện đúng. Vì ***1=1*** luôn đúng nên toàn bộ mệnh đề đều đúng. Do đó, nó sẽ báo cho server rằng email hợp lệ và login vào account admin. Ký tự ***“--”*** sử dụng trong SQL để comment data, mọi hạn chế đối với thông tin login sẽ không còn hoạt động vì chúng được hiểu là comment.

=> Refresh lại page và đã login thành công vào tài khoản admin: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image69.png?raw=true)

***Question 2: Log into the Bender account***

Email của Bender sẽ có dạng như sau ***bender@juice-sh.op*** nhưng hiện tại chưa biết password. 

Login request trong Burp Suite: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image70.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image71.png?raw=true)

Tiếp tục sử dụng SQLi payload, do đã biết email nên không cần mệnh đề ***1=1***, chỉ cần thêm ***--*** để bypass hệ thống login. 

*(1=1 được dùng khi email hoặc username không xác định hoặc không hợp lệ)*

=> Thay email thành ***bender@juice-sh.op'--***, password nhập gì cũng được. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image72.png?raw=true)

=> Send request và xem response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image73.png?raw=true)

=> Login thành công, refresh lại page: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image74.png?raw=true)

***Task 4: Who broke my lock?!***

Trong nhiệm vụ này, xem xét việc khai thác ***authentication*** thông qua các lỗ hổng khác nhau. Khi nói về lỗ hổng trong authentication, đề cập đến các cơ chế dễ bị thao túng. Những cơ chế này có thể là:

- Mật khẩu yếu với tài khoản có đặc quyền cao

- Trang ***forgotten password***

***Question 1: Bruteforce the Administrator account’s password***

Ở phần trước, đã sử dụng ***SQL Injection*** để login vào tài khoản admin nhưng vẫn không biết password.

=> Thử ***brute-force attack***, send login request tới Intruder trong Burp:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image75.png?raw=true)

=> Chọn giá trị password là giá trị cần chèn payload. 

Đối với payload, sử dụng ***best1050.txt*** từ ***Seclists*** (có thể cài đặt bằng lệnh ***apt-get install seclists***).

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image76.png?raw=true)

=> Có thể tìm thấy wordlist ở ***/usr/share/wordlists/seclists/passwords/Common-Credentials/best1050.txt***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image77.png?raw=true)

=> Start attack!

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image78.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image79.png?raw=true)

=> Tìm ra password admin là ***admin123***

=> Login admin:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image80.png?raw=true)

***Question 2: Reset Jim’s password!***

Cơ chế reset password có thể bị lợi dụng! Khi nhập vào trường email trong trang Forgot Password, câu hỏi bảo mật của Jim được đặt thành ***“Your eldest siblings middle name?”***. Trong phần trên, phát hiện rằng jim có thể liên quan đến ***Star Trek***. Tìm kiếm ***Jim Star Trek*** trên google, cho 1 trang wiki về Jame T.Kirk từ Star Trek:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image81.png?raw=true)

Nhìn qua thông tin gia đình:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image82.png?raw=true)

=> Anh trai của Jim có tên đệm là ***Samuel***.

=> Nhập vào trang Forgot Password cho phép đổi mật khẩu của Jim.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image83.png?raw=true)

=> Đổi pass của jim thành 12345

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image84.png?raw=true)

***Task 5: AH! Don’t look!***

Web application phải lưu trữ và truyền dữ liệu nhạy cảm một cách an toàn và bảo mật. Nhưng trong một số trường hợp, lập trình viên có thể không bảo vệ dữ liệu nhạy cảm của họ một cách chính xác, khiến dữ liệu dễ bị tấn công. Hầu hết, bảo vệ dữ liệu không được áp dụng nhất quán trên ứng dụng web, khiến một số trang có thể truy cập dược public. Đôi khi, thông tin bị rò rỉ ra public mà lập trình viên không hề biết, khiến web app dễ bị tấn công.

***Question 1: Access the Confidential Document!***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image85.png?raw=true)

=> Điều hướng đến ***About us*** và di chuột qua phần ***"Check out our terms of use"***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image86.png?raw=true)

=> Thấy nó liên kết đến http://10.10.220.50/ftp/legal.md. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image87.png?raw=true)

=> Thử điều hướng đến thư mục ***ftp*** và thấy nó được public: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image88.png?raw=true)

=> Download file ***acacquis.md*** và lưu. Có vẻ như còn nhiều file khác nhưng chưa cần quan tâm. 

=> Về trang chủ.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image89.png?raw=true)

***Question 2: Log into MC SafeSearch’s account!***

Xem qua video: https://youtu.be/v59CX2DiX0Y

=> Sau khi xem, biết rằng password là ***“Mr.Noodles”*** nhưng đã thay ***"một số nguyên âm thành số 0”*** nghĩa là chỉ thay chữ ***o*** thành số ***0***.

=> Password của  ***mc.safesearch@juice-sh.op*** là ***“Mr. N00dles”***.

=> Login vào account: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image90.png?raw=true)

***Question 3: Download the Backup File!***

=> Trở lại http://10.10.220.50/ftp/ và tải xuống ***pack.json.bak***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image91.png?raw=true)

=> Gặp lỗi 403 vì chỉ cho phép tải tệp .md và .pdf

=> Sử dụng ***Poison Null Byte***.

- ***Poison Null Byte (%00)*** là một kỹ thuật dùng để ***"đánh lừa"*** server bằng cách ***chèn ký tự đặc biệt*** vào chuỗi.

=> Cách sử dụng Poison Null Byte:

- ***Mã hóa URL***: ***Poison Null Byte (%00)*** cần được mã hóa thành ***%2500*** để sử dụng trong URL.

- ***Bypass lỗi 403:*** Thêm ***%2500*** vào cuối URL, sau đó thêm ***.md*** để bypass lỗi "403 Forbidden".

Ví dụ: 

- URL gốc: https://example.com/file

- URL sau khi thêm Poison Null Byte: https://example.com/file%2500.md

=> Tại sao cách này hoạt động?

- Khi server đọc URL, nó gặp ký tự ***%00*** (Poison Null Byte) và dừng xử lý tại đó.

- Phần ***.md*** bị bỏ qua, giúp truy cập tệp mà không bị chặn bởi lỗi 403. Thêm ***.md*** để phòng khi server có filter, chỉ nhận request có chứa ***.md***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image92.png?raw=true)

***Task 6: Who’s flying this thing?***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image93.png?raw=true)

- ***Broken Access Control*** là lỗ hổng cho phép người dùng truy cập trái phép vào các phần của hệ thống mà họ không có quyền.

- Lỗ hổng này thường xảy ra khi hệ thống không kiểm soát chặt chẽ quyền truy cập của người dùng.

=> Phân loại lỗ hổng: 

- ***Horizontal Privilege Escalation (Leo thang đặc quyền ngang)***:

    - Xảy ra khi một người dùng có thể truy cập dữ liệu hoặc thực hiện hành động của người dùng khác ***cùng cấp quyền***.

    - Ví dụ: User A có thể xem thông tin cá nhân của User B mà không được phép.

- ***Vertical Privilege Escalation (Leo thang đặc quyền dọc)***:

    - Xảy ra khi một người dùng có thể truy cập dữ liệu hoặc thực hiện hành động của người dùng ***có quyền cao hơn***.

    - Ví dụ: Một người dùng thông thường có thể truy cập vào trang quản trị của admin.

***Question 1: Access the administration page!***

=> Mở Debugger trên firefox.

Sau đó refresh trang và tìm tệp JS ***main-es2015.js***. Truy cập file http://10.10.220.50/main-es2015.js:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image94.png?raw=true)

=> Tìm kiếm cụm từ ***admin***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image95.png?raw=true)

=> Ra 10 kết quả. Chú ý đến ***path:administration***. Đây có thể là endpoint của admin ***/administration***. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image96.png?raw=true)

=> Thử truy cập ***/administration***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image97.png?raw=true)

=> Lỗi 403 là do chưa login. Login vào account admin và truy cập lại ***/#/administration***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image98.png?raw=true)

***Question 2: View another user’s shopping basket!***

=> Login vào tài khoản admin và click “Your Basket”:

Trước khi click, mở Burp Suite và Intercept request này: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image99.png?raw=true)

=> Send request và xem response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image100.png?raw=true)

=> Nhận thấy trong GET request ***/rest/basket/1***, 1 có vẻ như là ID của ***user***. Thử thay 1 thành 2: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image101.png?raw=true)

=> Send request và xem response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image102.png?raw=true)

=> Có thể xem được giỏ hàng của user có ID là 2. 

=> Về trang chủ

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image103.png?raw=true)

***Question 3: Remove all 5-star reviews!***

=> Điều hướng đến http://10.10.250.50/#/administration và click vào icon thùng rác với mỗi review 5-star:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image104.png?raw=true)

=> Về trang chủ: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image105.png?raw=true)

***Task 7: Where did that come from?***

***XSS (Cross-Site Scripting) là gì?***

- ***XSS*** là lỗ hổng cho phép kẻ tấn công chèn và chạy mã JavaScript (JS) độc hại trên ứng dụng web.

- Đây là một trong những lỗ hổng phổ biến nhất, với mức độ phức tạp từ dễ đến khó tùy thuộc vào cách ứng dụng xử lý dữ liệu.

***3 loại tấn công XSS chính:***

1. ***DOM XSS:***

- ***Cách hoạt động***: Sử dụng môi trường HTML để thực thi mã JS độc hại.

- ***Ví dụ***: Kẻ tấn công chèn thẻ ***< script >*** vào trang web.

- ***Đặc điểm***: Xảy ra hoàn toàn trên trình duyệt (client-side), không liên quan đến server.

2. ***Persistent XSS (Lưu trữ)***:

- ***Cách hoạt động***: Mã JS độc hại được lưu trữ trên server và chạy khi người dùng truy cập trang web chứa mã đó.

- ***Ví dụ***:  Chèn mã độc vào bài đăng trên blog hoặc bình luận.

- ***Đặc điểm***: Ảnh hưởng đến nhiều người dùng vì mã độc được lưu trữ lâu dài.

3. ***Reflected XSS (Phản xạ)***: 

- ***Cách hoạt động***: Mã JS độc hại được phản ánh lại từ server ngay lập tức, thường thông qua các tham số URL hoặc form nhập liệu.

- ***Ví dụ***: Chèn mã độc vào thanh tìm kiếm, và kết quả tìm kiếm hiển thị lại mã độc.

- ***Đặc điểm:*** Xảy ra khi server không kiểm tra hoặc làm sạch dữ liệu đầu vào.

***Question 1: Perform a DOM XSS!***

=> Sử dụng phần tử ***iframe*** với thẻ ***< alert >***:

    <iframe src="javascript:alert(`xss`)"> 

Việc chèn payload này vào thanh tìm kiếm sẽ kích hoạt hàm alert(): 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image106.png?raw=true)

Loại XSS này được gọi là ***XFS (Cross-Frame Scripting)***, 1 trong những hình thức phát hiện XSS phổ biến nhất trong ứng dụng web. Các website cho phép user sửa đổi frame hoặc các thành phần DOM khác rất có thể dễ bị XSS tấn công.

=> Tại sao lại hoạt động? 

Thông thường, thanh tìm kiếm sẽ gửi request đến server, sau đó nó sẽ gửi lại thông tin tìm kiếm liên quan, đây chính là nơi có lỗ hổng. Nếu không vệ sinh input chính xác, tin tặc có thể thực hiện tấn công XSS nhắm vào thanh tìm kiếm.

=> Về trang chủ: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image107.png?raw=true)

***Question 2: Perform a persistent XSS!***

=> Login vào tài khoản admin. 

Lần này quan tâm đến ***địa chỉ IP cuối cùng đăng nhập trang***.

Mở Burp Suite và Intercept logout request: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image108.png?raw=true)

=> Thêm request header ***True-Client-IP*** với value là 1 XSS payload: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image109.png?raw=true)

=> Request lúc này: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image110.png?raw=true)

=> Send request và xem response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image111.png?raw=true)

Khi Login lại admin account và điều hướng đến trang ***last login ip*** sẽ có thông báo: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image112.png?raw=true)

=> Tại sao lại hoạt động? 

***True-Client-IP*** header tương tự như ***X-Forwarded-For*** header, cả 2 đều cho server hoặc proxy biết IP của client là gì. 

Nếu server không kiểm tra hoặc làm sạch giá trị request header, nó có thể hiển thị giá trị payload chèn vào trực tiếp trên web. Việc chèn mã độc vào logout request có thể khiến người dùng vô tình thực thi mã độc khi họ đăng xuất.

=> Về trang chủ: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image113.png?raw=true)

***Question 3: Perform a reflected XSS!***

Đầu tiên, cần vào đúng trang để thực hiện ***Reflected XSS***.

=> Login tài khoản admin và điều hướng đến trang ***Order History***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image114.png?raw=true)

Có một icon hình xe tải, nhấp vào sẽ được chuyển tiếp đến trang ***track-result***. Phát hiện 1 id được ghép nối với order: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image115.png?raw=true)

Thử thay id này thành:

    <iframe src="javascript:alert(`xss`)">

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image116.png?raw=true)

=> Refresh trang: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image117.png?raw=true)

=> Về trang chủ: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Web_Hacking_Fundamentals/images/image118.png?raw=true)

















































