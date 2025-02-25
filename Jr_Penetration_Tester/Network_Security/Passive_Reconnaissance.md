# Passive Reconnaissance

**Task 1: Introduction**

=> Tool: 

- whois 

- nslookup

- dig

=> Online tool: 

- DNSDumpster

- Shodan.io

**Task 2: Passive Versus Active Recon**

- Nếu đang đóng vai kẻ tấn công, cần thu thập thông tin về hệ thống mục tiêu. Nếu đang đóng vai người bảo vệ, cần biết đối thủ sẽ khám phá những gì về hệ thống và mạng.

- Reconnaissance - Trinh sát (recon) có thể được định nghĩa là khảo sát sơ bộ để thu thập thông tin về mục tiêu.

- Trong Passive Reconnaissance, cần dựa vào kiến ​​thức có sẵn công khai. Đó là kiến ​​thức có thể truy cập từ các nguồn tài nguyên có sẵn công khai mà không cần tương tác trực tiếp với mục tiêu. "Nhìn lãnh thổ từ xa mà không đặt chân lên lãnh thổ đó". 

- Mặt khác, Active Reconnaissance không thể thực hiện được một cách kín đáo như vậy, đòi hỏi sự tham gia trực tiếp với mục tiêu.

**Task 3: Whois**

- WHOIS là giao thức yêu cầu và phản hồi tuân theo đặc tả RFC 3912. WHOIS server lắng nghe trên cổng TCP 43 để nhận các yêu cầu gửi đến.

- WHOIS server trả lời với nhiều thông tin khác nhau liên quan đến domain được yêu cầu.

- Bằng cách sử dụng AttackBox (hoặc máy Linux cục bộ, ví dụ như Parrot hoặc Kali), có thể dễ dàng truy cập whois client trên thiết bị đầu cuối. Cú pháp là whois DOMAIN_NAME, trong đó DOMAIN_NAME là miền đang cố gắng lấy thêm thông tin

![img](0)

**Task 4: nslookup and dig**

- Tìm địa chỉ IP của tên miền bằng cách sử dụng nslookup, viết tắt của Name Server Look Up. Cú pháp nslookup DOMAIN_NAME. Đầy đủ hơn nslookup + OPTIONS + DOMAIN_NAME + SERVER.

- SERVER là DNS server muốn truy vấn. Có thể chọn bất kỳ local DNS server hoặc public nào để truy vấn. Cloudflare cung cấp 1.1.1.1 và 1.0.0.1, Google cung cấp 8.8.8.8 và 8.8.4.4, còn Quad9 cung cấp 9.9.9.9 và 149.112.112.112.

- OPTIONS chứa loại truy cấn. Ví dụ, A cho địa chỉ IPv4 hoặc AAAA cho địa chỉ IPv6. 

- Ví dụ: ***nslookup -type=A tryhackme.com 1.1.1.1*** (hoặc nslookup -type=a tryhackme.com 1.1.1.1 vì nó không phân biệt chữ hoa chữ thường) có thể được sử dụng để trả về tất cả các địa chỉ IPv4 được tryhackme.com sử dụng.

![img](1)

- Giả sử muốn tìm hiểu về mail servers và cấu hình của một tên miền cụ thể. Có thể dùng lệnh ***nslookup -type=MX tryhackme.com***.

![img](2)

- Để có các truy vấn DNS nâng cao hơn và chức năng bổ sung, có thể sử dụng ***dig***, từ viết tắt của "Domain Information Groper".  Cú pháp dig + DOMAIN_NAME + TYPE.

![img](3)

=> So sánh nhanh giữa đầu ra của nslookup và dig cho thấy dig trả về nhiều thông tin hơn, chẳng hạn như TTL (Time To Live) theo mặc định. 

![img](4)

![img](5)

**Task 5: DNSDumpster**

- Các công cụ tra cứu DNS, chẳng hạn như nslookup và dig, không thể tự tìm thấy tên miền phụ. Tên miền đang kiểm tra có thể bao gồm một tên miền phụ khác có thể tiết lộ nhiều thông tin về mục tiêu.

- Có thể sử dụng dịch vụ trực tuyến cung cấp câu trả lời chi tiết cho các truy vấn DNS, chẳng hạn như DNSDumpster.

![img](6)

- DNSDumpster cũng sẽ thể hiện thông tin được thu thập bằng đồ họa. DNSDumpster hiển thị dữ liệu từ bảng trước đó dưới dạng biểu đồ.

![img](7)

**Task 6: Shodan.io**

- Một dịch vụ như Shodan.io có thể hữu ích để tìm hiểu nhiều thông tin khác nhau về mạng của khách hàng mà không cần chủ động kết nối với mạng đó.

- Shodan.io cố gắng kết nối với mọi thiết bị có thể truy cập trực tuyến để xây dựng một công cụ tìm kiếm các "thứ" được kết nối trái ngược với công cụ tìm kiếm cho các trang web. Sau khi nhận được phản hồi, nó sẽ thu thập tất cả thông tin liên quan đến dịch vụ và lưu vào cơ sở dữ liệu để có thể tìm kiếm được.

![img](8)

**Task 7: Summary**

- Lookup WHOIS records: ***whois tryhackme.com***

- Lookup DNS A records: **nslookup -type=A tryhackme.com***

- Lookup DNS MX records at DNS server: ***nslookup -type=MX tryhackme.com 1.1.1.1***

- Lookup DNS TXT records: ***nslookup -type=TXT tryhackme.com***

- Lookup DNS A records: ***dig tryhackme.com A***

- Lookup DNS MX records at DNS server: ***dig @1.1.1.1 tryhackme.com MX***

- Lookup DNS TXT records: ***dig tryhackme.com TXT***













