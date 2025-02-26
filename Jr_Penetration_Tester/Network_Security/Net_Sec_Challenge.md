# Net Sec Challenge

**Task 1: Introduction**

Phân tích các tùy chọn port scan trong Nmap:

1. **-sS (TCP SYN Scan)**

- Mô tả:

    - Là kiểu quét mặc định và nhanh nhất

    - Chỉ gửi gói tin có cờ SYN, chờ phản hồi từ mục tiêu: 

        - SYN-ACK: cổng mở

        - RST: cổng đóng

        - Không có phản hồi hoặc ICMP: cổng bị lọc

- Ưu điểm: 

    - Nhanh, hiệu quả, khó bị phát hiện (do không hoàn tất quá trình bắt tay 3 bước)

- Khi nào dùng?

    - Quét sơ bộ hệ thống để xác định cổng mở, đóng, hoặc bị lọc mà không muốn thu hút sự chú ý.

    - Yêu cầu quyền root (dùng với sudo).

2. **-sT (TCP Connect Scan)**

- Mô tả:

    - Sử dụng lệnh gọi hệ thống connect() để hoàn tất quá trình bắt tay TCP (TCP 3-way handshake)

    - Kết quả tương tự như -sS nhưng dễ bị phát hiện hơn.

- Ưu điểm: 

    - Không cần quyền root

- Nhược điểm: 
    
    - Chậm hơn và dễ bị hệ thống ghi lại (do hoàn tất kết nối TCP).

- Khi nào dùng? 

    - Khi không có quyền root trên hệ thống quét.

    - Khi cần một giải pháp đơn giản và không yêu cầu tốc độ.

3. **-sA (TCP ACK Scan)**

- Mô tả: 

    - Gửi gói ACK mà không quan tâm trạng thái cổng.

    - Dùng để xác định cổng bị lọc (Filtered) hoặc không bị lọc (Unfiltered): 

        - RST: không bị lọc

        - Không có phản hồi hoặc ICMP: Bị lọc

- Ưu điểm: 

    - Dùng để kiểm tra tường lửa và phát hiện quy tắc lọc cổng (firewall rules).

- Khi nào dùng? 

    - Kiểm tra cổng bị lọc hay không trong các môi trường có tường lửa hoặc IPS/IDS.

4. **-sW (Window scan)**

- Mô tả:

    - Biến thể của -sA, sử dụng kích thước cửa sổ TCP để suy luận trạng thái cổng.

    - Nếu kích thước cửa sổ TCP khác 0, cổng có thể mở.

- Khi nào dùng? 

    - Khi hệ thống không hỗ trợ hoặc phản hồi không đầy đủ với -sA.

    - Dùng trong các trường hợp cần kiểm tra cổng bị lọc hoặc không bị lọc.

5. **-sM (TCP Maimon scan)**

- Mô tả: 

    - Gửi gói FIN/ACK để kiểm tra trạng thái cổng.

    - Dựa trên cách hệ điều hành xử lý gói FIN/ACK:

        - Không có phản hồi: Cổng mở hoặc lọc.

        - RST: Cổng đóng.

- Khi nào dùng? 

    - Dùng để bypass các tường lửa hoặc thiết bị lọc mạng.

6. **-sU (UDP scan)**

- Mô tả: 

    - Quét các cổng UDP bằng cách gửi gói UDP đến mục tiêu:

        - Phản hồi ICMP Port Unreachable: Cổng đóng.

        - Phản hồi từ ứng dụng: Cổng mở.

        - Không có phản hồi: Cổng có thể mở hoặc bị lọc.

- Ưu điểm: 

    - Phát hiện dịch vụ chạy trên cổng UDP (DNS, SNMP, TFTP...).

- Nhược điểm: 

    - Chậm hơn nhiều so với quét TCP.

    - Dễ bị giới hạn bởi tường lửa hoặc thiết bị lọc.

- Khi nào dùng? 

    - Khi cần kiểm tra các dịch vụ UDP trên mục tiêu.

7. **-sN (Null scan), -sF (FIN scan), -sX (Xmas scan)**

**Null scan (-sN):**

- Mô tả: Gửi gói không cờ TCP (no flags).

- Khi nào dùng?  Khi muốn vượt qua tường lửa hoặc kiểm tra trạng thái cổng trong hệ thống không chuẩn.

**FIN scan (-sF)**:

- Mô tả: Gửi gói FIN để kiểm tra trạng thái cổng.

- Khi nào dùng? Khi muốn kiểm tra cổng bị lọc hoặc không bị lọc.

**Xmas scan (-sX)**:

- Mô tả: Gửi gói có tất cả các cờ TCP được bật (FIN, PSH, URG).

- Khi nào dùng? 

    - Phát hiện cổng bị lọc hoặc không bị lọc.

    - Sử dụng trên các hệ điều hành không chuẩn hoặc tường lửa cũ.

**Task 2: Challenge Questions**

Question 1: What is the highest port number being open less than 10,000?

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image82.png)

=> cổng 8080

Question 2: There is an open port outside the common 1000 ports; it is above 10000. What is it? 

Cú pháp lệnh: 

***sudo nmap -sS -p10000-15000 --min-rate 1000 --max-retries 2 -T4 --open 10.10.236.245***

- -p10000-15000 để giới hạn port nằm trong khoảng 10000 đến 15000

- --min-rate để yêu cầu NMAP gửi packet ở tốc độ chỉ định, ở đây --min-rate 1000 buộc Nmap gửi ít nhất 1000 packet mỗi giây, tăng tốc độ scan

- --max-retries để giảm số lần retry của Nmap. Mặc định Nmap sẽ thử scan lại nếu không nhận được response từ port. Ở đây, --max-retries 2 để giảm số lần thử lại còn 2

- Thêm tùy chọn -T4 để điều chỉnh tốc độ quét, trong CTF, T4 là phù hợp nhất, không gây lỗi hoặc bỏ sót dữ liệu

- Thêm tùy chọn --open để chỉ hiển thị open port

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image83.png)

Question 3: How many TCP ports are open? 

=> Cú pháp lệnh: 

***sudo nmap -sS -p- --open 10.10.236.245***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image84.png)

=> 6 cổng

Question 4: What is the flag hidden in the HTTP server header?

=> Kết nối đến HTTP server trên cổng 80

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image85.png)

Question 5: WHat is the flag hidden in the SSH server header?

=> Kết nối đến ssh server qua cổng 22:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image86.png)

Question 6: We have an FTP server listening on a nonstandard port. What is the version of the FTP server?

=> Thử kết nối đến cổng 21, mặc định của FTP server: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image87.png)

=> Bị chặn kết nối trên cổng 21. 

Thử dùng nmap: 

***sudo nmap -sV -p- -T4 --min-rate 1000 -max-retries 2 --open 10.10.139.39***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image88.png)

=> FTP server chạy trên port 10021 và có version là vsftpd 3.0.5

Question 7: We learned two usernames using social engineering: eddie and quinn. What is the flag hidden in one of these two account files and accessible via FTP?

=> Dùng lệnh ***hydra -l eddie -P /usr/share/wordlists/rockyou.txt ftp://10.10.139.39:10021/***
để tìm password cho user eddie:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image89.png)

=> password của eddit là jordan

Tiếp tục tìm của quinn: 

***hydra -l quinn -P /usr/share/wordlists/rockyou.txt ftp://10.10.139.39:10021/***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image90.png)

=> password của quinn là andrea

=> Kết nối ftp: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image91.png)

Question 8: Browsing to http://10.10.42.251:8080 displays a small challenge that will give you a flag once you solve it. What is the flag? 

Nhận thấy ở mô tả ghi nmap có thể bị phát hiện bởi IDS

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image92.png)

=> SỬ dụng thử Null scan để vượt qua firewall/IDS:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Network_Security/images/image93.png)


