# Active Reconnaissance

**Task 1: Introduction**

- Sử dụng trình duyệt web để thu thập thông tin

- sử dụng các công cụ đơn giản như ping, traceroute, telnet và nc để thu thập thông tin về mạng, hệ thống và dịch vụ.

**Task 2: Web Browser**

- Vì 80 và 443 là cổng mặc định cho HTTP và HTTPS nên web browser không hiển thị chúng trên thanh địa chỉ. Tuy nhiên, có thể sử dụng các cổng tùy chỉnh để truy cập một dịch vụ. Chẳng hạn, https://127.0.0.1:8834/ sẽ kết nối với 127.0.0.1 (localhost) tại cổng 8834 thông qua giao thức HTTPs.

- Với Developer tools được tích hợp sẵn trên các trình duyệt, có thể xem và thậm chí sửa đổi các tệp JavaScript (JS), kiểm tra các cookie được đặt trên hệ thống và khám phá cấu trúc thư mục của nội dung trang web.

- Một số extensions có thể giúp ích trong quá trình kiểm thử xâm nhập: 

    - FoxyProxy cho phép nhanh chóng thay đổi Proxy Server đang sử dụng để truy cập trang web mục tiêu.

    - User-Agent Switcher and Manager cung cấp khả năng giả vờ truy cập trang web từ một hệ điều hành khác hoặc web browser khác. 

    - Wappalyzer cung cấp thông tin chi tiết về công nghệ được sử dụng trên các trang web được truy cập.

**Task 3: Ping**

- Mục đích chính của ping là kiểm tra xem có thể truy cập hệ thống từ xa hay không và hệ thống từ xa có thể liên lạc lại hay không, hay nói cách khác: kiểm tra xem hệ thống từ xa có trực tuyến hay không.

- Lệnh ping sẽ gửi gói tin ICMP Echo đến hệ thống từ xa và nếu gói tin được định tuyến chính xác, không bị chặn bởi firewall nào thì hệ thống sẽ phản hồi ICMP Echo Reply. Bằng cách này, có thể kết luận hệ thống có đang trực tuyến hay không. 

- Cú pháp ***ping + MACHINE_IP*** hoặc ***ping + HOSTNAME***. Để chỉ định số packet sẽ gửi sử dụng tùy chọn ***-c + NUM_OF_PACKETS***, ví dụ:
***ping -c 10 MACHINE_IP*** (Linux) hoặc ***ping -n 10 MACHINE_IP*** (Windows).

![img](9)

- Nếu hệ thống từ xa không trực tuyến: 

![img](10)

- Tùy chọn sử dụng để set size của data khi ping là ***-s***.

- ICMP header có kích cỡ là 8 bytes.

**Task 4: Traceroute**

- Lệnh traceroute theo dõi lộ trình được thực hiện bởi các packets từ hệ thống đến server khác.

- Mục đích của traceroute là tìm địa chỉ IP của routers hoặc hops mà gói tin đi qua khi nó đi từ hệ thống đến server mục tiêu.

- Lệnh này cũng tiết lộ số lượng routers giữa hai hệ thống. 

- Cú pháp trên Linux: ***traceroute 10.10.27.158*** và trên Windows là ***tracert 10.10.27.158***.

- Mặc dù T trong TTL là viết tắt của Time, TTL cho biết số lượng Routers/ Hops tối đa mà gói tin có thể đi qua trước khi bị loại bỏ; TTL không phải là số đơn vị thời gian tối đa.

- Khi một router nhận được một gói tin, nó sẽ giảm TTL đi một trước khi chuyển nó sang router tiếp theo. 

![img](11)

=> Mỗi lần gói tin đi qua router, giá trị TTL của nó sẽ giảm đi 1. Ban đầu, nó rời khỏi hệ thống với giá trị TTL là 64; nó đến hệ thống đích với giá trị TTL là 60 sau khi đi qua 4 routers.

- Tuy nhiên, nếu TTL đạt đến 0, nó sẽ bị dropped. 

![img](12)

=> Hệ thống đặt TTL thành 1 trước khi gửi nó đến router. Router đầu tiên trên đường dẫn giảm TTL đi 1, dẫn đến TTL bằng 0. Do đó, Router này sẽ loại bỏ packet và gửi thông báo lỗi vượt quá thời gian truyền ICMP.

=> TTL=1 sẽ tiết lộ địa chỉ IP của router đầu tiên. Sau đó nó sẽ gửi một gói tin khác có TTL=2; gói này sẽ bị dropped ở router thứ hai. Và vân vân.

    user@AttackBox$ traceroute tryhackme.com
    traceroute to tryhackme.com (172.67.69.208), 30 hops max, 60 byte packets
    1  ec2-3-248-240-5.eu-west-1.compute.amazonaws.com (3.248.240.5)  2.663 ms * ec2-3-248-240-13.eu-west-1.compute.amazonaws.com (3.248.240.13)  7.468 ms
    2  100.66.8.86 (100.66.8.86)  43.231 ms 100.65.21.64 (100.65.21.64)  18.886 ms 100.65.22.160 (100.65.22.160)  14.556 ms
    3  * 100.66.16.176 (100.66.16.176)  8.006 ms *
    4  100.66.11.34 (100.66.11.34)  17.401 ms 100.66.10.14 (100.66.10.14)  23.614 ms 100.66.19.236 (100.66.19.236)  17.524 ms
    5  100.66.7.35 (100.66.7.35)  12.808 ms 100.66.6.109 (100.66.6.109)  14.791 ms *
    6  100.65.14.131 (100.65.14.131)  1.026 ms 100.66.5.189 (100.66.5.189)  19.246 ms 100.66.5.243 (100.66.5.243)  19.805 ms
    7  100.65.13.143 (100.65.13.143)  14.254 ms 100.95.18.131 (100.95.18.131)  0.944 ms 100.95.18.129 (100.95.18.129)  0.778 ms
    8  100.95.2.143 (100.95.2.143)  0.680 ms 100.100.4.46 (100.100.4.46)  1.392 ms 100.95.18.143 (100.95.18.143)  0.878 ms
    9  100.100.20.76 (100.100.20.76)  7.819 ms 100.92.11.36 (100.92.11.36)  18.669 ms 100.100.20.26 (100.100.20.26)  0.842 ms
    10  100.92.11.112 (100.92.11.112)  17.852 ms * 100.92.11.158 (100.92.11.158)  16.687 ms
    11  100.92.211.82 (100.92.211.82)  19.713 ms 100.92.0.126 (100.92.0.126)  18.603 ms 52.93.112.182 (52.93.112.182)  17.738 ms
    12  99.83.69.207 (99.83.69.207)  17.603 ms  15.827 ms  17.351 ms
    13  100.92.9.83 (100.92.9.83)  17.894 ms 100.92.79.136 (100.92.79.136)  21.250 ms 100.92.9.118 (100.92.9.118)  18.166 ms
    14  172.67.69.208 (172.67.69.208)  17.976 ms  16.945 ms 100.92.9.3 (100.92.9.3)  17.709 ms

=> Trong đầu ra traceroute ở trên, có 14 dòng được đánh số; mỗi dòng đại diện cho một router/hop. Hệ thống gửi ba gói tin có TTL được đặt thành 1, sau đó ba gói có TTL được đặt thành 2, v.v.

- Số hops/routers giữa hệ thống nguồn và hệ thống đích phụ thuộc vào thời gian chạy traceroute. Không có gì đảm bảo rằng các gói tin sẽ luôn đi theo cùng một route, ngay cả khi ở trên cùng một mạng hoặc lặp lại lệnh traceroute trong một thời gian ngắn.

- Một số router trả về địa chỉ IP public. Có thể kiểm tra một số router này dựa trên phạm vi kiểm thử xâm nhập dự kiến.

- Một số router không trả về reply. 

**Task 5: Telnet**

- Lệnh telnet sử dụng giao thức TELNET để quản trị từ xa. Cổng mặc định được telnet sử dụng là 23. 

- Từ góc độ bảo mật, telnet gửi tất cả dữ liệu, bao gồm username và password, ở dạng văn bản rõ. Giải pháp thay thế an toàn là giao thức SSH (Secure SHell).

- Có thể sử dụng Telnet để kết nối với bất kỳ dịch vụ nào và lấy biểu ngữ của dịch vụ đó. 

- Cú pháp ***telnet 10.10.27.158 PORT***. 

![img](13)

=> Điều đặc biệt quan tâm là khám phá ra loại và phiên bản của web server đã cài đặt, Server: Apache/2.4.61 (Debian). 

**Task 6: Netcat**

- Netcat hỗ trợ cả giao thức TCP và UDP. Nó có thể hoạt động như một client kết nối với listening port; Ngoài ra, nó có thể hoạt động như một listening port trên port được chọn. Do đó, đây là một công cụ tiện lợi có thể sử dụng như một client hoặc server đơn giản qua TCP hoặc UDP.

- Cú pháp ***nc 10.10.27.158 PORT***: 

![img](14)

=> Trong terminal ở trên, sử dụng netcat để kết nối với 10.10.27.158 cổng 80 bằng ***nc 10.10.27.158 80***. Tiếp theo, yêu cầu GET cho default page bằng ***GET / HTTP/1.1***; chỉ định cho server mục tiêu rằng client hỗ trợ HTTP phiên bản 1.1. Cuối cùng, cần đặt tên cho host, vì vậy thêm vào một dòng mới, ***host: netcat***.

- Cú pháp đầy đủ ***nc -lvnp 1234***.

- ***-l***: Listen mode

- ***-p***: Port number

- ***-n***: Numeric only; no resolution of hostnames via DNS (Chỉ số; không phân giải tên máy chủ qua DNS)

- ***-v***: Verbose output (optional, yet useful to discover any bugs) (Đầu ra chi tiết (tùy chọn, nhưng hữu ích để phát hiện bất kỳ lỗi nào))

- ***-vv***: Very Verbose (optional)

- ***-k***: Keep listening after client disconnects

- Đối với server-side, để lắng nghe trên cổng 1234, sử dụng lệnh ***nc -lvnp 1234***. Phía client, kết nối tới server bằng lệnh ***nc 10.10.27.158 1234*** (IP là IP của server). 

**Task 7: Putting It All Together**

- sử dụng traceroute để ánh xạ đường dẫn đến mục tiêu

- ping để kiểm tra xem hệ thống đích có phản hồi với ICMP Echo hay không 

- telnet để kiểm tra xem cổng nào đang mở và có thể truy cập được bằng cách thử kết nối với chúng
















