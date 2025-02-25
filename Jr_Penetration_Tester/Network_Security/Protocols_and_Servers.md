# Protocols and Servers

**Task 1: Introduction**

- HTTP

- FTP

- POP3

- SMTP

- IMAP

**Task 2: Telnet**

- Telnet protocol là giao thức thuộc Application layer được sử dụng để kết nối với thiết bị đầu cuối ảo của máy tính khác. Sử dụng Telnet, người dùng có thể đăng nhập vào một máy tính khác và truy cập thiết bị đầu cuối (bảng điều khiển) của nó để chạy chương trình, bắt đầu các quy trình hàng loạt và thực hiện các tác vụ quản trị hệ thống từ xa.

- Khi người dùng kết nối, họ sẽ được yêu cầu nhập username và password.

- tất cả giao tiếp giữa Telnet client và Telnet server không được mã hóa

- Telnet server sử dụng Telnet protocol để lắng nghe các kết nối đến trên port 23.

    pentester@TryHackMe$ telnet 10.10.204.19
    Trying 10.10.204.19...
    Connected to 10.10.204.19.
    Escape character is '^]'.
    Ubuntu 20.04.3 LTS
    bento login: frank
    Password: D2xc9CgD
    Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-84-generic x86_64)

    * Documentation:  https://help.ubuntu.com
    * Management:     https://landscape.canonical.com
    * Support:        https://ubuntu.com/advantage

    System information as of Fri 01 Oct 2021 12:24:56 PM UTC

    System load:  0.05              Processes:              243
    Usage of /:   45.7% of 6.53GB   Users logged in:        1
    Memory usage: 15%               IPv4 address for ens33: 10.10.204.19
    Swap usage:   0%

    * Super-optimized for small spaces - read how we shrank the memory
    footprint of MicroK8s to make it the smallest full K8s around.

    https://ubuntu.com/blog/microk8s-memory-optimisation

    0 updates can be applied immediately.


    *** System restart required ***
    Last login: Fri Oct  1 12:17:25 UTC 2021 from meiyo on pts/3
    You have mail.
    frank@bento:~$

- Trong hình bên dưới, nắm bắt được lưu lượng truy cập do Telnet tạo ra và việc tìm ra password rất dễ dàng.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image62.png)

- Telnet không còn được coi là một lựa chọn an toàn nữa, đặc biệt là bất kỳ ai nắm bắt được lưu lượng truy cập mạng sẽ có thể khám phá username và password, điều này sẽ cấp cho họ quyền truy cập vào hệ thống từ xa. Giải pháp thay thế an toàn là SSH.

**Task 3: Hypertext Transfer Protocol (HTTP)**

- Hypertext Transfer Protocol - Giao thức truyền siêu văn bản (HTTP) là giao thức được sử dụng để truyền các trang web.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image63.png)

- HTTP gửi và nhận dữ liệu dưới dạng văn bản rõ ràng (không được mã hóa)

- xem cách có thể request một trang từ webserver:

1. Đầu tiên, kết nối với port 80 bằng ***telnet 10.10.204.19 80***.

2. Tiếp theo, cần gõ ***GET /index.html HTTP/1.1*** để lấy trang index.html hoặc GET /HTTP/1.1 để lấy default page.

3. Cuối cùng, cần cung cấp một số giá trị cho host như host: telnet và nhấn phím Enter/Return hai lần.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image64.png)

=> người dùng chỉ cần nhập một vài lệnh để có được trang họ cần

**Task 4: File Transfer Protocol (FTP)**

- File Transfer Protocol (FTP) được phát triển để giúp việc truyền file giữa các máy tính khác nhau với các hệ thống khác nhau trở nên hiệu quả.

- FTP cũng gửi và nhận dữ liệu dưới dạng văn bản rõ ràng

1. Kết nối với FTP server bằng ứng dụng Telnet client. Vì FTP server lắng nghe trên port 21 theo mặc định nên phải chỉ định cho Telnet client để thử kết nối với port 21 thay vì default Telnet port

2. cần cung cấp username bằng lệnh USER Frank.

3. Sau đó, cung cấp password bằng lệnh PASS D2xc9CgD.

4. Vì đã cung cấp đúng username và password nên đã đăng nhập được.

- Một lệnh như STAT có thể cung cấp một số thông tin bổ sung. Lệnh SYST hiển thị System Type của mục tiêu (UNIX trong trường hợp này). PASV chuyển chế độ sang passive. Điều đáng chú ý là có hai chế độ cho FTP:

    - Active: Ở active mode, dữ liệu được gửi qua một kênh riêng có nguồn gốc từ port 20 của FTP server.

    - Passive: Ở passive mode, dữ liệu được gửi qua một kênh riêng biệt bắt nguồn từ port của FTP client port 1023.

- Lệnh TYPE A chuyển chế độ file transfer sang ASCII, trong khi TYPE I chuyển chế độ file transfer sang binary. Tuy nhiên, không thể transfer file bằng client đơn giản như Telnet vì FTP tạo một kết nối riêng để transfer file.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image65.png)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image66.png)

=>  sử dụng FTP client thực tế để tải xuống tệp văn bản: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image67.png)

**Task 5: Simple Mail Transfer Protocol (SMTP)**

- Gửi email qua Internet yêu cầu các thành phần sau:

    - Mail Submission Agent - Đại lý gửi thư (MSA)

    - Mail Transfer Agent - Đại lý chuyển thư (MTA)

    - Mail Delivery Agent - Đại lý chuyển phát thư (MDA)

    - Mail User Agent - Tác nhân người dùng thư (MUA)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image68.png)

- Theo cách tương tự, cần tuân theo một protocol để liên lạc với HTTP server và cần dựa vào các email protocol để giao tiếp với MTA và MDA. Các giao thức là:

1. Simple Mail Transfer Protocol (SMTP)

2. Post Office Protocol version 3 (POP3) hoặc Internet Message Access Protocol (IMAP)

- Simple Mail Transfer Protocol (SMTP) được sử dụng để liên lạc với MTA server. Vì SMTP sử dụng văn bản rõ ràng, trong đó tất cả các lệnh được gửi mà không cần mã hóa,

- SMTP server lắng nghe trên port 25 theo mặc định.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image69.png)

**Task 6: Post Office Protocol 3 (POP3)**

- Post Office Protocol version 3 (POP3) là protocol được sử dụng để tải xuống các email từ Mail Delivery Agent (MDA) server

- Email client kết nối với POP3 server, xác thực, tải xuống các thư email mới trước khi (tùy chọn) xóa chúng.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image70.png)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image71.png)

- Mặc định chạy trên cổng 110 

- Dựa trên cài đặt mặc định, Mail Client sẽ xóa email sau khi tải xuống.

=>  Việc truy cập vào cùng một tài khoản email qua nhiều client bằng POP3 thường không thuận tiện lắm vì sẽ mất dấu các tin nhắn đã đọc và chưa đọc. 

=> Để giữ cho tất cả các hộp thư được đồng bộ, cần xem xét các giao thức khác, chẳng hạn như IMAP.

**Task 7: Internet Message Access Protocol (IMAP)**

- Internet Message Access Protocol (IMAP) phức tạp hơn POP3. IMAP có thể đồng bộ hóa email trên nhiều thiết bị (và Mail Clients). Nói cách khác, nếu đã đánh dấu email là đã đọc khi kiểm tra email trên điện thoại thông minh, thay đổi sẽ được lưu trên IMAP server (MDA) và được sao chép trên máy tính xách tay khi đồng bộ hóa hộp thư đến.

- Mặc định chạy trên cổng 143. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image72.png)

=> Rõ ràng là IMAP gửi thông tin đăng nhập ở dạng văn bản rõ ràng, có thể thấy trong lệnh LOGIN frank D2xc9CgD. Bất cứ ai theo dõi lưu lượng mạng đều có thể biết username và password của Frank.

**Task 8: Summary**

- FTP: port - 21; application - file transfer; data security - cleartext

- HTTP: port - 80; application - WorldWide Web; data security - cleartext

- IMAP: port - 143; application - Email (MDA); data security - cleartext

- POP3: port - 110; application - Email (MDA); data security - cleartext

- SMTP: port  - 25; application - Email (MTA); data security - cleartext

- Telnet: port - 23; application - Remote Access; data security - cleartext



