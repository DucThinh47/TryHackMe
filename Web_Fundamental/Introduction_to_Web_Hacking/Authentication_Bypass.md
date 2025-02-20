# Authentication Bypass

***Username Enumeration***

Thông báo lỗi website là nguồn tài nguyên hữu ích, giúp đối chiếu thông tin, nhằm xây dựng danh sách username hợp lệ. Đây là form tạo account của website Acme IT Support:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image45.png?raw=true)

Nếu thử nhập username là ***admin*** và điền các field khác là các thông tin giả mạo, sẽ có 1 thông báo là ***An account with this username already exists***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image46.png?raw=true)

=> Sử dụng ***ffuf***: 

***ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.87.147/customers/signup -mr "username already exists"***

=> Câu lệnh này thực hiện 1 cuộc tấn công fuzzing nhằm tìm kiếm các valid value cho field username trên endpoint ***/signup***.

Phân tích lệnh:

***-w /usr/share/wordlists/SecLists/Usernames/Names/names.txt:***: 

- Chỉ định wordlist được sử dụng để fuzz

- List này sẽ chứa các tên username phổ biến từ kho SecLists

***X POST***:

- Chỉ định HTTP protocol là POST

***-d "username=FUZZ&email=x&password=x&cpassword=x"***:

- Dữ liệu của ***POST request*** được gửi đến server

- ***FUZZ*** là placeholder mà ffuf sẽ thay thế bằng các username từ wordlist

- Các field email, password, cpassword được đặt value cố định là x

***-H "Content-Type: application/x-www-form-urlencoded"***:

- Thêm header ***Content-Type***, chỉ định loại data trong request là application/x-www-form-urlencoded. (để server-side biết là client-side đang gửi form)

***-u http://10.10.87.147/customers/signup***:

- Chỉ định URL để gửi fuzz request

***-mr "username already exists":***

- Chỉ định 1 ***biểu thức regex*** để tìm kiếm trong response từ server

- Nếu response chứa cụm ***“username already exists”***, ffuf sẽ đánh dấu request đó là hợp lệ

=> Sau khi quét xong, lưu kết quả vào file ***valid_username.txt***. Các username phát hiện được là: *admin*, *robert*, *simon* và *steve*.

***Brute Force***

Sử dụng file ***valid_usernames.txt***, thực hiện 1 cuộc tấn công brute force trên trang login (http://10.10.87.147/customers/login). 

Sử dụng lệnh:

***ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.87.147/customers/login -fc 200***

=> Lệnh này sử dụng ffuf tool để thực hiện tấn công brute force vào endpoint ***/customers/login*** tại URL http://10.10.87.147/customers/login. Mục tiêu là dò tìm cặp username và password hợp lệ bằng cách kết hợp 2 wordlists: 1 cho username và 1 cho password.

Phân tích lệnh: 

***-w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2***

- ***-w***: chỉ định các wordlists để fuzz

- ***valid_usernames.txt***: Chứa list username

- ***10-million-password-list-top-100.txt***: chứa list password phổ biến

- ***:W1 và :W2***: set label để tham chiếu 2 wordlist. W1 để đại diện cho list username, W2 đại diện cho list password

***-X POST:***

- Chỉ định HTTP method là POST

***-d “username=W1&password=W2”:***

- Đây là payload của POST request gửi đến server

- W1 và W2 sẽ được thay thế lần lượt bởi các giá trị từ list username và password

***-H “Content-Type: application/x-www-form-urlencoded”:***

- Thêm Header chỉ định loại data gửi đi là ***application/x-www-form-urlencoded***

***-u http://10.10.87.147/customers/login:***

- Chỉ định endpoint là ***/customer/login***

***-fc 200:***

- -fc (Filter Code): Bộ lọc mã trạng thái HTTP

- Loại bỏ các phản hồi có mã trạng thái là 200 (thành công). Điều này có nghĩa là chỉ quan tâm đến các phản hồi không phải 200, vì phản hồi 200 thường cho biết đăng nhập thất bại (ví dụ: hiển thị thông báo lỗi).

=> Kết quả: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image47.png?raw=true)

***Logic Flaw***

Logic Flaw Example:

Mã bên dưới mô phỏng việc kiểm tra liệu điểm bắt đầu của path mà customer đang truy cập có bắt đầu bằng ***/admin*** không, nếu có thì các bước kiểm tra tiếp theo sẽ được thực hiện để xem liệu thực tế, customer có phải admin hay không. Nếu page không bắt đầu bằng ***/admin***, page sẽ được hiển thị cho client:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image48.png?raw=true)

Vì mã PHP trên sử dụng dấu ***“===”*** nên nó đang tìm kiếm 1 kết quả khớp chính xác chuỗi ***“admin”***, bao gồm cả cách viết hoa giống nhau. Code này có 1 logic flaw (lỗ hổng logic) vì nếu request path của user chưa được xác thực là ***/adMin*** thì sẽ họ sẽ không được kiểm tra đặc quyền và page sẽ được show cho họ, hoàn toàn bỏ qua việc xác thực.

Logic Flaw Practical:

Kiểm tra chức năng ***Reset Password*** của website Acme IT  Support (http://10.10.87.147/customers/reset). 1 form yêu cầu email được liên kết với account muốn reset password. Nếu 1 email không hợp lệ được nhập, sẽ nhận được error message ***“Account not found from supplied email address”***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image49.png?raw=true)

Sử dụng email ***robert@acmeitsupport.thm*** hợp lệ, sau đó form tiếp theo được hiển thị, yêu cầu điền username được liên kết với email này. Nếu nhập ***robert*** làm username và click ***Check Username*** button, sẽ nhận được thông báo xác nhận 1 email xác nhận reset password đã được gửi đến ***robert@acmeitsupport.thm***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image50.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image51.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image52.png?raw=true)

Lúc này, câu hỏi là ***lỗ hổng có thể xảy ra trong web application này là gì?***, vì cần phải biết cả email và username, sau đó liên kết reset password mới được gửi đến email của chủ sở hữu account. 

Trong bước thứ hai của quy trình nhận reset password email, username được gửi trong ***POST field*** tới web server, và email được gửi trong ***query string request*** dưới dạng GET field.

Thử sử dụng **curl** để thực hiện request tới server để minh họa điều này: 

***curl 'http://10.10.87.147/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image53.png?raw=true)

Trong web app này, thông tin tài khoản người dùng được lấy thông qua ***query string*** (dữ liệu truyền qua URL). Tuy nhiên, ở phần logic xử lý tiếp theo, email reset password lại được gửi dựa trên dữ liệu từ biến ***PHP $_REQUEST***.

Biến ***$_REQUEST*** trong PHP là một mảng chứa dữ liệu nhận được từ cả ***query string*** (dữ liệu trong URL) và ***POST data*** (dữ liệu gửi qua form). Nếu cùng một tên key (ví dụ: email) được sử dụng trong cả query string và POST data, logic của ứng dụng sẽ ưu tiên sử dụng giá trị từ POST data thay vì query string.

Điều này có nghĩa là nếu thêm một tham số mới vào form POST (ví dụ: ***email=attacker@example.com***), có thể kiểm soát địa chỉ email mà hệ thống sẽ gửi link reset mật khẩu đến. Đây có thể là một lỗ hổng bảo mật nghiêm trọng, vì kẻ tấn công có thể lợi dụng để chuyển hướng email reset mật khẩu đến địa chỉ email của họ.

Tạo 1 account có email là ***thinh1@customer.acmeitsupport.thm***

=> Thay thế lệnh ***curl*** thành: 

***curl 'http://10.10.87.147/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=thinh1@customer.acmeitsupport.thm'***

=> Lúc này ***Email reset password*** sẽ được gửi đến tài khoản có email là ***thinh1@customer.acmeitsupport.thm*** nhưng với username là ***robert***

Truy cập account có email là ***thinh1@customer.acmeitsupport.thm*** để xem email reset password có được gửi đến không: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image54.png?raw=true)

=> Truy cập link: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image55.png?raw=true)

***Cookie Tampering***

Việc kiểm tra và chỉnh sửa cookie trong một ***phiên làm việc trực tuyến*** (online session) có thể dẫn đến nhiều hậu quả nghiêm trọng về bảo mật. Cookie là những tệp dữ liệu nhỏ được web server gửi đến trình duyệt của người dùng và được lưu trữ trên máy tính của họ. Chúng thường chứa thông tin quan trọng như session ID, thông tin đăng nhập, hoặc các dữ liệu khác mà server sử dụng để nhận diện và xác thực người dùng.

Khi một người dùng hoặc kẻ tấn công kiểm tra và chỉnh sửa các giá trị trong cookie, họ có thể thay đổi cách hệ thống nhận diện và xử lý phiên làm việc của họ.

***Plain Text***

Nội dung của 1 số cookie có thể ở dạng ***plain text*** và chức năng của chúng rất rõ ràng. Ví dụ, nếu đây là cookie được web server set sau khi login thành công: 

***Set-Cookie: logged_in=true; Max-Age=3600; Path=/***

***Set-Cookie: admin=false; Max-Age=3600; Path=/***

Có 1 cookie ***logged_in***, xuất hiện để kiểm soát xem user hiện có login hay không? và 1 cookie khác ***admin***, kiểm soát xem user truy cập có quyền admin không. Sử dụng logic này, nếu thay đổi nội dung của cookie và gửi request, có thể thay đổi các đặc quyền!

Đầu tiên, bắt đầu bằng cách gửi request đến mục tiêu, sử dụng lệnh: 

***curl http://10.10.41.189/cookie_test***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image56.png?raw=true)

=> Lúc này returned message sẽ là ***Not Logged In***

Thử gửi 1 request khác với cookie ***logged_in*** được set thành ***true*** và cookie ***admin*** set thành ***false***:

***curl -H "Cookie: logged_in=true; admin=false" http://10.10.41.189/cookie-test***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image57.png?raw=true)

=> Nhận được message: ***Logged In As A User***

=> Cuối cùng, gửi 1 request với cookie ***logged_in*** và cookie ***admin*** đều set thành ***true***:

***curl -H "Cookie: logged_in=true; admin=true" http://10.10.41.189/cookie-test***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image58.png?raw=true)

=> Return message sẽ là ***Logged In As An Admin***.

***Hashing***

Đôi khi, ***cookie value*** có thể trông giống như 1 chuỗi dài các ký tự random, chúng được gọi là ***hashes*** (giá trị băm); là 1 cách biểu diễn ***không thể đảo ngược*** của văn bản gốc. Dưới đây là 1 số ví dụ:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image59.png?raw=true)

Từ bảng trên, có thể thấy hash output từ cùng 1 input string có thể khác nhau đáng kể tùy thuộc vào hash method được sử dụng.

Mặc dù hash không thể đảo ngược nhưng ***output giống nhau*** vẫn được tạo ra mọi lúc, điều này rất hữu ích khi các dịch vụ như https://crackstation.net/ lưu giữ cơ sở dữ liệu của hàng tỷ hàm băm và chuỗi gốc của chúng.

***Encoding***

Encoding (Mã hóa) tương tự như hashing ở chỗ nó tạo ra những gì trông như 1 random string of text, nhưng trên thực tế, encoding có thể đảo ngược. Vì vậy, câu hỏi được đặt ra, encoding để làm gì? Encoding cho phép chuyển đổi binary data thành text mà con người có thể đọc được, có thể truyền dễ dàng và an toàn qua các phương tiện chỉ hỗ trợ plain text ASCII characters.

Các loại encoding phổ biến là ***base32***, sẽ chuyển đổi binary data thành các ký tự ***A-Z*** và ***2-7***; ***base64***, chuyển đổi bằng cách sử dụng các ký tự ***a-z***, ***A-Z***, ***0-9***, ***+***, ***/*** và dấu ***“=”*** cho phần đệm. 

Lấy ví dụ, cookie được web server set khi login: 

***Set-Cookie: session=eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ==; Max-Age=3600; Path=/***

Chuỗi session khi sử dụng ***base64*** decoded sẽ có giá trị là ***{“id”:1, “admin”:false}***, có thể mã hóa lại bằng base64 nhưng lần này set admin là ***true***, điều đó giúp cấp quyền truy cập của admin.























