# Nmap Post Port Scans

**Task 1: Introduction**

- Phát hiện versions của dịch vụ đang chạy (trên tất cả cổng đang mở)

- Phát hiện hệ điều hành dựa trên bất kỳ dấu hiệu nào được mục tiêu tiết lộ

- Chạy traceroute của Nmap

- Chạy select Nmap scripts

- Lưu kết quả quét ở nhiều định dạng khác nhau

**Task 2: Service Detection**

- Thêm tùy chọn ***-sV*** sẽ thu thập và xác định thông tin dịch vụ và version. 

- Có thể kiểm soát cường độ bằng ***--version-intensity LEVEL*** trong đó Level nằm trong khoảng từ 0, nhẹ nhất và 9, hoàn thiện nhất là ***-sV --version-light*** có cường độ là 2, trong khi ***-sV --version-all*** có cường độ là 9.

- Việc sử dụng -sV sẽ buộc Nmap tiến hành TCP 3-way handshake và thiết lập kết nối => Không thể sử dụng cùng với -sS. 

![img](58)

**Task 3: OS Detection and Traceroute**

**OS Detection**

- Sử dụng tùy chọn **-O**. 

    pentester@TryHackMe$ sudo nmap -sS -O 10.10.126.141

    Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-10 05:04 BST
    Nmap scan report for 10.10.126.141
    Host is up (0.00099s latency).
    Not shown: 994 closed ports
    PORT    STATE SERVICE
    22/tcp  open  ssh
    25/tcp  open  smtp
    80/tcp  open  http
    110/tcp open  pop3
    111/tcp open  rpcbind
    143/tcp open  imap
    MAC Address: 02:A0:E7:B5:B6:C5 (Unknown)
    Device type: general purpose
    Running: Linux 3.X
    OS CPE: cpe:/o:linux:linux_kernel:3.13
    OS details: Linux 3.13
    Network Distance: 1 hop

    OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 3.91 seconds

- Nmap cần tìm ít nhất một cổng mở và một cổng đóng trên mục tiêu để đưa ra dự đoán đáng tin cậy. 

**Traceroute**

- Nếu muốn Nmap tìm các routers giữa hệ thống nguồn và hệ thống đích, thêm tùy chọn ***--traceroute***. 

![img](59)

=> Không có router/hops nào giữa hai thiết bị vì chúng được kết nối trực tiếp. 

**Task 4: Nmap Scripting Engine (NSE)**

- Script là một đoạn mã không cần phải biên dịch. Nói cách khác, nó vẫn ở dạng ban đầu mà con người có thể đọc được và không cần phải chuyển đổi sang ngôn ngữ máy. 

- Nmap Scripting Engine (NSE) là trình thông dịch Lua cho phép Nmap thực thi các scripts Nmap được viết bằng ngôn ngữ Lua.

- Cài đặt mặc định Nmap có thể dễ dàng chứa gần 600 scripts.

- Có thể cài đặt các scripts của người dùng khác và sử dụng chúng để quét. 

- Có thể chọn chạy các tập lệnh trong danh mục mặc định bằng cách sử dụng ***--script=default*** hoặc chỉ cần thêm ***-sC***.

    pentester@TryHackMe$ sudo nmap -sS -sC 10.10.126.141

    Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-10 05:08 BST
    Nmap scan report for ip-10-10-161-170.eu-west-1.compute.internal (10.10.161.170)
    Host is up (0.0011s latency).
    Not shown: 994 closed ports
    PORT    STATE SERVICE
    22/tcp  open  ssh
    | ssh-hostkey: 
    |   1024 d5:80:97:a3:a8:3b:57:78:2f:0a:78:ae:ad:34:24:f4 (DSA)
    |   2048 aa:66:7a:45:eb:d1:8c:00:e3:12:31:d8:76:8e:ed:3a (RSA)
    |   256 3d:82:72:a3:07:49:2e:cb:d9:87:db:08:c6:90:56:65 (ECDSA)
    |_  256 dc:f0:0c:89:70:87:65:ba:52:b1:e9:59:f7:5d:d2:6a (EdDSA)
    25/tcp  open  smtp
    |_smtp-commands: debra2.thm.local, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, 
    | ssl-cert: Subject: commonName=debra2.thm.local
    | Not valid before: 2021-08-10T12:10:58
    |_Not valid after:  2031-08-08T12:10:58
    |_ssl-date: TLS randomness does not represent time
    80/tcp  open  http
    |_http-title: Welcome to nginx on Debian!
    110/tcp open  pop3
    |_pop3-capabilities: RESP-CODES CAPA TOP SASL UIDL PIPELINING AUTH-RESP-CODE
    111/tcp open  rpcbind
    | rpcinfo: 
    |   program version   port/proto  service
    |   100000  2,3,4        111/tcp  rpcbind
    |   100000  2,3,4        111/udp  rpcbind
    |   100024  1          38099/tcp  status
    |_  100024  1          54067/udp  status
    143/tcp open  imap
    |_imap-capabilities: LITERAL+ capabilities IMAP4rev1 OK Pre-login ENABLE have LOGINDISABLEDA0001 listed SASL-IR ID more post-login LOGIN-REFERRALS IDLE
    MAC Address: 02:A0:E7:B5:B6:C5 (Unknown)

    Nmap done: 1 IP address (1 host up) scanned in 2.21 seconds

=> Nmap đã khôi phục tất cả 4 public keys liên quan đến máy chủ ssh đang chạy.

- Ví dụ ***--script "http-date"*** sẽ truy xuất ngày và giờ của máy chủ http : 

![img](60)

**Task 5: Saving the Output**

**NORMAL**

- Lưu output ở normal format bằng tùy chọn ***-oN FILENAME***

=> normal format nó dạng như sau:

![img](61)

**Grepable**

- Grepable format có tên từ lệnh grep

- giúp việc lọc đầu ra quét cho các từ khóa hoặc thuật ngữ cụ thể trở nên hiệu quả.

- Sử dụng tùy chọn ***-oG FILENAME***.

=> Ví dụ output file: 

    pentester@TryHackMe$ cat MACHINE_IP_scan.gnmap 
    # Nmap 7.60 scan initiated Fri Sep 10 05:14:19 2021 as: nmap -sS -sV -O -oG MACHINE_IP_scan 10.10.184.112
    Host: 10.10.184.112	Status: Up
    Host: 10.10.184.112	Ports: 22/open/tcp//ssh//OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)/, 25/open/tcp//smtp//Postfix smtpd/, 80/open/tcp//http//nginx 1.6.2/, 110/open/tcp//pop3//Dovecot pop3d/, 111/open/tcp//rpcbind//2-4 (RPC #100000)/, 143/open/tcp//imap//Dovecot imapd/	Ignored State: closed (994)	OS: Linux 3.13	Seq Index: 257	IP ID Seq: All zeros
    # Nmap done at Fri Sep 10 05:14:28 2021 -- 1 IP address (1 host up) scanned in 9.99 seconds

**XML**

- có thể lưu kết quả quét ở XML format bằng cách sử dụng ***-oX FILENAME***

- XML format sẽ thuận tiện nhất để xử lý đầu ra trong các chương trình khác.

- có thể lưu kết quả quét ở cả ba định dạng bằng cách sử dụng ***-oA FILENAME*** để kết hợp -oN, -oG và -oX cho normal, grepable và XML.

**Task 6: Summary**

- -sV: determine service/version info on open ports

- -sV --version-light: try the most likely probes (2)

- -sV --version-all: try all available probes (9)

- -O: detect OS

- --traceroute: run traceroute to target

- --script=SCRIPTS: Nmap scripts to run

- -sC or --script=default: run default scripts

- -A: equivalent to -sV -O -sC --traceroute

- -oN: save output in normal format

- -oG: save output in grepable format

- -oX: save output in XML format

- -oA: save output in normal, XML and Grepable formats