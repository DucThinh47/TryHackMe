# Nmap Advanced Port Scans

**Task 1: Introduction**

- Null Scan

- FIN Scan

- Xmas Scan

- Maimon Scan

- ACK Scan

- Window Scan

- Custom Scan

- Spoofing IP 

- Spoofing MAC

- Decoy Scan - Quét mồi nhử

- Fragmented Packets

- Idle / Zombie Scan

**Task 2: TCP Null Scan, FIN Scan, and Xmas Scan**

**Null Scan**

- Quá trình Null scan không set bất kỳ flags nào; tất cả 6 bit flag được set thành 0. 

- Sử dụng tùy chọn **-sN**, phải chạy với quyền root.

- Theo quan điểm của Nmap, việc thiếu response trong quá trình Null scan cho thấy rằng cổng đang mở hoặc firewall đang chặn gói tin.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image36.png)

- Lí tưởng nhất là mục tiêu phản hồi bằng gói tin với RST flag nếu cổng đóng. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image37.png)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image38.png)

**FIN Scan**

- Quá trình FIN scan gửi gói tin TCP có set FIN flag.

- Sử dụng tùy chọn **-sF**.

- Giống như Null scan, không có phản hồi nào nếu cổng TCP mở. Nmap không thể chắc chắn cổng mở hay bị firewall chặn. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image39.png)

- Hệ thống mục tiêu sẽ phản hồi bằng gói tin với RST flag nếu cổng đóng.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image40.png)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image41.png)

**Xmas Scan**

- Quá trình Xmas scan sẽ set đồng thời FIN flag, PSH flag và URG flag.

- Sử dụng tùy chọn **-sX**. 

- Tương tự, cổng đóng sẽ phản hồi gói tin với RST flag và không có phản hồi nếu cổng mở. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image42.png)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image43.png)

- Sử dụng Xmas scan có thể hiệu quả khi quét mục tiêu phía sau ***firewall stateless***. Firewall stateless sẽ kiểm tra xem gói tin đến có set SYN flag hay không, để phát hiện nỗ lực kết nối => việc sử dụng tổ hợp flag không khớp với gói tin SYN có thể đánh lừa loại firewall này. 

- Tuy nhiên, stateful firewall trên thực tế sẽ chặn tất cả gói tin được tạo ra như vậy và khiến kiểu quét này trở nên vô dụng.

**Task 3: TCP Maimon Scan**

- Maimon scan sẽ gửi gói tin có FIN và ACK flag được set. 

- Quá trình quét này sẽ không hoạt động trên hầu hết các mục tiêu gặp phải trong các mạng hiện đại

- Sử dụng tùy chọn ***-sM***. Hầu hết các hệ thống mục tiêu đều phản hồi bằng RST packet bất kể cổng TCP có mở hay không.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image45.png)

=> Không thể phát hiện vì cổng mở và cổng đóng đều được phản hồi giống nhau. 

**Task 4: TCP ACK, Window, and Custom Scan**

**TCP ACK Scan**

- ACK scan sẽ gửi gói tin TCP có set ACK flag.

- Sử dụng tùy chọn ***-sA***. 

- Mục tiêu phản hồi bằng gói tin có set RST flag bất kể cổng mở hay đóng. 

=> quá trình quét này sẽ không cho biết liệu cổng mục tiêu có mở trong một thiết lập đơn giản hay không.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image46.png)

=> Scan khi chưa thiết lập firewall: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image47.png)

- Kiểu quét này sẽ hữu ích nếu có firewall phía trước mục tiêu. Do đó, dựa trên việc gói tin ACK  nào có phản hồi, có thể biết được cổng nào không bị firewall chặn. Nói cách khác, kiểu quét này phù hợp hơn để khám phá các bộ quy tắc và cấu hình firewall.

=> Scan sau khi thiết lập firewall: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image48.png)

=> 3 cổng không bị firewall chặn, còn lại bị chặn. 

**Window Scan**

- TCP window scan gần giống như ACK scan; tuy nhiên, nó kiểm tra TCP Window field của gói tin TCP có RST flag được trả về.

- Sử dụng tùy chọn ***-sW***. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image49.png)

=> Scan khi chưa thiết lập firewall: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image50.png)

=> Scan khi thiết lập firewall phía trước mục tiêu: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image51.png)

**Custom Scan**

- Nếu muốn thử nghiệm một tổ hợp TCP flag mới ngoài các kiểu TCP scan tích hợp sẵn, có thể thực hiện bằng cách sử dụng ***--scanflags***.

- Ví dụ, nếu muốn set đồng thời SYN, RST và FIN flag, cú pháp sẽ là ***--scanflags RSTSYNFIN***. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image52.png)

**Task 5: Spoofing and Decoys**

- Có thể quét hệ thống mục tiêu bằng địa chỉ IP giả mạo và thậm chí cả địa chỉ MAC giả mạo. Việc quét như vậy chỉ có lợi trong trường hợp có thể đảm bảo nắm bắt được phản hồi. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image53.png)

=> kẻ tấn công khởi chạy lệnh ***nmap -S SPOOFED_IP 10.10.173.49***. Do đó, Nmap sẽ tạo tất cả gói tin bằng địa chỉ IP nguồn được cung cấp SPOOFED_IP. Máy mục tiêu sẽ phản hồi các gói tin đến và gửi phản hồi đến địa chỉ IP đích SPOOFED_IP. Để quá trình quét này hoạt động và cho kết quả chính xác, kẻ tấn công cần theo dõi lưu lượng mạng để phân tích phân tích.

=> 3 bước để quét bằng địa chỉ IP giả mạo: 

1. Kẻ tấn công gửi một gói tin có nguồn là địa chỉ chỉ IP giả mạo đến máy mục tiêu.

2. Máy mục tiêu phản hồi địa chỉ IP giả mạo.

3. Kẻ tấn công nắm bắt phản hồi để tìm ra cổng mở. 

- Chỉ định giao diện mạng, sử dụng tùy chọn ***-e*** và tắt ping scan bằng tùy chọn ***-Pn***.

=> Thay vì ***nmap -S SPOOFED_IP 10.10.173.49***, sử dụng ***nmap -e NET_INTERFACE -Pn -S SPOOFED_IP 10.10.173.49*** để cho Nmap biết rõ ràng nên sử dụng network interface nào và không mong đợi nhận được phản hồi lệnh ping. Quá trình quét này sẽ vô ích nếu hệ thống kẻ tấn công không thể giám sát mạng để tìm phản hồi. 

- Khi ở trên cùng subnet với máy mục tiêu, có thể giả mạo địa chỉ MAC. Chỉ định địa chỉ MAC nguồn bằng ***--spoof-mac SPOOFED_MAC***

=> Việc dùng địa chỉ giả mạo chỉ hiệu quả trong một số điều kiện nhất định, thay vào đó có thể sử dụng decoys - mồi nhử. 

- Làm cho quá trình quét dường như đến từ nhiều địa chỉ IP để địa chỉ IP của kẻ tấn công sẽ bị mất trong số đó. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image54.png)

=> Cú pháp, sử dụng tùy chọn ***-D***. Ví dụ: ***nmap -D 10.10.0.1,10.10.0.2,ME 10.10.173.49***, trong đó ***10.10.173.49*** là địa chỉ IP mục tiêu. 

**Task 6: Fragmented Packets**

**Firewall**

- Firewall là một phần mềm hoặc phần cứng cho phép gói tin đi qua hoặc chặn chúng. Hoạt động dựa trên firewall rules. 

- Traditional firewall ít nhất sẽ kiểm tra IP header và Transport layer header. 

- Firewall phức tạp hơn cũng sẽ cố gắng kiểm tra dữ liệu được mang theo bởi Transport layer.

**IDS** - Intrusion detection system 

- Kiểm tra gói tin mạng để tìm các mẫu hành vi chọn lọc hoặc chữ ký nội dung cụ thể. 

- Đưa ra cảnh báo bất cứ khi nào gặp phải một quy tắc độc hại. 

- Ngoài IP header và Transport layer header, IDS sẽ kiểm tra nội dung dữ liệu trong transport layer và kiểm tra xem nó có khớp với bất kỳ mẫu độc hại nào không.

=> Để giảm khẳ năng phát hiện Nmap scanning của Firewall / IDS, có thể chia gói tin thành các gói tin nhỏ hơn. 

**Fragmented Packets**

- Nmap sử dụng tùy chọn **-f** để phân mảnh gói tin. 

- Sau khi được chọn, dữ liệu IP sẽ được chia thành 8 byte trở xuống

- Việc thêm -f (-f -f hoặc -ff) khác sẽ chia dữ liệu thành các đoạn 16 byte thay vì 8.

- Có thể sử dụng tùy chọn ***--mtu*** để thay đổi giá trị mặc định, tuy nhiên phải luôn chọn bội số của 8

- Để hỗ trợ việc lắp ráp lại gói tin ở phía người nhận, IP sử dụng identification (ID) và fragment offset

- Nếu muốn tăng kích thước gói tin để làm cho chúng trông vô hại, sử dụng tùy chọn ***--data-length NUM***, trong đó num chỉ định số byte muốn thêm vào gói tin.

**Task 7: Idle / Zombie Scan**

- Việc giả mạo địa chỉ IP nguồn chỉ hữu ích trong 1 số tình huống, cần phải ở vị trí có thể giám sát lưu lượng. 

=> Sử dụng Idle scan hoặc gọi là zombie scan, sử dụng 1 máy tính "rảnh rỗi" trên mạng. Nmap sẽ giả mạo các gói tin quét như thể chúng đến từ máy zombie, sau đó kiểm tra phản hồi từ máy này bằng cách theo dõi giá trị IP ID trong IP header.

- Cú pháp ***nmap -sI ZOMBIE_IP MACHINE_IP***

- Quy trình Idle scan gồm 3 bước: 

1. Kích hoạt máy zombie để ghi lại giá trị IP ID hiện tại.

2. Gửi gói tin SYN giả mạo đến cổng trên mục tiêu, làm cho mục tiêu nghĩ rằng gói tin đến từ máy zombie.

3. Kích hoạt lại máy zombie để kiểm tra sự thay đổi của IP ID, từ đó xác định cổng trên mục tiêu có mở hay không.

=> Setup:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image55.png)

=> Trường hợp cổng đóng:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image56.png)

=> Trường hợp cổng mở: (IP ID của máy zombie tăng lên)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/imag57.png)

=> Trong trường hợp thứ ba, máy mục tiêu hoàn toàn không phản hồi do các quy tắc tường lửa. Việc thiếu phản hồi này sẽ dẫn đến kết quả tương tự như với cổng đóng; idle host sẽ không tăng IP ID.

- Bước cuối, kẻ tấn công gửi tiếp gói tin có cờ SYN/ACK đến máy zomie 1 lần nữa, máy zombie gửi lại gói tin có cờ RST => tăng IP ID 1 lần nữa. 

=> kẻ tấn công so sánh IP ID giữa bước setup và bước cuối này, nếu chênh lệch  1 nghĩa là cổng bên máy mục tiêu đóng, nếu chênh lệch 2 có nghĩa cổng mở. 

**Task 8: Getting More Details**

- Thêm tùy chọn ***--reason** nếu muốn Nmap cung cấp thêm lý do cho kết luận của nó.

    pentester@TryHackMe$ sudo nmap -sS 10.10.252.27


    Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:39 BST
    Nmap scan report for ip-10-10-252-27.eu-west-1.compute.internal (10.10.252.27)
    Host is up (0.0020s latency).
    Not shown: 994 closed ports
    PORT    STATE SERVICE
    22/tcp  open  ssh
    25/tcp  open  smtp
    80/tcp  open  http
    110/tcp open  pop3
    111/tcp open  rpcbind
    143/tcp open  imap
    MAC Address: 02:45:BF:8A:2D:6B (Unknown)


    Nmap done: 1 IP address (1 host up) scanned in 1.60 seconds


    pentester@TryHackMe$ sudo nmap -sS --reason 10.10.252.27


    Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:40 BST
    Nmap scan report for ip-10-10-252-27.eu-west-1.compute.internal (10.10.252.27)
    Host is up, received arp-response (0.0020s latency).
    Not shown: 994 closed ports
    Reason: 994 resets
    PORT    STATE SERVICE REASON
    22/tcp  open  ssh     syn-ack ttl 64
    25/tcp  open  smtp    syn-ack ttl 64
    80/tcp  open  http    syn-ack ttl 64
    110/tcp open  pop3    syn-ack ttl 64
    111/tcp open  rpcbind syn-ack ttl 64
    143/tcp open  imap    syn-ack ttl 64
    MAC Address: 02:45:BF:8A:2D:6B (Unknown)


    Nmap done: 1 IP address (1 host up) scanned in 1.59 seconds

=> Kết quả ở trên cho thấy hệ thống này được coi là trực tuyến vì Nmap "đã nhận được phản hồi arp".

- Tùy chọn ***-v*** hoặc ***-vv*** có thể cung cấp chi tiết hơn kết quả output.

**Task 9: Summary**

- TCP Null Scan: ***nmap -sN TARGET***

- TCP FIN Scan: ***nmap -sF TARGET***

- TCP Xmas Scan: ***nmap -sX TARGET***

- TCP Maimon Scan: ***nmap -sM TARGET***

- TCP ACK Scan: ***nmap -sA TARGET***

- TCP Window Scan: ***nmap -sW TARGET***

- Custom TCP Scan: ***nmap --scanflags URGACKPSHRSTSYNFIN 10.10.10.187***

- Spoofed Source IP: ***nmap -S SPOOFED_IP TARGET***

- Spoofed MAC Address: ***nmap -spoof-mac SPOOFED_MAC***

- Decoy Scan: ***nmap -D DECOY_IP,ME 10.10.10.187***

- Idle (Zombie) Scan: ***sudo nmap -sI ZOMBIE_IP 10.10.10.187***

- Fragment IP data into 8 bytes: ***-f***

- Fragment IP data into 16 bytes: ***-ff***

- --source-port PORT_NUM: specify source port number

- --data-length NUM: append random data to reach given length

=> Null, FIN, Xmas Scan tạo ra phản hồi từ các cổng đã đóng. Maimon, ACK và Windows Scan tạo ra phản hồi từ các cổng mở và đóng. 




