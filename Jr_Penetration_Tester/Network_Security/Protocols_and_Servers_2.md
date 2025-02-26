# Protocols and Servers 2

**Task 1: Introduction**

- Sniffing Attack (Network Packet Capture) - Tấn công đánh hơi (Chụp gói mạng)

- Man-in-the-Middle (MITM) Attack

- Password Attack (Authentication Attack)

- Vulnerabilities

- The Security Triad (Confidentiality, Integrity và Availability)

- Hydra tool

**Task 2: Sniffing Attack**

- Tấn công sniffing đề cập đến việc sử dụng công cụ bắt gói mạng để thu thập thông tin về mục tiêu. 

- Khi một giao thức giao tiếp ở dạng văn bản rõ ràng, không được mã hóa trong quá trình truyền, dữ liệu được trao đổi có thể được bên thứ ba thu thập để phân tích.

- ***tcpdump*** là một giao diện dòng lệnh, mã nguồn mở miễn phí

- ***Wireshark*** là chương trình giao diện đồ họa người dùng, mã nguồn mở miễn phí có sẵn

- ***Tshark*** là một CLI thay thế cho Wireshark

=> Ví dụ, sử dụng tcpdump để cố lấy username và password:

***sudo tcpdump port 110 -A***

Trong đó: 

    - port 110 để chỉ định sẽ lọc các gói tin POP3.

    - tùy chọn -A hiển thị nội dung gói tin bắt được ở định dạng ASCII

    pentester@TryHackMe$ sudo tcpdump port 110 -A
    [...]
    09:05:15.132861 IP 10.20.30.1.58386 > 10.20.30.148.pop3: Flags [P.], seq 1:13, ack 19, win 502, options [nop,nop,TS val 423360697 ecr 3958275530], length 12
    E..@.V@.@.g.
    ...
    ......n......"............
    .;....}.USER frank

    09:05:15.133465 IP 10.20.30.148.pop3 > 10.20.30.1.58386: Flags [.], ack 13, win 510, options [nop,nop,TS val 3958280553 ecr 423360697], length 0
    E..4..@.@.O~
    ...
    ....n....".........?P.....
    ...i.;..
    09:05:15.133610 IP 10.20.30.148.pop3 > 10.20.30.1.58386: Flags [P.], seq 19:43, ack 13, win 510, options [nop,nop,TS val 3958280553 ecr 423360697], length 24
    E..L..@.@.Oe
    ...
    ....n....".........<-.....
    ...i.;..+OK Password required.

    09:05:15.133660 IP 10.20.30.1.58386 > 10.20.30.148.pop3: Flags [.], ack 43, win 502, options [nop,nop,TS val 423360698 ecr 3958280553], length 0
    E..4.W@.@.g.
    ...
    ......n......".....??.....
    .;.....i
    09:05:22.852695 IP 10.20.30.1.58386 > 10.20.30.148.pop3: Flags [P.], seq 13:28, ack 43, win 502, options [nop,nop,TS val 423368417 ecr 3958280553], length 15
    E..C.X@.@.g.
    ...
    ......n......".....6......
    .<.....iPASS D2xc9CgD
    [...]

=> Cũng có thể sử dụng Wireshark để đạt kết quả tương tự.  

![img](73)

=> Yêu cầu duy nhất để cuộc tấn công bắt gói mạng thành công này là **có quyền truy cập vào giữa hai hệ thống đang liên lạc**.

**Task 2: Man-in-the-Middle (MITM) Attack**

![img](74)

- Cuộc tấn công xảy ra khi hai bên không xác nhận tính xác thực và tính toàn vẹn của từng tin nhắn.

- Nhiều công cụ sẽ hỗ trợ thực hiện một cuộc tấn công MITM, chẳng hạn như Ettercap và Bettercap.

- Giải pháp nằm ở việc xác thực phù hợp cùng với mã hóa hoặc ký các tin nhắn được trao đổi, với sự trợ giúp của PKI - Public Key Infrastructure và chứng chỉ gốc đáng tin cậy. 

- Transport Layer Security - TLS giúp bảo vệ khỏi các cuộc tấn công MITM.

**Task 4: Transport Layer Security (TLS)**

- SSL (Secure Sockets Layer) bắt đầu khi world wide web bắt đầu thấy các ứng dụng mới, chẳng hạn như mua sắm trực tuyến và gửi thông tin thanh toán.

- Có thể thêm mã hóa vào các giao thức thông qua Presentation layer, dữ liệu sẽ được trình bày ở định dạng được mã hóa thay vì dạng ban đầu. 

![img](75)

- Do mối quan hệ chặt chẽ giữa SSL và TLS, nên cái này có thể được sử dụng thay cho cái kia. Tuy nhiên, TLS an toàn hơn SSL và trên thực tế nó đã thay thế SSL. 

- Có thể nói rằng tất cả các server hiện tại đều sử dụng TLS

- Các giao thức truyền văn bản dưới dạng rõ có thể được nâng cấp sử dụng mã hóa thông qua SSL/TLS. 

- Có thể sử dụng TLS để nâng cấp HTTP, FTP, SMTP, POP3 và IMAP

- HTTP - port 80 => HTTPs - port 443

- FTP - port 21 => FTPs - port 990

- SMTP - port 25 => SMTPs - port 465

- POP3 - port 110 => POP3s - port 995

- IMAP - port 143 => IMAPs - port 993

- Bình thường, để truy xuất nội dung trag web qua HTTP, trình duyệt cần thực hiện ít nhất 2 bước: 

    - Thiết lập kết nối TCP với web server

    - Gửi HTTP request

=> HTTPS yêu cầu một bước bổ sung để mã hóa lưu lượng. Bước mới diễn ra sau khi thiết lập kết nối TCP và trước khi gửi yêu cầu HTTP

![img](776)

- Sau khi bắt tay SSL/TLS đã được thiết lập, bất kỳ ai đang xem kênh liên lạc sẽ không thể truy cập được các HTTP requests và dữ liệu trao đổi.

- Ví dụ, khi duyệt trang web TryHackMe qua HTTPs, trình duyệt sẽ mong đợi webserver của TryHackMe cung cấp ***chứng chỉ đã ký từ cơ quan cấp chứng chỉ đáng tin cậy***.

=> trình duyệt đảm bảo rằng nó đang liên lạc với đúng servervà cuộc tấn công MITM không thể xảy ra.

**Task 5: Secure Shell (SSH)**

- Secure Shell (SSH) được tạo ra để cung cấp một cách an toàn cho việc quản trị hệ thống từ xa.

- Có thể xác nhận danh tính hệ thống từ xa

- Tin nhắn trao đổi được mã hóa và chỉ người nhận dự kiến mới có thể giải mã

- Cả 2 bên có thể phát hiện bất kỳ sửa đổi nào trong tin nhắn

- SSH server lắng nghe trên port 22, theo mặc định.

- Cú pháp ***ssh username@10.10.125.30***:

![img](77)

- username và password được mã hóa khi gửi, tất cả các lệnh thực thi trên hệ thống từ xa sẽ được gửi qua kênh được mã hóa.

![img](78)

- có thể sử dụng SSH để truyền file bằng SCP (Secure Copy Protocol) dựa trên giao thức SSH. Cú pháp ***scp mark@10.10.125.30:/home/mark/archive.tar.gz ~***, lệnh này sẽ sao chép file archive.tar.gz từ hệ thống từ xa nằm trong thư mục /home/mark sang thư mục ~, thư mục gốc của user hiện tại đang truy nhập hệ thống từ xa

- Cú pháp sao chép file từ hệ thống cục bộ sang hệ thống từ xa: ***scp backup.tar.bz2 mark@10.10.125.30:/home/mark/*** 

    user@TryHackMe$ scp document.txt mark@10.10.125.30:/home/mark
    mark@10.10.125.30's password: 
    document.txt                                        100% 1997KB  70.4MB/s   00:00

- FTP có thể được bảo mật bằng SSL/TLS bằng giao thức FTPS sử dụng port 990. Đồng thời, FTP cũng có thể được bảo mật bằng giao thức SSH là giao thức SFTP.

![img](79)

**Task 6: Password Attack**

![img](80)

- Việc xác minh danh tính có thể sử dụng một trong những cách sau, hoặc kết hợp các cách: 

    - Biết gì? (password, PIN,...)

    - Có gì? (SIM card, RFID card,...)

    - Là gì? (vân tay,...)

- Các cuộc tấn công chống lại mật khẩu thường được thực hiện bởi:

    - Password Guessing

    - Dictionary Attack: mở rộng việc đoán mật khẩu và cố gắng đưa tất cả các từ hợp lệ vào từ điển hoặc danh sách từ.

    - Brute Force Attack: Cuộc tấn công này là cuộc tấn công toàn diện và tốn thời gian nhất, trong đó kẻ tấn công có thể thử tất cả các kết hợp ký tự có thể

- Thử mật khẩu phổ biến một cách tự động => sử dụng ***Hydra***

- Cú pháp ***hydra -l username -P wordlist.txt server service***, trong đó: 

    - -l username: -l phải đặt trước username, tức là tên đăng nhập của mục tiêu.

    - -P wordlist.txt: -P đứng trước tệp wordlist.txt, đây là tệp văn bản chứa danh sách mật khẩu muốn thử với tên người dùng được cung cấp.

    - server là tên máy chủ hoặc địa chỉ IP của máy chủ mục tiêu.

    - service cho biết dịch vụ mà đang cố gắng khởi động tấn công Dictionary.

Ví dụ: 

- ***hydra -l mark -P /usr/share/wordlists/rockyou.txt 10.10.125.30 ftp*** sẽ sử dụng mark làm username khi nó lặp lại các mật khẩu được cung cấp đối với FTP server.

- ***hydra -l mark -P /usr/share/wordlists/rockyou.txt ftp://10.10.125.30*** giống ví dụ trên

- ***hydra -l frank -P /usr/share/wordlists/rockyou.txt 10.10.125.30 ssh*** sẽ sử dụng frank làm tên người dùng khi nó cố gắng đăng nhập qua SSH bằng các mật khẩu khác nhau.

- Tùy chọn mở rộng: 

    - ***-s PORT*** để chỉ định non-default port cho dịch vụ được đề cập.

    - ***-V hoặc -vV***, verbose, làm cho Hydra hiển thị các tổ hợp tên người dùng và mật khẩu đang được thử.

    - ***-t n*** trong đó n là số lượng kết nối song song tới đích. -t 16 sẽ tạo 16 luồng dùng để kết nối với mục tiêu.

    - ***-d***, để debugging, để có được thông tin chi tiết hơn về những gì đang diễn ra.

- Một số biện pháp giảm thiểu: 

    - Password Policy:  Thực thi các ràng buộc về độ phức tạp tối thiểu đối với mật khẩu do người dùng đặt.

    - Account Lockout: Khóa tài khoản sau một số lần thử thất bại nhất định.

    - Throttling Authentication Attempts - Kiểm soát các nỗ lực xác thực: Trì hoãn phản hồi cho nỗ lực đăng nhập.

    - Using CAPTCHA - Sử dụng CAPTCHA: Yêu cầu giải một câu hỏi khó đối với máy.

    - Yêu cầu sử dụng chứng chỉ công cộng để xác thực.

    - Two-Factor Authentication - Xác thực hai yếu tố: Yêu cầu người dùng cung cấp mã có sẵn thông qua các phương tiện khác, chẳng hạn như email, ứng dụng điện thoại thông minh hoặc SMS.

    - Có nhiều cách tiếp cận khác phức tạp hơn hoặc có thể yêu cầu một số kiến ​​thức đã được thiết lập về người dùng, chẳng hạn như định vị địa lý dựa trên IP

![img](81)

**Task 7: Summary**

- 3 cuộc tấn công phổ biến: 

    1. Sniffing Attack

    2. MITM Attack

    3. Password Attack

- FTP - port 21: File transfer, Cleartext

- FTPs - port 880: File transfer, Encrypted

- HTTP - port 80: Worldwide Web, Cleartext

- HTTPs - port 443: Worldwide Web, Encrypted

- IMAP - port 143: Email (MDA), Cleartext

- IMAPs - port 993: Email (MDA), Encrypted

- POP3 - port 110: Email (MDA), Cleartext

- POP3s - port 995: Email (MDA), Encrypted

- SFTP - port 22: File transfer, Encrypted

- SSH - port 22: Remote Access and File transfer, Encrypted

- SMTP - port 25: Email (MTA), Cleartext

- SMTPs - port 465: Email (MTA), Encrypted

- Telent - port 23: Remote access, Cleartext

- Hydra vẫn là một công cụ rất hiệu quả có thể khởi chạy từ thiết bị đầu cuối để thử các mật khẩu khác nhau:

    - -l username: Provide the login name

    - -P WordList.txt: Specify the password list to use

    - server service: Set the server address and service to attack

    - -s PORT: Use in case of non-default service port number

    - -V or -vV: Show the username and password combinations being tried

    - -d: Display debugging output if the verbose output is not helping









