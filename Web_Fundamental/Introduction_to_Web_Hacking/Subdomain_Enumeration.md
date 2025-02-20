# Subdomain Enumeration

***Subdomain enumeration*** (Liệt kê tên miền phụ) là quá trình tìm kiếm subdomain hợp lệ cho 1 domain, tại sao phải làm điều này? Câu trả lời là để ***mở rộng phạm vi tấn công***, nhằm thử và khám phá thêm các điểm dễ bị tổn thương tiềm ẩn. Khám phá 3 phương pháp liệt kê subdomain khác nhau: ***Brute Force***, ***OSINT*** và ***Virtual Host***.

***OSINT - SSL/TLS Certificates***

Mục đích của Certificate Transparency là ngăn việc sử dụng các chứng chỉ độc hại và vô tình được tạo. Có thể sử dụng dịch vụ này để khám phá các subdomain thuộc 1 domain. Trang web như https://crt.sh và https://ui.ctsearch.entrust.com/ui/ctsearchui cung cấp ***cơ sở dữ liệu chứng chỉ*** có thể tìm kiếm, hiển thị kết quả hiện tại và trong quá khứ.

Ví dụ: truy cập ***crt.sh*** và tìm domain ***tryhackme.com** và tìm domain name đã được truy cập vào 2020-12-26:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image39.png?raw=true)

=> Domain name được truy cập vào 2020-12-26: ***store.tryhackme.com***

***OSINT - Search Engines***

Việc sử dụng các phương pháp tìm kiếm nâng cao trên website như Google, ví dụ như ***site:filter***, có thể thu hẹp kết quả tìm kiếm. Ví dụ: ***site:*.domain.com -site:www.domain.com*** sẽ chỉ chứa các kết quả dẫn đến domain ***domain.com*** nhưng loại trừ mọi liên kết đến ***www.domain.com***; do đó, nó sẽ hiển thị subdomain thuộc về ***domain.com***

***DNS Bruteforce***

***Bruteforce DNS*** (Domain Name System) Enumeration là phương pháp thử hàng chục, hàng trăm, hàng nghìn hoặc thậm chí hàng triệu subdomain khác nhau có thể có từ danh sách được xác định trước, gồm các subdomain thường được sử dụng. Vì phương pháp này yêu cầu nhiều requests, nên cần tool để giúp quá trình diễn ra nhanh hơn, trong trường hợp này là tool ***dnsrecon***.

Ví dụ: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image40.png?raw=true)

Câu lệnh ***dnsrecon -t brt -d acmeitsupport.thm*** được dùng để dò tìm các bản ghi DNS thông qua phương pháp brute-force.

- Tùy chọn ***-t*** : chỉ định loại kiểm tra

    => ***-t brt***: chọn loại kiểm tra là brt - viết tắt của brute-force

- Tùy chọn ***-d***: chỉ định target domain, trong TH này là ***acmeitsupport.thm***

***OSINT - Sublist3r***

Tự động hóa bằng ***Sublist3r*** để tăng tốc quá trình OSINT khám phá subdomain, có thể tự động hóa phương pháp này với sự trợ giúp của tool như ***Sublist3r***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image41.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image42.png?raw=true)

***Virtual Hosts***

Thử lệnh sau để khám phá subdomain mới của website Acme IT Support:

***ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.92.70***

=> Kết quả: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image43.png?raw=true)

Lệnh này sử dụng tùy chọn ***-w*** để chỉ định wordlist sẽ sử dụng (trong TH này là namelists.txt). Tùy chọn ***-H*** để adds /edits 1 header (trong TH này là Host header), có từ khóa ***FUZZ*** trong khoảng trống nơi subdomain thường xuất hiện và đây là nơi sẽ thử tất cả tùy chọn từ wordlist.

Bởi vì lệnh trên sẽ luôn cho ra kết quả hợp lệ nên cần lọc output. Có thể làm điều này bằng cách sử dụng the page size, tùy chọn ***-fs***. Chỉnh sửa lệnh sau, thay thế {size} bằng giá trị size xuất hiện nhiều nhất từ lệnh trước:

***ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.92.70 -fs {size}***

(Giá trị size xuất hiện nhiều nhất từ lệnh trước: ***2395***)

=> Câu lệnh mới:  

***ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.92.70 -fs 2395***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image44.png?raw=true)

=> Lọc ra được 2 subdomain: 
- ***delta***  

- ***yellow***






