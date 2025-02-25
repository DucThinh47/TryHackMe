# Nmap Live Host Discovery

**Task 1: Introduction**

=> Những hệ thống nào đang hoạt động? 

=> Những dịch vụ nào đang chạy trên các hệ thống này? 

- ARP scan: sử dụng ARP requests để khám phá live hosts

- ICMP scan: Sử dụng ICMP requests để khám phá live hosts

- TCP/UDP ping scan: Gửi gói tin đến cổng TCP và cổng UDP để xác định live hosts

- arp-scan và masscan

**Task 2: Subnetworks**

- Một phân đoạn mạng là một nhóm máy tính được kết nối bằng phương tiện dùng chung (phương tiện dùng chung có thể là Ethernet switch hoặc điểm truy cập Wifi)

- Mạng con - subnetwork tương đương với một hoặc nhiều phân đoạn mạng được kết nối với nhau và được cấu hình để sử dụng cùng một router. 

- Phân đoạn mạng đề cập đến kết nối vật lý, trong khi mạng con đề cập đến kết nối logic. 

![img](15)

=> Trong sơ đồ mạng trên, có 4 phân đoạn mạng, cũng như mạng con. Nói chung, hệ thống sẽ được kết nối với một trong các phân đoạn mạng / mạng con này.

- Subnetwork, hay đơn giản là subnet, có dải địa chỉ IP riêng và được kết nối với mạng rộng hơn thông qua router. Có thể có firewall thực thi các chính sách bảo mật. 

- Subnets có /16, có nghĩa là subnet mask có thể được viết là 255.255.0.0. Subnet này có thể có khoảng 65 nghìn hosts.

- Subnets có /24, chỉ ra rằng subnet mask có thể được biểu thị là 255.255.255.0. Subnet này có thể có khoảng 250 hosts.

- Nếu đang ở Network A, chỉ có thể sử dụng ARP scan để khám phá các thiết bị trong subnet đó (10.1.100.0/24)

- ARP là giao thức thuộc link-layer và các ARP packets được liên kết với subnet của chúng.

![img](16)

- Computer1 => Computer1 (broadcast). Packet type là ARP request. Data là Computer6 (vì đang yêu cầu địa tìm địa chỉ MAC của Computer6 bằng ARP request).

=> 4 thiết bị có thể nhận ARP request là computer1, computer2, computer3 và router. Computer6 không thể nhận vì không nằm trong cùng subnet với computer1. 

![img](17)

- Từ computer4 => computer4 (broadcast). Packet type là ARP request. Data là computer6 (vì đang yêu cầu tìm địa chỉ MAC của computer6 bằng ARP request).

=> 4 thiết bị có thể nhận ARP request là computer4, computer5, computer6 và router. Lúc này computer6 có thể nhận được ARP request vì nằm trong cùng subnet với computer4. 

**Task 3: Enumerating Targets**

- Với nmap, có thể cung cấp list, range hoặc subnet. Ví dụ về đặc tả mục tiêu là:

    - list: MACHINE_IP scanme.nmap.org example.com sẽ quét 3 địa chỉ IP

    - range: 10.11.12.15-20 sẽ quét 6 địa chỉ IP: 10.11.12.15, 10.11.12.16,… và 10.11.12.20.

    - subnet: MACHINE_IP/30 sẽ quét 4 địa chỉ IP.

- Cũng có thể cung cấp một file làm input cho danh sách mục tiêu của mình, ***nmap -iL list_of_hosts.txt***.

- Nếu muốn kiểm tra danh sách các hosts mà Nmap sẽ quét, sử dụng ***nmap -sL TARGETS***. Tùy chọn này sẽ cung cấp danh sách chi tiết các hosts mà Nmap sẽ quét mà không thực sự quét chúng. Đây còn gọi là list scan

- Nmap sẽ phân giải DNS ngược trên tất cả các mục tiêu để lấy tên. Những cái tên có thể tiết lộ nhiều thông tin khác nhau cho pentester. (Nếu không muốn Nmap vào DNS server, thêm tùy chọn ***-n***.)

![img](18)

**Task 4: Discovering Live Hosts**

- ARP protocol có 1 mục đích: gửi frame đến broadcast - địa  chỉ quảng bá trên phân đoạn mạng và yêu cầu máy tính có địa chỉ IP cụ thể phản hồi bằng cách cung cấp MAC address. 

- ICMP có nhiều loại. Ping ICMP sử dụng Type 8  (Echo) và Type 0 (Echo Reply).

- Nếu muốn ping một hệ thống trên cùng một subnet, truy vấn ARP sẽ được yêu cầu trước ICMP Echo.

**Task 5: Nmap Host Discovery Using ARP**

- Khi người dùng đặc quyền cố gắng quét các mục tiêu trên local network (Ethernet), Nmap sẽ sử dụng các ARP request. Người dùng đặc quyền là root hoặc user thuộc sudoers và có thể chạy sudo.

- Khi người dùng đặc quyền cố gắng quét các mục tiêu bên ngoài mạng cục bộ, Nmap sử dụng ICMP echo requests, TCP ACK (Acknowledge) tới cổng 80, TCP SYN (Synchronize) tới cổng 443 và ICMP timestamp request.

- Khi người dùng bình thường cố gắng quét các mục tiêu bên ngoài mạng cục bộ, Nmap sẽ sử dụng TCP 3-way handshake bằng cách gửi SYN packets tới cổng 80 và 443.

- Theo mặc định, Nmap sử dụng tính năng ping scan để tìm các live hosts, sau đó chỉ tiến hành quét live hosts. 

- Nếu muốn Nmap chỉ thực hiện quét để phát hiện live hosts mà không quét cổng, có thể dùng **nmap -sn TARGETS**. Đây còn gọi là ping scan

- Chỉ có thể quét ARP nếu ở trên cùng subnet với hệ thống đích, tức là trên cùng Ethernet/Wifi. Cần biết MAC address của bất kỳ hệ thống nào trước khi giao tiếp với nó. Để tìm địa chỉ MAC, HĐH sẽ gửi truy vấn ARP.

- Nếu muốn Nmap chỉ thực hiện quét ARP mà không quét cổng trong mạng cục bộ, có thể sử dụng ***nmap -PR -sn TARGETS***, trong đó -PR chỉ ra rằng chỉ muốn quét ARP.

    pentester@TryHackMe$ sudo nmap -PR -sn 10.10.210.6/24

    Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-02 07:12 BST
    Nmap scan report for ip-10-10-210-75.eu-west-1.compute.internal (10.10.210.75)
    Host is up (0.00013s latency).
    MAC Address: 02:83:75:3A:F2:89 (Unknown)
    Nmap scan report for ip-10-10-210-100.eu-west-1.compute.internal (10.10.210.100)
    Host is up (-0.100s latency).
    MAC Address: 02:63:D0:1B:2D:CD (Unknown)
    Nmap scan report for ip-10-10-210-165.eu-west-1.compute.internal (10.10.210.165)
    Host is up (0.00025s latency).
    MAC Address: 02:59:79:4F:17:B7 (Unknown)
    Nmap scan report for ip-10-10-210-6.eu-west-1.compute.internal (10.10.210.6)
    Host is up.
    Nmap done: 256 IP addresses (4 hosts up) scanned in 3.12 seconds

- Dành riêng cho ARP scan, có thể dùng ***arp-scan --localnet*** hoặc đơn giản là ***arp-scan -l***. Lệnh này sẽ gửi truy vấn ARP đến tất cả các địa chỉ IP hợp lệ trên mạng cục bộ.

    pentester@TryHackMe$ sudo arp-scan 10.10.210.6/24
    Interface: eth0, datalink type: EN10MB (Ethernet)
    WARNING: host part of 10.10.210.6/24 is non-zero
    Starting arp-scan 1.9 with 256 hosts (http://www.nta-monitor.com/tools/arp-scan/)
    10.10.210.75	02:83:75:3a:f2:89	(Unknown)
    10.10.210.100	02:63:d0:1b:2d:cd	(Unknown)
    10.10.210.165	02:59:79:4f:17:b7	(Unknown)

    4 packets received by filter, 0 packets dropped by kernel
    Ending arp-scan 1.9: 256 hosts scanned in 2.726 seconds (93.91 hosts/sec). 3 responded

**Task 6: Nmap Host Discovery Using ICMP**

- Có thể ping mọi địa chỉ IP trên mạng mục tiêu và xem ai sẽ phản hồi ping request (ICMP Type 8/Echo) bằng ping response (ICMP Type 0).

- Nhiều firewall chặn ICMP Echo.

- Để sử dụng ICMP echo request để khám phá các live hosts, thêm tùy chọn ***-PE***. (Thêm ***-sn*** nếu không muốn quét cổng).

    pentester@TryHackMe$ sudo nmap -PE -sn 10.10.68.220/24

    Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-02 10:16 BST
    Nmap scan report for ip-10-10-68-50.eu-west-1.compute.internal (10.10.68.50)
    Host is up (0.00017s latency).
    MAC Address: 02:95:36:71:5B:87 (Unknown)
    Nmap scan report for ip-10-10-68-52.eu-west-1.compute.internal (10.10.68.52)
    Host is up (0.00017s latency).
    MAC Address: 02:48:E8:BF:78:E7 (Unknown)
    Nmap scan report for ip-10-10-68-77.eu-west-1.compute.internal (10.10.68.77)
    Host is up (-0.100s latency).
    MAC Address: 02:0F:0A:1D:76:35 (Unknown)
    Nmap scan report for ip-10-10-68-110.eu-west-1.compute.internal (10.10.68.110)
    Host is up (-0.10s latency).
    MAC Address: 02:6B:50:E9:C2:91 (Unknown)
    Nmap scan report for ip-10-10-68-140.eu-west-1.compute.internal (10.10.68.140)
    Host is up (0.00021s latency).
    MAC Address: 02:58:59:63:0B:6B (Unknown)
    Nmap scan report for ip-10-10-68-142.eu-west-1.compute.internal (10.10.68.142)
    Host is up (0.00016s latency).
    MAC Address: 02:C6:41:51:0A:0F (Unknown)
    Nmap scan report for ip-10-10-68-220.eu-west-1.compute.internal (10.10.68.220)
    Host is up (0.00026s latency).
    MAC Address: 02:25:3F:DB:EE:0B (Unknown)
    Nmap scan report for ip-10-10-68-222.eu-west-1.compute.internal (10.10.68.222)
    Host is up (0.00025s latency).
    MAC Address: 02:28:B1:2E:B0:1B (Unknown)
    Nmap done: 256 IP addresses (8 hosts up) scanned in 2.11 seconds

=> Kết quả đầu ra ở trên cho thấy Nmap không cần gửi ICMP packets vì nó xác nhận rằng các host này hoạt động dựa trên ARP response mà nó nhận được.

-  Lặp lại quá trình quét ở trên; tuy nhiên, lần này, quét từ hệ thống thuộc subnet khác. Kết quả tương tự nhưng không có địa chỉ MAC:

![img](19)

- Sử dụng ICMP echo request thường có xu hướng bị chặn bởi firewall => có thể dùng ICMP Timestamp requests hoặc ICMP Address Mask requests để khám phá live hosts. 

- Nmap sử dụng Timestamp request (ICMP Type 13) và kiểm tra xem nó có nhận được Timestamp reply hay không (ICMP Type 14). Sử dụng tùy chọn ***-PP*** sẽ yêu cầu Nmap sử dụng ICMP Timestamp requests.

![img](20)

- Tương tự, Nmap sử dụng truy vấn Address Mask (ICMP Type 17) và kiểm tra xem nó có nhận được Address Mask Reply hay không (ICMP Type 18). Quá trình quét này có thể được kích hoạt bằng tùy chọn ***-PM***.

    pentester@TryHackMe$ sudo nmap -PM -sn 10.10.68.220/24

    Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 12:13 EEST
    Nmap done: 256 IP addresses (0 hosts up) scanned in 52.17 seconds

=> Dựa trên các lần quét trước đó, biết rằng có ít nhất 8 hosts đang hoạt động nhưng lần quét này không trả về kết quả nào. Nguyên nhân là do hệ thống đích hoặc firewall trên route đang chặn loại ICMP packet này.

***-PE -PP -PM***

**Task 7: Nmap Host Discovery Using TCP and UDP**

**TCP SYN Ping**

- Có thể gửi một packet có SYN flag (Synchronize) được đặt thành cổng TCP, 80 theo mặc định và chờ response. Một cổng mở sẽ trả lời bằng SYN/ACK (Acknowledge); một cổng đóng sẽ dẫn đến RST (Reset).

- Nếu muốn dùng Nmap để sử dụng TCP SYN ping, có thể làm như vậy thông qua tùy chọn ***-PS*** theo sau là port number, range, list hoặc kết hợp chúng. Ví dụ: ***-PS21*** sẽ nhắm mục tiêu cổng 21, trong khi ***-PS21-25*** sẽ nhắm mục tiêu các cổng 21, 22, 23, 24 và 25. Cuối cùng ***-PS80,443,8080*** sẽ nhắm mục tiêu ba cổng 80, 443 và 8080.

- Người dùng đặc quyền (root và sudoers) có thể gửi TCP SYN packets và không cần hoàn thành bắt tay 3 bước TCP ngay cả khi cổng mở. 

![img](21)

=> Vì không chỉ định số cổng, theo mặc định, Nmap sử dụng cổng TCP 80, bất kỳ dịch vụ nào đang hoạt động trên cổng 80 đều sẽ reply. 

**TCP ACK Ping**

- Thao tác này sẽ gửi một gói tin có đặt ACK flag. Phải chạy Nmap với tư cách là người dùng có đặc quyền để có thể thực hiện việc này. Nếu thử với tư cách là người dùng không có đặc quyền, Nmap sẽ cố gắng thực hiện bắt tay 3 bước.

- Sử dụng tùy chọn ***-PA***. Cũng có thể chỉ định số cổng theo sau, ví dụ: -PA21, -PA21-25 và -PA80,443,8080. Nếu không có cổng nào được chỉ định, cổng 80 sẽ được sử dụng.

- Bất kỳ TCP packet nào có ACK flag sẽ nhận lại TCP packet có RST flag được đặt.

![img](22)

**UDP Ping**

- Có thể sử dụng UDP để khám phá live hosts. Ngược lại với TCP SYN ping, việc gửi UDP packet đến một cổng mở sẽ không dẫn đến bất kỳ reply nào. Tuy nhiên, nếu gửi UDP packet đến một cổng UDP đã đóng, sẽ nhận được thông báo ***ICMP port unreachable packet***. 

- Cú pháp, thêm tùy chọn ***-PU***

![img](23)

**Masscan**

- Để hoàn thành quá trình quét mạng của mình một cách nhanh chóng, Masscan khá tích cực với tốc độ packets mà nó tạo ra.

- ***masscan MACHINE_IP/24 -p443***

- ***masscan MACHINE_IP/24 -p80,443***

- ***masscan MACHINE_IP/24 -p22-25***

- ***masscan MACHINE_IP/24 --top-ports 100***

**Task 8: Using Reverse-DNS Lookup**

- Theo mặc định, Nmap sẽ khám phá online hosts; tuy nhiên, có thể sử dụng tùy chọn ***-R*** để truy vấn DNS server ngay cả đối với offline hosts.

![img](24)

**Task 9: Summary**

- ARP Scan: ***nmap -PR -sn ...***

- ICMP Echo Scan: ***nmap -PE -sn ...***

- ICMP Timestamp Scan: ***nmap -PP -sn ...***

- ICMP Address Mask Scan: ***nmap -PM -sn ... ***

- TCP SYN Ping Scan: ***nmap -PS -sn ...***

- TCP ACK Ping Scan: ***nmap -PA -sn ...***

- UDP Ping Scan: ***nmap -PU -sn ...***





















