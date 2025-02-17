# How Websites Work

1. View the website on this link. What is the password hidden in the source code? 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image16.png?raw=true)

Truy cập website: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image17.png?raw=true)

Một trong những **việc đầu tiên** cần làm khi đánh giá một web application về các vấn đề bảo mật là **xem mã nguồn**. 

=> View page source: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image18.png?raw=true)

=> Tìm được username và password.

2. View the website on this task and inject HTML so that a malicious link to http://hacker.com is shown.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image19.png?raw=true)

Truy cập website: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image20.png?raw=true)

=> View page source: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image21.png?raw=true)

=> hàm JS **sayHi** sẽ hiển thị, kích hoạt bất cứ input data nào được nhập khi click button Say Hi. 

=> Inject HTML code vào input field link đến website **http://hacker.com** và click Say Hi Button: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image22.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/How_The_Web_Works/images/image23.png?raw=true)



