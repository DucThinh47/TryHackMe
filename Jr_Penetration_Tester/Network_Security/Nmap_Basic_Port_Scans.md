# Nmap Basic Port Scans

**Task 1: Introduction**

Kiểm tra ports nào đang mở và đang lắng nghe cũng như ports nào đã đóng.

**Task 2: TCP and UDP Ports**

- TCP port hoặc UDP Port được sử dụng để xác định dịch vụ mạng đang chạy trên host đó.

- Một cổng thường được liên kết với một dịch vụ bằng số cổng cụ thể. HTTP server sẽ liên kết với TCP port 80 theo mặc định; hơn nữa, nếu HTTP Server hỗ trợ SSL/TLS, nó sẽ lắng nghe trên TCP port 443. (TCP Port 80 và 443 là port mặc định cho HTTP và HTTPS)

- Cổng mở cho biết có một số dịch vụ đang lắng nghe trên cổng đó.

- Cổng đóng chỉ ra rằng không có dịch vụ nào đang nghe trên cổng đó.

- Đôi khi, cổng mở nhưng firewall đã chặn gói tin gửi đến.

- ***Open***: chỉ ra rằng một dịch vụ đang lắng nghe trên cổng được chỉ định

- ***Closed***: cho biết rằng không có dịch vụ nào đang lắng nghe trên cổng được chỉ định, mặc dù cổng đó có thể truy cập được.

- ***Filtered***: có nghĩa là Nmap không thể xác định được cổng đang mở hay đóng vì cổng này không thể truy cập được.

- ***Unfiltered***: có nghĩa là Nmap không thể xác định cổng đang mở hay đóng, mặc dù cổng đó có thể truy cập được

- ***Open|Filtered***: Điều này có nghĩa là Nmap không thể xác định xem cổng đang mở hay được lọc.

- ***Closed|Filtered***: Điều này có nghĩa là Nmap không thể quyết định liệu một cổng có bị đóng hay bị lọc hay không.

- DNS sử dụng UDP cổng 53 theo mặc định

- SSH sử dụng TCP cổng 22 theo mặc định

**Task 2: TCP Flags**

**TCP header flags**: 

1. ***URG: Urgent flag*** - Cờ khẩn cấp cho biết rằng urgent pointer - con trỏ khẩn cấp được gửi. Con trỏ khẩn cấp cho biết rằng dữ liệu đến là khẩn cấp và TCP Segment có URG flag được đặt sẽ được xử lý ngay lập tức mà không cần phải đợi các TCP Segment đã gửi trước đó.

2. ***ACK: Acknowledgment flag*** - Cờ xác nhận chỉ ra rằng acknowledgement number - số xác nhận là có ý nghĩa. Nó được sử dụng để xác nhận việc nhận được TCP segment.

3. ***PSH: Push Flag*** - Cờ đẩy yêu cầu TCP truyền dữ liệu tới ứng dụng kịp thời.

4. ***RST: Reset Flag*** - Cờ reset được sử dụng để thiết lập lại kết nối. Một thiết bị khác, chẳng hạn như firewall, có thể gửi nó để phá vỡ kết nối TCP. Cờ này cũng được sử dụng khi dữ liệu được gửi đến host và không có dịch vụ nào ở đầu nhận trả lời.

5. ***SYN: Synchronize flag*** - Cờ đồng bộ hóa được sử dụng để bắt đầu bắt tay 3 bước TCP và đồng bộ hóa sequence number với hosts khác. Sequence number phải được đặt ngẫu nhiên trong quá trình thiết lập kết nối TCP.

6. ***FIN flag*** -  Người gửi không còn dữ liệu để gửi.

**Task 4: TCP Connect Scan**

- Hoạt động bằng cách hoàn thành quá trình bắt tay 3 bước TCP.

- Trong thiết lập kết nối TCP tiêu chuẩn, client gửi TCP packet có set TCP SYN flag và server phản hồi bằng SYN/ACK nếu port mở; cuối cùng, client hoàn tất quá trình bắt tay 3 bước bằng cách gửi ACK.

- Nmap TCP connect scan: ***nmap -sT TARGET***, lệnh sẽ yêu cầu Nmap thực hiện quét cổng bằng cách thiết lập kết nối TCP đầy đủ. 

![img](25)

- Sử dụng TCP connect scan để khám phá các cổng TCP đang mở khi không phải người dùng đặc quyền.

- Theo mặc định, Nmap sẽ cố gắng kết nối với 1000 cổng phổ biến nhất.

- Với cổng TCP đóng, nó sẽ phản hồi request với gói SYN được set RST/ACK flag. 

![img](26)

=> cổng 143 đang mở nên cổng đã phản hồi bằng gói SYN được set SYN/ACK flag và Nmap hoàn thành quá trình bắt tay 3 bước bằng cách gửi lại gói tin được set ACK flag. 

![img](27)

- Nếu muốn quét nhanh hơn, thêm tùy chọn ***-F***, sẽ giảm số lượng cổng được quét từ 1000 xuống 100 cổng phổ biến nhất.

- Tùy chọn ***-r*** cũng có thể được thêm vào để quét ports theo thứ tự liên tiếp thay vì thứ tự ngẫu nhiên. Tùy chọn này hữu ích khi kiểm tra xem ports có mở một cách nhất quán hay không

![img](28)

**Task 5: TCP SYN Scan**

- Chế độ quét mặc định là SYN scan và nó yêu cầu người dùng có đặc quyền (root hoặc sudoer) để chạy nó.

- SYN scan không cần hoàn tất quá trình bắt tay 3 bước TCP; thay vào đó, nó sẽ ngắt kết nối sau khi nhận được response từ server => giảm khả năng bị phát hiện quá trình quét.

- Sử dụng tùy chọn ***-sS***. 

![img](29)

- So sánh trong Wireshark giữa TCP connect scan và TCP SYN scan: 

=> *TCP connect scan*:

![img](30)

=> *TCP SYN scan*:

![img](31)

- TCP SYN scan là chế độ quét mặc định khi chạy Nmap với tư cách người dùng đặc quyền, chạy bằng root hoặc sử dụng sudo và đây là một lựa chọn rất đáng tin cậy.

![img](32)

**Task 6: UDP Scan**

- UDP là một protocol không có kết nối và do đó nó không yêu cầu bất kỳ sự bắt tay nào để thiết lập kết nối.

- Không thể đảm bảo rằng dịch vụ chạy trên cổng UDP sẽ phản hồi các gói tin. Tuy nhiên, nếu gói tin UDP được gửi đến một cổng đóng, lỗi ***ICMP port unreachable(type 3, code 3)*** sẽ được trả về

- Sử dụng tùy chọn ***-sU***. 

- Việc gửi gói tin UDP tới một cổng mở sẽ không phản hồi bất cứ điều gì.

![img](33)

![img](34)

=> Cổng 111 đang mở, cổng 68 không xác định được đang mở hay được lọc. 

![img](35)

**Task 7: Fine-Tuning Scope and Performance**

- Có thể chỉ định cổng muốn quét thay vì 1000 cổng mặc định.

- ***-p22,80,443*** sẽ quét cổng 22, 80 và 443.

- ***-p1-1023*** sẽ quét tất cả các cổng từ 1 đến 1023, trong khi ***-p20-25*** sẽ quét các cổng từ 20 đến 25.

- Yêu cầu quét tất cả cổng bằng cách sử dụng ***-p-***, thao tác này sẽ quét tất cả 65535 cổng. Nếu muốn quét 100 cổng phổ biến nhất, thêm ***-F***. Sử dụng ***--top-ports 10*** sẽ quét 10 cổng phổ biến nhất.

- Kiểm soát thời gian quét bằng cách sử dụng ***-T<0-5>***. ***-T0*** là chậm nhất (paranoid), trong khi ***-T5*** là nhanh nhất. 

- Mặc định Nmap sử dụng ***-T3***. 

- Có thể chọn kiểm soát packet rate bằng cách sử dụng ***--min-rate < number >*** và ***--max-rate < number >***. Ví dụ: ***--max-rate 10*** hoặc ***--max-rate=10*** đảm bảo rằng máy quét không gửi quá 10 gói tin mỗi giây.

- Có thể kiểm soát việc song song hóa việc thăm dò bằng cách sử dụng ***--min-parallelism < numprobes >*** và ***--max-parallelism < numprobes >***. Nmap thăm dò các mục tiêu để khám phá live hosts và open ports; song song hóa thăm dò chỉ định số lượng các đầu dò như vậy có thể chạy song song. Ví dụ: ***--min-parallelism=512*** yêu cầu Nmap duy trì song song ít nhất 512 đầu dò

**Task 8: Summary**

- TCP Connect Scan: ***nmap -sT TARGET***

- TCP SYN Scan: ***nmap -sS TARGET***

- UDP Scan: ***nmap -sU TARGET***

- ***-p-***: scan all ports

- ***-p1-1023***: scan port 1 to 1023

- ***-F***: 100 most common ports

- ***-r***: scan ports in consecutive order - quét cổng theo thứ tự liên tiếp

- ***-T<0-5>***: -T0 being the slowest and T5 the fastest

- ***--max-rate 50***: rate <= 50 packets/sec

- ***--min-rate 15***: rate >= 15 packets/sec

- ***--min-parallelism 100***: at least 100 probes in parallel



























