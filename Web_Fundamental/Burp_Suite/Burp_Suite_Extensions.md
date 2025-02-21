# Burp Suite: Extensions

***The Extensions Interface***

![img](69)

1. Extensions List: Box trên cùng hiển thị extensions list hiện được cài đặt trong Burp Suite cho project hiện tại. Cho phép active hoặc deactivate các extension riêng lẻ.

2. Managing Extensions: Phía bên trái có các tùy chọn để quản lý extensions: 
- Add: Sử dụng nút này để cài đặt các extensions mới từ các file trên disk. Các file này có thể là các module được mã hóa tùy chỉnh hoặc các module thu được từ các nguồn bên ngoài không có sẵn trong BApp Store.
- Remove: Nút này cho phép gỡ cài đặt extensions khỏi Burp Suite
- Up/Down: Các nút này kiểm soát thứ tự liệt kê các extension đã cài đặt. Thứ tự xác định trình tự các extensions được gọi khi xử lý lưu lượng. Các extension được áp dụng theo thứ tự giảm dần, bắt đầu từ đầu list và di chuyển xuống dưới. Thứ tự này rất cần thiết, đặc biệt là khi xử lý extensions sửa đổi requests, vì 1 số extension có thể xung đột hoặc can thiệp vào extension khác.

3. Details, Output và Errors: Ở phía dưới cùng, có các phần dành riêng cho extension được chọn: 
- Details: Phần này cung cấp thông tin về extensions đã chọn, ví dụ như tên, phiên bản và mô tả của nó.
- Output: Extensions có thể tạo output trong quá trình thực thi và phần này sẽ hiển thị mọi output hoặc kết quả có liên quan.
- Errors: Nếu extensions gặp bất kỳ lỗi nào trong quá trình thực thi, chúng sẽ được hiển thị trong phần này. Điều này có thể có ích trong debugging và troubleshooting lỗi của extensions.

Tóm lại, giao diện Extensions trong Burp Suite cho phép user quản lý và giám sát extensions đã cài đặt, active hoặc deactive chúng cho các project cụ thể và xem các chi tiết, output và error liên quan đến từng extension. Bằng cách sử dụng extensions, Burp Suite trở thành nền tảng mạnh mẽ và có thể tùy chỉnh cho các nhiệm vụ đánh giá web application và kiểm tra bảo mật khác nhau.

***The BApp Store***

Trong Burp Suite, BApp Store (Burp App Store) cho phép dễ dàng khám phá và tích hợp extensions một cách liền mạch vào tool. Extensions có thể được viết bằng nhiều ngôn ngữ khác nhau, trong đó Java và Python là những lựa chọn phổ biến nhất. Java Extensions tích hợp tự động với Burp Suite framework, trong khi Python Extensions yêu cầu trình thông dịch Jython.

Thử cài đặt extension ***Request Timer***, được viết bởi Nick Taylor. Extension Request Timer cho phép ghi lại thời gian cần thiết để mỗi request nhận được response.

![img](70)

![img](71)

Sau khi cài xong sẽ hiển thị trên thanh menu của Burp SUite: 

![img](72)

***Jython***

Để sử dụng các module Python trong Burp Suite, cần bao gồm file Jython Interpreter JAR, đây là 1 bản triển khai Java của Python. Jython Interpreter (Trình thông dịch Jython) cho phép chạy extensions dựa trên Python

![img](73)

![img](74)

***The Burp Suite API***

Trong Burp Suite Extensions module, cung cấp quyền truy cập vào nhiều API endpoints, cho phép tạo và tích hợp các module với Burp Suite. CÁc API này cung cấp nhiều chức năng khác nhau, cho phép mở rộng khả năng của Burp Suite để phù hợp với nhu cầu cụ thể.

Để xem API endpoints có sẵn, điều hướng đến tab phụ API trong module Extensions. Mỗi mục được liệt kê trong bảng bên trái đại diện cho 1 API endpoint khác nhau có thể được truy cập từ bên trong Extensions:

![img](75)




