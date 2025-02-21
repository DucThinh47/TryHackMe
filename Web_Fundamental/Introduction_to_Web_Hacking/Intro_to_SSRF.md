# Intro to SSRF

***Type of SSRF***

Có 2 loại lỗ hổng SSRF; đầu tiên là ***Regular SSRF*** , nơi data được trả về màn hình của attacker. Loại thứ hai là ***Blind SSRF***, là loại mà SSRF xảy ra nhưng không có thông tin nào được trả về màn hình attacker. 

***SSRF Examples***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image86.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image87.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image88.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image89.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image90.png?raw=true)

Ví dụ về 1 tấn công SSRF: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image91.png?raw=true)

***Finding an SSRF***

4 ví dụ phổ biến về việc phát hiện lỗ hổng SSRF: 

1. Khi 1 URL đầy đủ được dùng làm giá trị cho 1 tham số trên thanh URL:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image92.png?raw=true)

2. 1 field ẩn trong form: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image93.png?raw=true)

3. Một phần URL dưới dạng tham số trong URL, ví dụ như chỉ hostname:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image94.png?raw=true)

4. Có thể chỉ là path của URL dưới dạng 1 tham số:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image95.png?raw=true)

***Defeating Common SSRF Defenses***

***1. Deny List***
Deny List là một cơ chế bảo mật cho phép tất cả requests được chấp nhận, trừ những request liên quan đến các tài nguyên (resources) được liệt kê trong danh sách hoặc khớp với một mẫu cụ thể.

=> Mục đích: Bảo vệ các điểm truy cập (endpoints) nhạy cảm, chẳng hạn như ***localhost*** hoặc ***127.0.0.1***, vì chúng có thể chứa dữ liệu nhạy cảm hoặc thông tin hiệu suất máy chủ.

=> Cách bypass: Attacker có thể sử dụng các cách viết khác của ***localhost*** hoặc ***127.0.0.1*** để vượt qua Deny List, ví dụ:

Các địa chỉ thay thế: 0, 0.0.0.0, 0000, 127.1, 127.*.*.*, 2130706433, 017700000001.

Hoặc sử dụng subdomain trỏ về ***127.0.0.1***: Ví dụ, ***127.0.0.1.nip.io***.

Trong môi trường đám mây:

Địa chỉ IP ***169.254.169.254*** thường chứa siêu dữ liệu (metadata) của máy chủ đám mây, bao gồm cả thông tin nhạy cảm.

=> Kẻ tấn công có thể đăng ký một subdomain trỏ đến địa chỉ IP này để bypass Deny List.

***2. Allow List***
Allow List là cơ chế ngược lại với Deny List: tất cả các request đều bị từ chối, trừ những request xuất hiện trong danh sách hoặc khớp với một mẫu cụ thể.

Ví dụ: Một ứng dụng web chỉ cho phép các URL bắt đầu bằng ***https://website.thm***.

=> Cách bypass: Attacker có thể tạo một subdomain trên domain của họ, ví dụ: ***https://website.thm.attackers-domain.thm***. web app có thể nhầm lẫn và cho phép request này, từ đó attacker có thể kiểm soát các yêu cầu nội bộ.

***3. Open Redirect***
Open Redirect là một endpoint trên máy chủ cho phép chuyển hướng người dùng đến một địa chỉ website khác.

Ví dụ: Một liên kết như https://website.thm/link?url=https://tryhackme.com sẽ chuyển hướng người dùng đến https://tryhackme.com.

=> Mục đích ban đầu: Ghi lại số lần nhấp vào liên kết cho mục đích quảng cáo hoặc tiếp thị.

=> Lỗ hổng: Nếu có lỗ hổng SSRF (Server-Side Request Forgery) và ứng dụng chỉ cho phép các URL bắt đầu bằng https://website.thm/, attacker có thể lợi dụng tính năng chuyển hướng để gửi yêu cầu nội bộ đến một domain mà họ kiểm soát.

***SSRF Practical***

Truy cập website Acme IT Support: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image96.png?raw=true)

=> Thử chọn 1 Avatar và Inspect: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image97.png?raw=true)

=> Thử thay đổi value thành ***private*** để truy cập endpoint /private:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image98.png?raw=true)

=> Sau đó chọn avatar được thay đổi value private này và click chọn Update Avatar:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image99.png?raw=true)

=> Nhận được thông báo lỗi ***URL cannot start with private***.

=> Có thể web server đã sử dụng ***Allow list***, trong đó có rule chỉ cho phép URL bắt đầu bằng một giá trị nào đó. 

=> Thay value thành ***x/../private***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image100.png?raw=true)

=> Click chọn lại avatar này và click Button Update Avatar: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image101.png?raw=true)

=> Thông báo thành công, lúc này Current Avatar đang là không có ảnh gì.

=> Thử View source page: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image102.png?raw=true)

=> Giải mã chuỗi base64 này: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Introduction_to_Web_Hacking/images/image103.png?raw=true)




