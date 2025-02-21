# Burp Suite: The Basics

***What is Burp Suite?***

=> Burp Suite là một ***framework*** dựa trên Java, được thiết kế để phục vụ tiến hành thử nghiệm xâm nhập Web Application. 

=> Nói 1 cách đơn giản, Burp Suite nằm bắt và cho phép thao tác tất cả lưu lượng HTTP/HTTPs giữa trình duyệt và server. Khả năng ***chặn***, ***xem*** và ***sửa đổi*** các request trước khi chúng đến server mục tiêu hoặc thậm chí, thao túng các response trước khi trình duyệt nhận được khiến Burp Suite trở thành 1 tool hữu ích để kiểm tra web application thủ công.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image.png?raw=true)

***Features of Burp Community***

- ***Proxy***: chặn và sửa đổi request, cũng như response. 

- ***Repeater***: Cho phép ghi lại, sửa đổi và gửi lại cùng 1 request nhiều lần. 

- ***Intruder***: Mặc dù bị giới hạn phiên bản, Intruder vẫn cho phép gửi request đến endpoints. Nó thường được sử dụng cho các cuộc tấn công brute-force hoặc fuzzing endpoints.

- ***Decoder***: Cung cấp dịch vụ có giá trị để chuyển đổi data. Nó có thể giải mã thông tin thu được hoặc mã hóa payload trước khi gửi chúng đến mục tiêu. Mặc dù tồn tại các dịch vụ thay thế nhưng việc tận dụng Decoder trong Burp Suite mang lại hiệu quả cao.

- ***Comparer***: Cho phép so sánh 2 phần data ở cấp độ word hoặc byte. Mặc dù không dành riêng cho Burp Suite, khả năng gửi trực tiếp các phân đoạn data lớn có thể đến Comparer bằng 1 phím tắt duy nhất sẽ tăng tốc đáng kể quá trình 

- ***Sequencer***: Thường được sử dụng khi đánh giá tính ngẫu nhiên của tokens, ví dụ như giá trị cookie session hoặc data được cho là được tạo ngẫu nhiên khác. Nếu thuật toán được sử dụng để tạo ra các giá trị này thiếu tính ngẫu nhiên an toàn thì nó có thể tạo cơ hội cho các cuộc tấn công.

***Install Burp Suite***

***The Dashboard***

***Navigation***

***Introduction to the Burp Proxy***

=> Burp Proxy là tool cơ bản và quan trọng trong Burp Suite. Cho phép nắm bắt request và response giữa user và target web server.

=> ***Intercepting Requests***: Khi requests được thực hiện thông qua Burp Suite, chúng sẽ bị chặn và không thể tiếp cận target server. Các request xuất hiện trong tab Proxy, cho phép thực hiện các hành động khác như forwarding, dropping, editing, hoặc sending đến Burp modules khác:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image1.png?raw=true)

=> ***Taking Control***: Khả năng chặn request cho phép giành quyền kiểm soát hoàn toàn lưu lượng truy cập web.

=> ***Capture and Logging***: Burp Suite ghi lại requests được thực hiện thông qua proxy theo mặc định, ngay cả khi tính năng Interception bị tắt. Chức năng logging có thể hữu ích cho việc phân tích và xem xét các request trước đó sau này.

=> ***WebSocket Support***: Burp Suite cũng capture và logs thông tin liên lạc của WebSocket, cung cấp hỗ trợ bổ sung khi phân tích Web Application.

*(**WebSocket** là một giao thức truyền thông hai chiều toàn phần (full-duplex), được xây dựng trên nền tảng giao thức TCP. Nó cho phép giao tiếp thời gian thực giữa máy khách (client) và máy chủ (server) thông qua một kết nối duy nhất mà không cần phải thiết lập lại kết nối mỗi lần trao đổi dữ liệu như HTTP truyền thống)*

=> ***Logs and History***: Các request đã được ghi có thể xem trong các tab phụ HTTP history và WebSockets history, cho phép phân tích lại và gửi request đến các Burp module khác nếu cần.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image2.png?raw=true)

=> ***Response Interception***: Theo mặc định, proxy không chặn response của server, trừ khi được yêu cầu rõ ràng trên cơ sở từng request. Checkbox ***“Intercept responses based on the following rules”***, cùng với các rule đã được xác định, cho phép chặn response linh hoạt hơn

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image3.png?raw=true)

=> ***Match and Replace***: Phần “Match and Replace” trong Proxy settings cho phép sử dụng biểu thức thông thường (regex) để sửa đối request đến và đi. Tính năng này cho phép thực hiện các thay đổi động, ví dụ như sửa đổi tác nhân người dùng hoặc thao tác với cookie

***Site Map and Issue Definitions***

***Target*** trong Burp Suite không chỉ cung cấp khả năng kiểm soát phạm vi thử nghiệm, nó còn bao gồm 3 tab phụ: 

=> ***Site map***: Tab phụ cho phép vạch ra các ứng dụng web mà ta đang nhắm vào theo cấu trúc cây. Mỗi page ta truy cập khi proxy đang hoạt động sẽ được hiển thị trên site map. Tính năng cho phép ta tự động tạo site map bằng cách duyệt ứng dụng web. Ngay cả với Community version, ta vẫn có thể sử dụng site map để tích lũy data trong các bước liệt kê ban đầu. Nó rất hữu ích trong việc lập bản đồ APIs, vì mọi API endpoint được ứng dụng web access sẽ được ghi lại trong site map.

=> ***Issue definitions***: Với Community version, vẫn có quyền truy cập vào danh sách lỗ hổng mà trình quét tìm kiếm. Phần Issue definitions cung cấp danh sách đầy đủ các lỗ hổng web, kèm theo mô tả và tài liệu tham khảo.

=> ***Scope Settings*** : Cài đặt này cho phép kiểm soát phạm vi mục tiêu trong Burp Suite. Cho phép bao gồm hoặc loại trừ các domain / IP cụ thể để xác định phạm vi thử nghiệm. Bằng cách quản lý phạm vi, có thể tập trung vào các ứng dụng web mà ta đang nhắm mục tiêu cụ thể, tránh thu được những lưu lượng không cần thiết.

***Scoping and Targeting***

Việc capturing và logging tất cả lưu lượng truy cập có thể nhanh chóng trở nên quá tải, đặc biệt khi chỉ muốn tập trung vào các ứng dụng web cụ thể. Đây là lúc scoping phát huy tác dụng.

Bằng cách đặt scope cho dự án, ta có thể hạn chế Burp Suite chỉ nhắm mục tiêu (các) ứng dụng web cụ thể mà ta muốn thử nghiệm. Để thực hiện, chuyển sang tab Target, chuột phải vào 1 mục tiêu từ danh sách bên trái và chọn Add To Scope. Sau đó chọn Yes khi Burp hỏi có muốn dừng logging bất cứ thứ gì không nằm trong scope hay không.

***Proxying HTTPs***

***Example Attack***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image4.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image5.png?raw=true)

=> Chỉnh sửa request trực tiếp: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image6.png?raw=true)

Sau khi chèn đoạn script vào tham số ***email***, bôi đen và ***ctrl + U*** để tự động format phù hợp.

