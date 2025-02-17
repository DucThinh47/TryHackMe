# DNS in details

1. Gía trị bản ghi **CNAME** của domain **shop.website.thm** là gì?

Sử dụng câu lệnh ***nslookup --type=CNAME shop.website.thm***, trong đó: 

- ***nslookup***: công cụ truy vấn thông tin DNS.

- ***--type=CNAME***: chỉ định bản ghi cần truy vấn là **CNAME**.

    *(Bản ghi **CNAME** sẽ phân giải domain name gốc thành một domain name khác)*

- ***shop.website.thm***: domain name cần truy vấn: 

![img](0)

Phân tích kết quả trả về: 

- ***Server: 127.0.0.53***: IP address của local DNS server (là address trên máy tính cục bộ).

- ***Address: 127.0.0.53#53***: IP address và port number (53 - UDP 53: default port cho DNS) của DNS server.

- ***Non-authoritative answer***: kết quả không đến từ một DNS server chính thức (authoritative DNS) của domain name website.thm, mà từ một DNS server trung gian (caching DNS).

- ***shops.myshopify.com***: CNAME record của domain name shop.website.thm.

2. Giá trị bản ghi **TXT** của domain name website.thm là bao nhiêu? 

*(Bản ghi TXT là các trường văn bản, nơi lưu trữ mọi dữ liệu dựa trên văn bản. Bản ghi thường được dùng để **liệt kê servers** có quyền gửi email thay mặt cho domain; cũng có thể dùng để **xác minh quyền sở hữu domain** khi đăng ký dịch vụ bên thứ 3)*

Sử cụng câu lệnh tương tự như trên, chỉ thay --type thành bản ghi TXT, ***nslookup --type=TXT website.thm***:

![img](1)

Kết quả trả về giống với bản ghi CNAME, chỉ thay thành giá trị bản ghi TXT.

3. Giá trị thứ tự ưu tiên cho bản ghi MX của domain website.thm là bao nhiêu? 

Tương tự như trên, chỉ thay --type thành bản ghi MX, ***nslookup --type=MX website.thm***:

*(Bản ghi MX **phân giải theo địa chỉ Mail Server** cho domain name đang truy vấn. Những bản ghi này đi kèm với **cờ ưu tiên**, cho cline-side biết thứ tự thử server nếu server chính gặp sự cố và email cần gửi đến 1 server dự phòng)*

![img](2)

Phân tích kết quả trả về:

- ***website.thm mail exchanger***: bản ghi MX cho biết mail server chịu trách nhiệm nhận email gửi đến domain name website.thm.

- ***30***: giá trị ưu tiên của bản ghi MX. Số càng nhỏ, ưu tiên càng cao. Trong trường hợp có nhiều bản ghi MX, server sẽ thử gửi email đến server có giá trị ưu tiên thấp nhất trước.

- ***alt4.aspmx.l.google.com***: address của mail server mà domain name website.thm sử dụng.

4. IP address cho bản ghi A của website www.website.thm? 

*(Bản ghi A phân giải domain name thành **IPv4 addresses**)*

Sử dụng lệnh tương tự, thay --type thành bản ghi A, ***nslookup --type=A website.thm***: 

![img](3)

Phân tích kết quả trả về: 

- ***Name: website.thm***: domain name cần phân giải thành địa chỉ IPv4.

- ***Address: 10.10.10.10***: IPv4 address mà bản ghi A của domain name phân giải thành.












