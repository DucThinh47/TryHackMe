# SQL Injection

***Database là gì?***

Cơ sở dữ liệu là 1 cách lưu trữ điện tử các bộ sưu tập dữ liệu 1 cách có tổ chức. DB được điều khiển bởi ***DBMS***, là từ viết tắt của ***Database Management System***. DBMSs chia làm 2 loại: ***Relational*** (Quan hệ) và ***Non-Relational*** (Không quan hệ).

Trong DBMS, có thể có nhiều DB, mỗi DB chứa tập data riêng. Ví dụ, có DB tên là “shop”. Trong DB này, lưu trữ thông tin về các sản phẩm có sẵn để mua, User đã đăng ký cửa hàng trực tuyến và thông tin về các đơn hàng đã nhận được. Lưu thông tin này 1 cách riêng biệt trong DB bằng cách sử dụng ***tables***.

![img](148)

***Tables là gì?***

![img](149)

***Relational Vs Non-Relational Databases:***

***Relational DB*** lưu trữ thông tin trong các tables và thông thường, các tables chia sẻ thông tin giữa chúng; chúng sử dụng columns để chỉ định và xác định data đang được lưu trữ và các row thực sự để lưu trữ data. Tables thường chứa 1 cột ***ID*** (Primary Key), được sử dụng trong các tables khác để tham chiếu đến nó và tạo mối quan hệ giữa các tables, do đó có tên Relational Table.

***Non-relational DB***, đôi khi được gọi là ***NoSQL***, là bất kỳ loại DB nào không sử dụng tables, columns và rows để lưu trữ data. Không cần phải xây dựng bố cục DB cụ thể để mỗi row có thể chứa thông tin khác nhau, mang lại sự linh hoạt hơn. Một số DB phổ biến thuộc loại này ***MongoDB***, ***Cassandra*** và ***ElasticSearch***.

***SQL là gì?***

SQL (Structured Query Language), là ngôn ngữ giàu tính năng được dùng để truy vấn DB. Những truy vấn SQL này được gọi là câu lệnh (***query string***).

1 số commands đơn giản nhất là ***SELECT, UPDATE, INSERT*** và ***DELETE*** dữ liệu. Mặc dù hơi giống nhau, nhưng 1 số DB server có cú pháp riêng và những thay đổi nhỏ.

***What is SQL Injection?***

SQL Injection xảy ra khi data do User cung cấp được đưa vào truy vấn SQL.

Xem xét tình huống, sau khi ta bắt gặp 1 blog online và mỗi mục blog có ***1 số ID duy nhất***. Các mục blog có thể được đặt ở chế độ public hoặc private. URL cho mỗi mục blog có thể trông như sau: 
https://website.thm/blog?id=1

Từ URL trên, ta thấy rằng mục blog được chọn xuất phát từ tham số ***id***. Web Application cần truy xuất bài viết từ DB và có thể sử dụng câu lệnh SQL trông giống như sau: 

	SELECT * from blog where id=1 and private=0 LIMIT 1;

Câu lệnh SQL trên đang tìm kiếm trong table tên là blog 1 bài viết có ***column id=1*** và ***column private=0***, nghĩa là nó có thể để public và giới hạn chỉ 1 kết quả.

Như đã đề cập, SQL Injection xảy ra khi User Input Data được đưa vào truy vấn DB. Trong TH này, tham số id từ chuỗi truy vấn được sử dụng trực tiếp trong truy vấn SQL.

Giả sử bài viết có ID là 2 vẫn bị khóa ở chế độ riêng tư, không xem được trên website. Bây giờ, ta thử gọi URL:
	https://website.thm/blog?id=2;--

Tương đương với câu lệnh SQL:

	SELECT * from blog where id=2;-- and private=0 LIMIT 1;

Dấu ***“;”*** trong URL biểu thị sự kết thúc của câu lệnh SQL và dấu ***“--”*** khiến mọi thứ sau nó được coi là comments. Nghĩa là, thực tế, ta đang chạy truy vấn: 

	SELECT * from blog where id=2;--

Truy vấn sẽ trả về bài viết có id=2 dù nó được đặt ở chế độ public hay private.

***In-Band SQL Injection***

***In-Band SQLi*** là loại SQL Injection dễ phát hiện và khai thác nhất; In-band chỉ đề cập đến cùng 1 phương thức giao tiếp đang được sử dụng để khai thác lỗ hổng và cũng nhận được kết quả, ví dụ như phát hiện lỗ hổng SQL Injection trên 1 website và sau đó có thể trích xuất data từ DB vào cùng 1 page.

***Error-Based SQL Injection***

Kiểu SQL Injection này là cách hữu ích nhất để lấy thông tin về cấu trúc DB, vì các thông báo lỗi từ DB được in trực tiếp lên màn hình browser. Điều này thường được dùng để liệt kê toàn bộ DB. 

***Union-Based SQL Injection***

Kiểu SQL Injection này sử dụng toán tử SQL UNION cùng với lệnh SELECT để trả về kết quả bổ sung cho page. Phương pháp này là phổ biến nhất để trích xuất lượng lớn data thông qua lỗ hổng SQL Injection.

***Practical***

![img](150)

Thử thay đổi id=1 trong URL thành id=0 và thêm phần sau:  

    UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'

Phương thức ***group_concat()*** lấy column được chỉ định (TH này là ***table_name***) từ nhiều row được trả về và đặt nó vào 1 chuỗi được phân tách bằng dấu ***“,”***. DB ***information_schema***: mọi user đều có quyền truy cập vào DB này, nó chứa thông tin về tất cả DB và table mà user có quyền truy cập.

![img](151)

Mục tiêu là mật khẩu của Martin 

=> Cần nghiên cứu tabe ***staff_users***

Thay đổi truy vấn thành:

    0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'

=> Lệnh đã trả về 3 cột cho bảng staff_users: ***id***, ***username*** và ***password***.

=> Tiếp theo, thay đổi lệnh thành:

    0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users

Tương tự, sử dụng ***group_concat*** để trả về tất cả row thành dạng 1 string và khiến nó dễ đọc hơn.

![img](152)

=> Tìm ra mật khẩu của Martin!

***Blind SQLi-Authentication Bypass***

Không như In_Band SQL Injection, nơi ta có thể xem kết quả tấn công trực tiếp ngay trên màn hình, Blind SQLi là khi ta nhận được rất ít hoặc không có phản hồi nào để xác nhận xem các truy vấn được chèn trên thực tế có thành công hay không, điều này do thông báo lỗi đã bị vô hiệu hóa. 

***Authentication Bypass***

Một trong những kỹ thuật Blind SQL Injection đơn giản nhất là bypassing các phương thức authentication ví dụ như Login forms. Trong TH này, ta quan tâm đến việc truy xuất data từ DB, để bypass việc Login.

Login Forms được kết nối với DB của user thường được phát triển theo cách Web application sẽ không quan tâm đến nội dung của username và password, mà chỉ quan tâm nhiều hơn đến việc liệu cả 2 có tạo thành 1 cặp khớp với trong user table hay không. Đơn giản hơn, Web Application sẽ hỏi DB “Bạn có user có username là bob và password là bob123 không?”, DB trả lời có hoặc không, tùy thuộc vào điều đó, Web Application sẽ cho phép ta tiếp tục hay không.

Ví dụ, đây là câu query ban đầu để kiểm tra việc Login: 

    select * from users where username='' and password='' LIMIT 1;

Trong Login Form, có thể nhập ***‘ OR 1=1;--*** vào trường password: 

Khi đó query trở thành: 

    select * from users where username='' and password='’ OR 1=1;-- ' LIMIT 1;

Vì 1=1 là lệnh luôn đúng và ta đã dùng toán tử OR, điều này sẽ khiến truy vấn trả về là đúng.

***Blind SQLi - Boolean Based***

Boolean-Based SQL Injection đề cập đến phản hồi mà ta nhận được từ các lần thử chèn, có thể là đúng / sai, có / không, bật / tắt, 1/ 0 hoặc bất kỳ phản hồi nào chỉ có hai kết quả. Kết quả đó xác nhận rằng SQL Injection payload của ta có thành công hay không. 

***Blind SQLi - Time Based***

Time-Based Blind SQL Injection rất giống phương pháp Boolean-Based ở chỗ các yêu cầu tương tự được gửi đi nhưng có chỉ bảo trực quan nào cho thấy truy vấn của ta sai hay đúng vào thời điểm này. Thay vào đó, chỉ bảo truy vấn chính xác của ta dựa trên thời gian hoàn thành truy vấn đó. Độ trễ thời gian này được đưa ra bằng cách sử dụng các phương thức tích hợp sẵn như SLEEP(x) cùng với câu lệnh UNION. Phương thức SLEEP() sẽ chỉ thực thi khi câu lệnh UNION SELECT thành công.

***Out-of-Band SQLi***

Out-of-Band SQLi không phổ biến vì nó phụ thuộc vào các tính năng cụ thể được kích hoạt trên DB server hoặc logic nghiệp vụ của Web Application, điều này thực hiện 1 số lệnh gọi mạng bên ngoài dựa trên kết quả từ truy vấn SQL.

Một cuộc tấn công Out-Of-Band được phân loại bằng cách có 2 kênh liên lạc khác nhau, 1 kênh để phát động cuộc tấn công và kênh kia để thu thập kết quả. Ví dụ, kênh tấn công có thể là 1 web request, và kênh thu thập kết quả có thể đang giám sát các yêu cầu HTTP / DNS được gửi tới dịch vụ mà ta kiểm soát.

1. Attacker đưa ra request tới 1 website dễ bị tấn công SQL Injection bằng 1 payload đã bị tiêm mã.

2. Website thực hiện 1 truy vấn SQL tới DB, truy vấn này cũng chuyển payload bị tiêm của attacker.

3. Payload này buộc yêu cầu HTTP quay trở lại máy của attacker chứa dữ liệu từ DB

![img](153)

***Giải pháp***

***Prepared Statements (with Parameterized Queries)***

Trong truy vấn đã chuẩn bị, điều đầu tiên developer viết là truy vấn SQL, sau đó mọi thông tin input của User sẽ được thêm dưới dạng tham số sau đó. Việc viết tất cả câu lệnh đã chuẩn bị sẵn để đảm bảo cấu trúc mã SQL không thay đổi và DB có thể phân biệt giữa truy vấn và dữ liệu. 

***Input Validation:***

Xác thực Input có thể giúp ích trong việc bảo vệ những gì được đưa vào truy vấn SQL. Việc sử dụng danh sách cho phép có thể hạn chế input chỉ ở 1 số chuỗi nhất định hoặc phương pháp thay thế chuỗi trong ngôn ngữ lập trình có thể lọc các ký tự ta muốn cho phép hoặc không.

***Escaping User Input:***

Việc cho phép User nhập data có chứa ký tự như ‘ “ $ \ có thể khiến truy vấn SQL bị hỏng hoặc tệ hơn, tạo cơ hội cho SQL Injection. Thoát khỏi dữ liệu nhập của User là phương pháp thêm dấu gạch chéo ngược \ vào trước các ký tự này, sau đó khiến chúng được phân tích cú pháp giống như một chuỗi thông thường chứ không phải ký tự đặc biệt























