# Putting it all together

***Bộ cân bằng tải***: tìm ra server phù hợp nhất để chuyển tiếp request.

***CDN*** (Content Deliver Networks): lưu trữ file tĩnh, tăng tốc độ truy cập từ client đến server.

***Databases***: nơi website sử dụng để lưu trữ thông tin.

***WAF*** (Web Application Firewall): nằm giữa client và server, bảo vệ web server khỏi bị hack và tấn công DoS.

**Quiz**: How a request to a website works to reveal the flag?

1. Request **tryhackme.com** in your browser.

2. Check Local Cache for IP Address.

3. Check your recursive DNS Server for Address.

4. Query root server to find authoritative DNS Server.

5. Authoritative DNS server advises the IP address for the website. 

6. Request passes through a Web Application Firewall.

7. Request passes through a Load Balancer.

8. Connect to Web server on port 80 or 443.

9. Web server receives the GET request.

10. Web application talks to Database.

11. Your Browser renders the HTML into a viewtable website. 