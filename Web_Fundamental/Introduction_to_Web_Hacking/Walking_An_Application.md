# Walking An Application

**Các công cụ có sẵn trên trình duyệt**

Trình duyệt hiện đại cung cấp nhiều công cụ tích hợp giúp kiểm tra và phân tích web application một cách chi tiết. Dưới đây là một số công cụ quan trọng và cách sử dụng chúng để phát hiện các vấn đề bảo mật:

1. ***View source***: Công cụ này cho phép bạn xem mã nguồn HTML của một trang web. Đây là bước đầu tiên để hiểu cấu trúc của trang và tìm kiếm các thông tin nhạy cảm có thể bị lộ.

**Lưu ý**: Đôi khi mã nguồn có thể được minify hoặc obfuscate, làm cho việc đọc và phân tích trở nên khó khăn hơn.

2. ***Inspector***: Công cụ Inspector cho phép kiểm tra và chỉnh sửa các phần tử HTML và CSS trên trang web. Điều này giúp hiểu rõ hơn về cách trang web được xây dựng và có thể phát hiện các vấn đề như hidden fields, các phần tử bị ẩn, hoặc các thông tin nhạy cảm được giấu trong DOM.

3. ***Debugger***: Debugger là công cụ mạnh mẽ để kiểm tra và kiểm soát luồng thực thi của JavaScript trên trang web. Nó cho phép đặt breakpoints, theo dõi biến, và kiểm tra cách thức hoạt động của các hàm JavaScript.

4. ***Network***: Công cụ Network cho phép theo dõi tất cả các yêu cầu mạng mà trang web thực hiện, bao gồm các yêu cầu HTTP/HTTPS, tải tài nguyên, và các yêu cầu API. Điều này giúp phát hiện các vấn đề như thông tin nhạy cảm được truyền qua mạng mà không được mã hóa, hoặc các yêu cầu không cần thiết có thể tiết lộ thông tin nội bộ.

**View some page source!**

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image.png?raw=true)

Thử xem source page của trang chủ website Acme IT Support và chọn ra những thông tin quan trọng.

=> Source page của trang chủ website Acme IT Support:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image1.png?raw=true)

Ở đầu page, thấy một số mã bắt đầu bằng **< !--** và kết thúc bằng **-->** , đây là comments. Comments là những thông điệp do website developer để lại, thường dùng để giải thích gì đó trong code cho programmers khác, hoặc thậm chí là ghi chú nhắc nhở cho chính mình. Những comments này không được hiển thị trên webpage thực tế. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image2.png?raw=true)

=> Comment này có đề cập đến một endpoint: ***"/new-home-beta"***. 

Truy cập ***/new-home-beta***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image3.png?raw=true)

Liên kết đến các trang khác nhau trong HTML được viết bằng thẻ liên kết ( đây là các phần tử HTML bắt đầu bằng ***<a*** ) và liên kết được chuyển hướng đến được lưu trữ trong thuộc tính ***href***.

Nếu xem kỹ hơn Source Page, có 1 liên kết ẩn đến 1 trang bắt đầu bằng ***“secr”***. Trong thực tế, có thể phát hiện ra một số khu vực riêng tư được doanh nghiệp sử dụng để lưu trữ thông tin công ty / nhân viên / khách hàng:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image4.png?raw=true)

=> Ngoài ***/secret-page*** còn tìm ra endpont ***/assets***.

Các tệp như CSS, JavaScript và Hình ảnh có thể sử dụng trong HTML code, có thể thấy tất cả các tệp này đều được lưu trữ trong 1 directory. Nếu xem directory này trong browser thì có lỗi cấu hình. Những gì được hiển thị là 1 page trống hoặc 1 trang có lỗi 403 - Forbidden, cho biết không có quyền truy cập vào directory. Thay vào đó, tính năng liệt kê directory được kích hoạt, tính năng trong thực tế sẽ liệt kê mọi file trong directory. Đôi khi đây không không phải vấn đề, tất cả file trong directory đều an toàn để xem, nhưng trong 1 số trường hợp, backup files, source code hoặc thông tin bí mật khác có thể được lưu trữ ở đây. 

Truy cập ***/assets***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image5.png?raw=true)

=> Tìm thấy file ***flag.txt***. Truy cập file: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image6.png?raw=true)

Nhiều website ngày nay không tạo từ đầu và sử dụng cái được gọi là **framework**. 1 framework là 1 tập hợp mã tạo sẵn, dễ dàng cho phép developer đưa vào các tính năng phổ biến mà website yêu cầu, như blog, quản lý user, xử lý form,... giúp developer tiết kiệm thời gian.

Viewing page source thường có thể cung cấp manh mối về 1 framework có đang được dùng hay không, nếu có thì framework nào và phiên bản nào. Việc biết framework và phiên bản có thể hữu ích vì có thể có lỗ hổng công khai trong framework và website có thể không sử dụng phiên bản framework mới nhất. Ở cuối page, tìm thấy comment về framework và version được dùng, cũng như liên kết tới website của framework:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image7.png?raw=true)

=> Download file ***tmp.zip***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image8.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image9.png?raw=true)

=> Giải nén file và tìm được: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image10.png?raw=true)

Ngoài việc xem Source Page trực tiếp, có thể sử dụng Inspector để chỉnh sửa và tương tác với các thành phần trên page, điều này rất hữu ích với web developer trong việc debug. Trên website Acme IT Support, trong phần news có 3 bài viết. 2 bài viết đầu tiên có thể đọc được, nhưng bài thứ 3 bị chặn với 1 thông báo cho biết phải là khách hàng cao cấp mới có thể xem bài viết. Floating boxes chặn nội dung page thường được gọi là paywalls vì chúng có nghĩa là sẽ chặn nội dung hiển thị cho đến khi trả phí:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image11.png?raw=true)

Click chuột phải vào paywall và sẽ thấy tùy chọn Inspect từ menu, tùy chọn này sẽ mở developer tools ở phía dưới hoặc bên phải website, tùy thuộc vào browser hoặc tùy chọn. Bây giờ sẽ thấy các thành phần /HTML tạo nên website:

Click vào thẻ ***< div>*** có class là ***premium-customer-blocker***. Sẽ thấy CSS style được áp dụng cho thẻ, ví dụ: margin-top: 60px và text-align: center. Style cần quan tâm ở đây là ***display: block***. Sử dụng Inspector và có thể thay đổi bất kỳ thông tin nào trên website, bao gồm cả nội dung. Nhưng điều này chỉ được chỉnh sửa trên browser từ phía client và khi refresh, mọi thứ sẽ trở lại bình thường. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image13.png?raw=true)

=> Thử xóa ***display: block;*** trong Inspector: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image14.png?raw=true)

=> Sau khi xóa, tìm thấy nội dung bị ẩn phía sau paywall: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image15.png?raw=true)

Trên website Acme IT Support, click vào contact page, mỗi khi page được tải, có thể thấy screen nhấp nháy nhanh màu đỏ. Sử dụng Debugger để tìm hiểu xem flash màu đỏ này là gì và liệu có gì đằng sau nó không. 

Trong cả 2 kiểu browser, ở phía bên trái, sẽ thấy danh sách tất cả resources mà webpage hiện tại đang sử dụng. Nếu click vào assets folder, sẽ thấy 1 file flash.min.js. 

Nhiều khi xem các file js, sẽ nhận thấy mọi thứ đều nằm trên 1 dòng, đó là vì nó đã được thu nhỏ, nghĩa là tất cả định dạng (tabs, spacing và newline) đã bị xóa để làm cho file nhỏ hơn. File này cũng không phải ngoại lệ, và nó cũng bị xáo trộn, khiến nó cố tình khó đọc, do đó các developers khác không thể dễ dàng sao chép. 

Có thể trả về 1 số định dạng bằng cách sử dụng tùy chọn “Pretty Print”, trông giống như 2 dấu {} để dễ đọc hơn 1 chút, mặc dù do bị che khuất nên vẫn khó hiểu chuyện gì đang xảy ra với file. Cuộn xuống dưới và thấy dòng ***flash['remove'];***

Đoạn mã JS nhỏ này sẽ xóa the red popup from page. Có thể sử dụng 1 tính năng khác của Debugger là ***breakpoints***. Đây là những points trong code mà có thể buộc browser ngừng xử lý JS và tạm dừng quá trình thực thi hiện tại. 

Nếu click vào số dòng chứa mã trên, sẽ thấy nó chuyển sang màu xanh lam; nghĩa là đã chèn 1 breakpoint ở trên trang. Bây giờ thử làm mới page!

File ***flash.min.js***: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image16.png?raw=true)

=> Đặt ***breakpoint***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image17.png?raw=true)

Click vào mục Contact và page sẽ dừng ở breakpoint này: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image18.png?raw=true)

Tab ***Network*** trên developer tool có thể dùng để theo dõi mọi request bên ngoài mà webpage đưa ra. Nếu click vào tab Network rồi refresh page, sẽ thấy tất cả file mà page yêu cầu:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image19.png?raw=true)

Khi tab Network đang mở, ở mục Contact, hãy điền thử vào contact form và nhấn nút Send Message. Sẽ nhận thấy 1 event trên tab Network, đây là form đang được gửi ở chế độ nền bằng method có tên là ***AJAX***. AJAX là 1 method gửi và nhận dữ liệu mạng trong nền web application mà không can thiệp bằng cách thay đổi web page hiện tại. 

Kiểm tra page mà form được gửi đến…
Sau khi điền thông tin vào Form và gửi, truy cập thử Request URL:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image20.png?raw=true)

=> Truy cập ***/contact-msg***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image21.png?raw=true)












