# IDOR

***IDOR*** là viết tắt của ***Insecure Direct Object Reference*** (Tham chiếu trực tiếp đối tượng không an toàn), một loại lỗ hổng liên quan đến access control. 
Loại lỗ hổng này xảy ra khi web server nhận input data do user cung cấp để truy xuất các object (files, data, documents), quá nhiều sự tin tưởng được đặt vào input data và nó không được xác thực ở phía server để xác nhận đối tượng được request có thuộc về user request nó không.

***An IDOR Example***

Khi cần thay đổi hồ sơ, truy cập ***http://online-service.thm/profile?user_id=1305***, nếu thay đổi giá trị ***user_id*** thành 1000, khi đó URL là ***http://online-service.thm/profile?user_id=1000***, điều này dẫn đến việc xem thông tin của user khác. Đây là lỗ hổng ***IDOR***! Vì vậy, nên kiểm tra trên website để xác nhận rằng thông tin user thuộc về user đã gửi request login.

***Finding IDORs Encoded IDs***

***Encoded IDs:***

![img](60)

***Finding IDORs in Hashed IDs***

***Hashed IDs***

***Hashed IDs*** sẽ phức tạp hơn 1 chút so với IDs được encode, nhưng chúng có thể tuân theo 1 mẫu có thể dự đoán được, ví dụ như phiên bản hashed của giá trị integer. Ví dụ: id ***123*** sẽ trở thành ***202cb962ac59075b964b07152d234b70*** nếu ***md5*** hashing được dùng.

***Finding IDORs in Unpredictable IDs***

***Unpredictable IDs***

Nếu không thể phát hiện ID bằng các phương pháp trên thì phương pháp phát hiện IDOR hữu ích là tạo 2 account và hoán đổi ID giữa chúng. Nếu có thể xem thông tin của user khác bằng cách sử dụng ID của họ trong khi vẫn login bằng account khác (hoặc hoàn toàn không login) nghĩa là đã tìm thấy lỗ hổng IDOR!

***Where are IDORs located?***

Đôi khi, endpoint có thể có 1 tham số không được tham chiếu, được sử dụng trong quá trình phát triển và được đưa vào sản xuất. Ví dụ, lệnh call tới ***/user/details*** sẽ hiển thị thông tin user (được xác thực thông qua session). Nhưng thông qua 1 cuộc tấn công gọi là khai thác tham số, phát hiện ra 1 tham số là ***user_id***, có thể sử dụng để hiển thị thông tin của user khác, ví dụ ***/user/details?user_id=123***.

***A practical IDOR Example***

Thử tạo 1 account và login account trong Acme IT Support website: 

![img](61)

Để ý thấy khi click tab Your Account, thông tin trong field ***username*** và ***email*** đã được fill trước. 

=> Tìm hiểu tại sao những field này lại được pre-filled?

=> Điều này có thể liên quan đến session-cookie, thử Inspect page và click tab Network, refresh lại page, sẽ thấy 1 lệnh call tới endpoint ***/api/v1/customer?id={user_id}***:

![img](62)

=> Mở Burp Suite và Intercept request khi refresh lại page và thay đổi ***user_id*** thành 1: : 

![img](63)

=> Thấy được username của user khác là ***adam84*** và email: ***adam-84@fakemail.thm***.

=> Thử thay đổi user_id thành 3:

![img](64)

=> Thấy được username của user khác là ***john911*** và email: ***j@fakemail.thm***








