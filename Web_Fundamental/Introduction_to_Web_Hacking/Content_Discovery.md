# Content Discovery

***Robots.txt***

Tệp ***robots.txt*** là tài liệu quan trọng được dùng để hướng dẫn các công cụ tìm kiếm (search engines) về những trang (pages) hoặc khu vực nào trên website được phép hoặc không được phép thu thập dữ liệu và hiển thị trong kết quả tìm kiếm. Việc sử dụng tệp ***robots.txt*** là một thực hành phổ biến, đặc biệt khi chủ sở hữu website muốn hạn chế việc hiển thị một số khu vực nhất định trên công cụ tìm kiếm. Bằng cách này, chủ sở hữu có thể đảm bảo rằng những thông tin quan trọng hoặc riêng tư không bị lộ ra ngoài. 

Đối với một penetration tester (người kiểm tra thâm nhập), tệp ***robots.txt*** có thể cung cấp một danh sách các vị trí trên website mà chủ sở hữu không muốn công khai hoặc không muốn người khác khám phá. Đây có thể là những khu vực chứa dữ liệu nhạy cảm, các trang quản trị, hoặc các tệp cấu hình quan trọng. Việc phân tích tệp ***robots.txt*** có thể giúp penetration tester xác định các điểm tiềm ẩn để kiểm tra sâu hơn, từ đó phát hiện các lỗ hổng bảo mật hoặc thông tin hữu ích khác.

Ví dụ về tệp ***robots.txt*** trên website Acme IT Support: 

![img](22)

=> Thư mục không được phép xem bởi crawlers (như Google) là: ***/staff-portal***

***Favicon***

Favicon là 1 icon nhỏ hiển thị trên thanh địa chỉ hoặc tab của browser, được sử dụng để xây dựng thương hiệu cho 1 website:

![img](23)

Đôi khi, khi frameworks được dùng để build 1 website, Favicon nằm trong quá trình cài đặt vẫn còn sót lại và nếu web developer không thay thế framework này bằng 1 framework tùy chỉnh, điều này có thể để lại manh mối về framework nào đang được sử dụng. OWASP lưu trữ 1 db gồm các Favicon phổ biến có thể dùng để kiểm tra Favicon mục tiêu https://wiki.owasp.org/index.php/OWASP_favicon_database. Sau khi biết về framework stack, có thể sử dụng các tài nguyên bên ngoài để khám phá thêm. 

Ví dụ, truy cập vào 1 website có sử dụng ***favicon***:

![img](24)

Trên Kali Linux, sử dụng câu lệnh: ***curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum***
Câu lệnh này sẽ download Favicon và lấy giá trị ***md5 hash*** của nó: 

![img](25)

=> Tra cứu mã hash này trên OWASP_favicon_database để tìm framework mà Favicon này thuộc về: 

![img](26)

=> Framework mà Favicon này thuộc về là ***cgiirc (0.5.9)***. 

***sitemap.xml***

Khác với tệp ***robots.txt***, vốn giới hạn phạm vi thu thập thông tin của các công cụ tìm kiếm, tệp ***sitemap.xml*** cung cấp một danh sách đầy đủ các tệp và trang mà chủ sở hữu website mong muốn được hiển thị trên công cụ tìm kiếm. Đây là một công cụ hữu ích giúp các công cụ tìm kiếm dễ dàng khám phá và lập chỉ mục (index) nội dung của website.

Trong một số trường hợp, tệp ***sitemap.xml*** có thể liệt kê cả những khu vực của website khó điều hướng hoặc ít được liên kết trực tiếp từ trang chủ. Ngoài ra, nó cũng có thể bao gồm các trang web cũ mà trang hiện tại không còn sử dụng hoặc không còn hiển thị công khai, nhưng vẫn hoạt động ngầm và có thể truy cập được. Điều này giúp đảm bảo rằng các công cụ tìm kiếm có thể thu thập dữ liệu một cách toàn diện, ngay cả với những trang ẩn hoặc ít được chú ý.

Thử xem file ***sitemap.xml*** trên web Acme IT Support:

![img](27)

***HTTP Headers***

Khi gửi request đến web server, server sẽ trả về nhiều ***HTTP headers*** khác nhau. Những Headers này đôi khi có thể chứa thông tin hữu ích như phần mềm web server và có thể cả ngôn ngữ lập trình / tệp lệnh đang được sử dụng. Trong ví dụ dưới đây, có thể thấy web server là ***NGINX version 1.18.0*** và chạy ***PHP version 7.4.3***. Sử dụng thông tin này, có thể tìm lỗ hổng của phiên bản software được web server sử dụng. 

*Giải thích về lệnh curl*:

Lệnh ***curl < Client Url >*** - công cụ để truyền tải data giữa máy tính và server qua protocol khác nhau, phổ biến nhất là HTTP, HTTPs, FTP,...

Tác dụng phổ biến: 

- Tải xuống tệp tin từ Internet: 

    *curl -O http://example.com/file.txt*

- Gửi yêu cầu HTTP (GET, POST, PUT, DELETE,...)

    *curl http://example.com* (Mặc định là GET request)

    *curl -X POST -d "key1=value1&key2=value2" http://example.com/api* (Thêm tùy chọn -X POST chỉ định POST request)

    *curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' http://example.com/api* (JSON request)

- Xác thực với API

    *curl -u username:password http://example.com/api* (sử dụng basic authen)

    *curl -H "Authorization: Bearer TOKEN" http://example.com/api* (sử dụng Bearer Token)

- Kiểm tra và gỡ lỗi kết nối mạng

    *curl -I http://example.com* (kiểm tra thông tin HTTP header)

    *curl -v http://example.com* (hiển thị thông tin chi tiết của request/response)

- Tải tệp tin với quyền tự động tiếp tục:

    *curl -C --O http://example.com/largefile.zip*

- Gửi tệp tin lên server:
   
    *curl -F "file=@localfile.txt" http://example.com/upload*

Và còn nhiều ứng dụng khác...

Thử chạy lệnh ***curl*** với web server này, trong đó tùy chọn ***-v*** được bật để xem chi tiết, tùy chọn này sẽ xuất ra headers!

![img](28)

***Framework Stack***

Sau khi thiết lập 1 framework cho website, từ ví dụ về Favicon hoặc bằng cách tìm kiếm manh mối trong Soure Page như comments, copyright notices hoặc credits (ghi nhận tác giả), từ đó có thể xác định framework của website. Từ đây, có thể tìm hiểu thêm về software và thông tin khác, từ đó dẫn đến nhiều nội dung hơn có thể khám phá. 

Xem Source Page của website Acme IT Support, sẽ thấy comments ở cuối page, kèm theo thời gian tải trang và link đến website của framework:

![img](29)

=> Truy cập website Framework: 

![img](30)

=> Thử truy cập endpoint ***/thm-framework-login*** và log in với tài khoản admin được cung cấp: 

![img](31)

Ngoài ra còn có các tài nguyên bên ngoài giúp khám phá thông tin về website; những tài nguyên này thường được gọi là ***OSINT*** hoặc (Open-Source Intelligence) vì chúng là các tools có sẵn miễn phí để thu thập thông tin.

***Google Hacking / Dorking***

Google Hacking / Dorking sử dụng các tính năng công cụ tìm kiếm nâng cao của Google, cho phép chọn content tùy chỉnh. Ví dụ, chọn ra kết quả từ 1 domain name nhất định bằng cách sử dụng ***site: filter***, ví dụ ***site:tryhackme.com***, sau đó có thể kết hợp kết quả này với các cụm từ tìm kiếm nhất định như ***“admin”***: ***site:tryhackme.com admin***, điều này sẽ chỉ trả về kết quả từ website tryhackme có chứa từ “admin” trong nội dung. Đồng thời cũng có thể kết hợp nhiều filter. Dưới đây là filters: 

![img](32)

***OSINT - Wappalyzer***

***Wappalyzer*** (https://www.wappalyzer.com/) là 1 công cụ online và extension của trình duyệt, giúp xác định những công nghệ mà 1 website sử dụng, như framework, Content Management Systems (CMS), payment processors,... và thậm chí có thể tìm version.

***OSINT - Wayback Machine***

***Wayback Machine*** (https://archive.org/web/) là kho lưu trữ lịch sử của websites có từ cuối những năm 90. Có thể tìm domain name và nó sẽ hiển thị mỗi khi dịch vụ quét web page và lưu contents còn hoạt động. Dịch vụ này có thể giúp khám phá old pages có thể vẫn còn hoạt động trên website hiện tại.

***OSINT - Github***

Để hiểu ***Github***, cần hiểu ***Git***. Git là 1 version control system (hệ thống kiểm soát phiên bản) theo dõi các thay đổi đối với files trong dự án. Làm việc trong team dễ dàng hơn vì có thể xem từng thành viên đang chỉnh sửa những gì và đã thực hiện những thay đổi nào đối với file. Khi user đã thực hiện xong thay đổi, họ commit chúng với 1 message và push chúng trở lại repository (trung tâm lưu trữ) để những user khác pull những thay đổi này về máy cục bộ của họ. Github là phiên bản lưu trữ của Git trên Internet. Các repository có thể được đặt ở chế độ công khai hoặc riêng tư và có nhiều biện pháp kiểm soát truy cập khác nhau. Có thể sử dụng tính năng tìm kiếm của GitHub để tìm tên công ty hoặc trang web nhằm thử và định vị các kho lưu trữ của website. Sau khi được phát hiện, có thể có quyền truy cập vào source code, password hoặc nội dung khác. 

***OSINT - S3 Buckets***

***S3 Buckets*** là dịch vụ lưu trữ do **Amazon AWS** cung cấp, cho phép lưu files, thậm chí cả static website trên cloud có thể truy cập qua HTTP và HTTPs. Chủ sở hữu files có thể đặt quyền truy cập để đặt files ở chế độ công khai, riêng tư, thậm chí có thể chỉnh sửa. Đôi khi, các quyền truy cập có thể được đặt sai và vô tình cho phép truy cập vào các file không được cung cấp công khai. Format của S3 Buckets là ***http(s)://{name}.s3.amazonaws.com***, trong đó ***{name}*** do chủ sở hữu quyết định, vd như ***tryhackme-assets.s3.amazonaws.com***. Có thể phát hiện S3 Buckets theo nhiều cách, ví dụ như tìm URL trong Source Page của website, Github repositories, hoặc thậm chí tự động hóa quy trình. 1 phương pháp tự động hóa phổ biến là sử dụng tên công ty, theo sau là các thuật ngữ phổ biến như ***{name}-assets***, ***{name}-www***, ***{name}-public***, ***{name}-private***,...

***Automated Discovery***

***Automated discovery*** là quá trình sử dụng các tool để khám phá nội dung thay vì thực hiện thủ công. Quá trình thường được tự động hóa vì nó thường chứa hàng trăm, hàng nghìn, thậm chí hàng triệu requests tới web server. Requests này kiểm tra xem file hoặc directory có tồn tại trên website hay không, cho phép truy cập vào resources không biết là có tồn tại. Quá trình này có thể thực hiện bằng cách sử dụng 1 nguồn tài nguyên gọi là ***wordlists***.

***Wordlists***

Là các files text chứa 1 list dài các từ thường được sử dụng; chúng có thể bao gồm nhiều trường hợp sử dụng khác nhau. Ví dụ: một ***password wordlist*** sẽ bao gồm các password được sử dụng thường xuyên nhất, trong trường hợp này, sẽ yêu cầu list chứa tên file và directory được sử dụng phổ biến nhất. Tài nguyên wordlists được cài đặt sẵn trên THM Attackbox là https://github.com/danielmiessler/SecLists 

***Automation Tools***

Mặc dù có sẵn nhiều tools khám phá nội dung khác nhau, tất cả đều có tính năng và sai sót, 3 công cụ được cài đặt sẵn: ***ffluf***, ***dirb*** và ***gobuster***. Thử thực hiện các lệnh:

***ffluf***:

![img](33)

=> Kết quả: 

![img](34)

=> Tìm được các file hoặc directory ẩn trên web server tại địa chỉ http://10.10.75.206

Tùy chọn ***-w /usr/share/wordlist/SecLists/Discovery/Web-Content/common.txt*** để chỉ định wordlist được sử dụng là ***common.txt*** (chứa list path hoặc tên file phổ biến).

Tùy chọn ***-u http://10.10.75.206/FUZZ*** chỉ định URL mục tiêu. ***FUZZ*** là 1 placeholder mà ffuf sẽ thay thế cho từng từ trong wordlist (common.txt) để thử nghiệm các path hoặc tên file.

***Sử dụng dirb***:

![img](35)

=> Kết quả: 

![img](36)

=> Công cụ ***dirb*** sẽ:

- Lấy từng từ trong ***wordlist*** (common.txt)

- Gắn từ đó vào URL mục tiêu để tạo request, ví dụ: 
    http://10.10.75.206/contact
    
    http://10.10.75.206/customers

    ...

- Kiểm tra phản hồi từ web server, nếu trả về mã HTTP như ***200 OK*** hoặc ***403 Forbidden*** nghĩa là đường dẫn tồn tại, ***404 Not Found*** nghĩa là đường dẫn không tồn tại.

***Sử dụng Gobuster:***

![img](37)

=> Kết quả: 

![img](38)

- Tùy chọn ***-dir***: chế độ dò tìm directory

- Tùy chọn ***--url http://10.10.75.206***: địa chỉ URL mục tiêu

- Tùy chọn ***-w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt*** để chỉ định wordlist








