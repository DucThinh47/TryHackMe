# HTTP in Details

1. On the mock webpage on the right there is an issue, once you've found it, click on it. What is the challenge flag? 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image4.png?raw=true)

=> Website đang sử dụng **HTTP** chứ không phải **HTTPs**, click vào icon tượng trưng cho việc không sử dụng **HTTPs** trên thanh URL: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image5.png?raw=true)

**HTTPs** (HyperText Transfer Protocol Secure), phiên bản bảo mật hơn của **HTTP**. Dữ liệu của **HTTPs** được **mã hóa**, giúp bảo vệ dữ liệu khi đang truyền hoặc đang nhận, đồng thời đảm bảo việc **giao tiếp với đúng web server**, chứ không phải với một server mạo danh. 

2. Make a GET request to /room

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image6.png?raw=true)

Phân tích request:

- ***GET /room HTTP/1.1***: Phương thức HTTP được phía Client sử dụng, giúp yêu cầu dữ liệu từ server (ở đây yêu cầu server trả về nội dung trang **/room**). Đồng thời thông báo cho server rằng đang sử dụng giao thức HTTP có version 1.1

- ***Host:tryhackme.com***: thông báo cho server rằng yêu cầu đang muốn truy cập website **tryhackme.com**

- ***User-Agent:Mozilla/5.0 Firefox/87.0***: thông báo cho server rằng máy client đang sử dụng trình duyệt Firefox version 87.0

- ***Content-Length:0***: thông báo cho server biết lượng data mong đợi trong request. Bằng cách này, web server có thể đảm bảo nó không thiếu bất kỳ data nào

=> Response được trả về: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image7.png?raw=true)

Phân tích response: 

- ***HTTP/1.1 200 Ok***: server thông báo HTTP version và status code 200-OK, nghĩa là request đã được gửi thành công

- ***Server:nginx/1.15.8***: thông báo tên và phiên bản server đang sử dụng

- ***Sun, 22 Dec 2024 0:17:26 GMT***: thông báo thời gian và múi giờ.

- ***Content-Type:text/html;charset=utf-8***: thông báo cho client loại dữ liệu nào sẽ được response, giúp browser phía client biết cách xử lý data

- ***Content-Length:252***: lượng data được trả về

- ***Last-Modified:Sun, 22 Dec 2024 0:17:26 GMT***: thông báo thời gian lần cuối cùng dữ liệu được chỉnh sửa

- Còn lại là data được trả về (1 trang html)

3. Make a GET request to /blog and using the gear icon set the id parameter to 1 in the URL field:

Request: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image8.png?raw=true)

Lúc này request trở thành ***GET /blog?id=1 HTTP/1.1***, việc thêm tham số id giúp yêu cầu dữ liệu cụ thể từ server (trong trường hợp này là blog có id=1).

**id** là tham số còn **?id=1** là query string. 

=> Response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image9.png?raw=true)

Server trả về nội dung của blog có id là 1.

4. Make a DELETE request to /user/1: 

Request: 

Sử dụng **DELETE** method để yêu cầu server xóa một tài nguyên cụ thể, trong trường hợp này là **/user**, endpoint để quản lý user, **/1**, ID user cần xóa.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image10.png?raw=true)

=> Response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image11.png?raw=true)

5. Make a PUT request to /user/2 with the username parameter set to admin:

Request: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image12.png?raw=true)


Sử dụng **PUT** method để yêu cầu server chỉnh sửa một tài nguyên, cụ thể tài nguyên ở đây nằm ở endpoint **/user**, **/2**, ID của user là 2 và tài nguyên là **username** với giá trị mới là **admin**.

=> Response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image13.png?raw=true)

6. POST the username of thm and a password of letmein to /login:

Request: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image14.png?raw=true)

Sử dụng **POST** method để gửi dữ liệu đến server yêu cầu tạo mới tài nguyên, cụ thể tài nguyên ở đây là **username** có giá trị là **thm** và **password** có giá trị alf **letmein**.

=> Response: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image15.png?raw=true)




