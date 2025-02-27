# Metasploit: Meterpreter

**Task 1: Introduction**

- Meterpreter là một payload Metasploit hỗ trợ quá trình kiểm thử xâm nhập với nhiều thành phần có giá trị. 

- Meterpreter sẽ chạy trên hệ thống đích và hoạt động như một tác nhân trong kiến ​​trúc chỉ huy và kiểm soát

- Có thể tương tác với hệ điều hành và các tập tin đích, đồng thời sử dụng các lệnh chuyên biệt của Meterpreter.

**How does Meterpreter work?**

- Meterpreter chạy trên hệ thống đích nhưng không được cài đặt trên đó. Nó chạy trong bộ nhớ và không tự ghi vào đĩa trên đích, nhằm mục đích tránh bị phát hiện trong quá trình quét chống vi-rút vì theo mặc định, hầu hết các phần mềm chống vi-rút sẽ quét các tệp mới trên đĩa.

- Meterpreter chạy trong bộ nhớ (RAM - Bộ nhớ truy cập ngẫu nhiên) để tránh việc có tệp phải được ghi vào đĩa trên hệ thống đích (ví dụ: Meterpreter.exe). Bằng cách này, Meterpreter sẽ được coi là một tiến trình và không có tệp trên hệ thống đích.

- Meterpreter cũng nhằm mục đích tránh bị phát hiện bởi các giải pháp IPS (Hệ thống ngăn chặn xâm nhập) và IDS (Hệ thống phát hiện xâm nhập) dựa trên mạng bằng cách sử dụng giao tiếp được mã hóa với máy chủ nơi Metasploit chạy (thường là máy tấn công).

=> Ví dụ, một máy Windows mục tiêu bị khai thác bằng lỗ hổng MS17-010.

- Meterpreter đang chạy với ID tiến trình (PID) là 1304

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image70.png?raw=true)

- Nếu liệt kê các tiến trình đang chạy trên hệ thống đích bằng lệnh ***ps***, sẽ thấy PID 1304 là ***spoolsv.exe*** chứ không phải ***Meterpreter.exe***. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image71.png?raw=true)

- tiến thêm một bước và xem xét các DLL (Dynamic-Link Libraries 
- Thư viện liên kết động) được sử dụng bởi quy trình Meterpreter (PID 1304 trong trường hợp này), vẫn không tìm thấy bất cứ điều gì (ví dụ: không có Meterpreter.dll)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image72.png?raw=true)

=> Meterpreter sẽ thiết lập kênh liên lạc được mã hóa (TLS) với hệ thống của kẻ tấn công.

=> Tuy nhiên, hầu hết các phần mềm chống vi-rút sẽ phát hiện ra Meterpreter.

**Task 2: Meterpreter Flavors**

- Payload trong Metasploit ban đầu có thể chia thành 2 loại: ***inline*** (còn gọi là ***single***) và ***staged***. 

- Staged payloads được gửi đến mục tiêu qua hai bước:

1. Một phần nhỏ (stager) được gửi trước để thiết lập kết nối.

2. Phần còn lại của payload được tải xuống sau đó.
Cách tiếp cận này giúp giảm kích thước ban đầu của payload.

- Trong khi đó, inline payloads (single payloads) được gửi toàn bộ trong một bước duy nhất.

- Meterpreter, một loại payload phổ biến, cũng có cả hai phiên bản: staged và inline. Ngoài ra, Meterpreter còn có nhiều phiên bản khác nhau tùy thuộc vào hệ thống mục tiêu.

- xem danh sách các phiên bản Meterpreter có sẵn, có thể sử dụng lệnh ***msfvenom --list payloads | grep meterpreter***. Lệnh này sẽ liệt kê tất cả các payload liên quan đến Meterpreter.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image73.png?raw=true)

=> Danh sách sẽ hiển thị các phiên bản Meterpreter có sẵn cho các nền tảng sau;

- Android
- Apple iOS
- Java
- Linux
- OSX
- PHP
- Python
- Windows

=> Quyết định sử dụng phiên bản Meterpreter nào sẽ chủ yếu dựa trên ba yếu tố:

- Hệ điều hành mục tiêu

- Các thành phần có sẵn trên hệ thống đích

- Các loại kết nối mạng có thể có với hệ thống đích (có cho phép kết nối TCP thô không? Bạn chỉ có thể có kết nối ngược HTTPS không? Địa chỉ IPv6 không được giám sát chặt chẽ như địa chỉ IPv4 phải không? v.v.)

- một số khai thác sẽ có payload Meterpreter mặc định, có thể thấy trong ví dụ bên dưới với khai thác ***ms17_010_eternalblue***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image74.png?raw=true)

-  cũng có thể liệt kê payload có sẵn khác bằng lệnh ***show payloads*** với bất kỳ mô-đun nào.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image75.png?raw=true)

***Task 3: Meterpreter Commands***

- Lệnh ***help*** trên bất kỳ phiên Meterpreter nào sẽ liệt kê tất cả lệnh có sẵn.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image76.png?raw=true)

- Meterpreter sẽ cung cấp ba loại công cụ chính:

    - Built-in commands
    - Meterpreter tools
    - Meterpreter scripting

- chạy lệnh ***help***, sẽ thấy các lệnh của Meterpreter được liệt kê theo các danh mục khác nhau:

    - Core commands
    - File system commands
    - Networking commands
    - System commands
    - User interface commands
    - Webcam commands
    - Audio output commands
    - Elevate commands
    - Password database commands
    - Timestamp commands

**Meterpreter commands**

- Core commands: 

    - ***background***: Backgrounds the current session
    - ***exit***: Terminate the Meterpreter session
    - ***guid***: Get the session GUID (Globally Unique Identifier)
    - ***help***: Displays the help menu
    - ***info***: Displays information about a Post module
    - ***irb***: Opens an interactive Ruby shell on the current session
    - ***load***: Loads one or more Meterpreter extensions
    - ***migrate***: Allows you to migrate Meterpreter to another process
    - ***run***: Executes a Meterpreter script or Post module
    - ***sessions***: Quickly switch to another session

- File system commands:

    - ***cd***: Will change directory
    - ***ls***: Will list files in the current directory (dir will also work)
    - ***pwd***: Prints the current working directory
    - ***edit***: will allow you to edit a file
    - ***cat***: Will show the contents of a file to the screen
    - ***rm***: Will delete the specified file
    - ***search***: Will search for files
    - ***upload***: Will upload a file or directory
    - ***download***: Will download a file or directory

- Networking commands: 

    - ***arp***: Displays the host ARP (Address Resolution Protocol) cache
    - ***ifconfig***: Displays network interfaces available on the target system
    - ***netstat***: Displays the network connections
    - ***portfwd***: Forwards a local port to a remote service
    - ***route***: Allows you to view and modify the routing table

- System commands:

    - ***clearev***: Clears the event logs
    - ***execute***: Executes a command
    - ***getpid***: Shows the current process identifier
    - ***getuid***: Shows the user that Meterpreter is running as
    - ***kill***: Terminates a process
    - ***pkill***: Terminates processes by name
    - ***ps***: Lists running processes
    - ***reboot***: Reboots the remote computer
    - ***shell***: Drops into a system command shell
    - ***shutdown***: Shuts down the remote computer
    - ***sysinfo***: Gets information about the remote system, such as OS

- Other Commands:

    - ***idletime***: Returns the number of seconds the remote user has been idle
    - ***keyscan_dump***: Dumps the keystroke buffer
    - ***keyscan_start***: Starts capturing keystrokes
    - ***keyscan_stop***: Stops capturing keystrokes
    - ***screenshare***: Allows you to watch the remote user's desktop in real time
    - ***screenshot***: Grabs a screenshot of the interactive desktop
    - ***record_mic***: Records audio from the default microphone for X seconds
    - ***webcam_chat***: Starts a video chat
    - ***webcam_list***: Lists webcams
    - ***webcam_snap***: Takes a snapshot from the specified webcam
    - ***webcam_stream***: Plays a video stream from the specified webcam
    - ***getsystem***: Attempts to elevate your privilege to that of local system
    - ***hashdump***: Dumps the contents of the SAM database

**Task 4: Post-Exploitation with Meterpreter**

- Meterpreter cung cấp nhiều lệnh hữu ích hỗ trợ giai đoạn hậu khai thác.

**Help**

- Lệnh này sẽ cung cấp danh sách tất cả các lệnh có sẵn trong Meterpreter

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image77.png?raw=true)

**Meterpreter commands**

- Lệnh ***getuid*** sẽ hiển thị cho người dùng xem Meterpreter hiện đang chạy. Điều này sẽ cho ý tưởng về cấp độ đặc quyền có thể có trên hệ thống đích

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image78.png?raw=true)

- Lệnh ***ps*** sẽ liệt kê các tiến trình đang chạy. Cột PID cũng sẽ cung cấp cho thông tin PID mà cần để di chuyển Meterpreter sang một quy trình khác.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image79.png?raw=true)

**Migrate**

- Di chuyển sang một tiến trình khác giúp Meterpreter tương tác với tiến trình đó. Ví dụ, nếu phát hiện một trình xử lý văn bản đang chạy trên hệ thống mục tiêu (như word.exe hoặc notepad.exe), có thể di chuyển Meterpreter sang tiến trình đó và bắt đầu ghi lại các phím được nhấn bởi người dùng.

- Một số phiên bản Meterpreter cung cấp các lệnh như ***keyscan_start***, ***keyscan_stop***, và ***keyscan_dump*** để biến Meterpreter thành một keylogger.

- Để di chuyển sang một tiến trình cụ thể, sử dụng lệnh ***migrate*** kèm theo PID (Process ID) của tiến trình mục tiêu

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image80.png?raw=true)

=> có thể mất đặc quyền người dùng nếu bạn di chuyển từ người dùng có đặc quyền cao hơn (ví dụ: HỆ THỐNG) sang một quy trình được bắt đầu bởi người dùng có đặc quyền thấp hơn (ví dụ: máy chủ web)

**Hashdump**

- Lệnh ***hashdump*** sẽ liệt kê nội dung của cơ sở dữ liệu SAM. Cơ sở dữ liệu SAM (Security Account Manager) lưu trữ mật khẩu của người dùng trên hệ thống Windows. Các mật khẩu này được lưu trữ ở định dạng NTLM (New Technology LAN Manager).

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image81.png?raw=true)

**Search**

- Lệnh ***search*** rất hữu ích để định vị các tệp có thông tin hấp dẫn. Trong ngữ cảnh CTF, điều này có thể được sử dụng để nhanh chóng tìm thấy cờ hoặc tệp bằng chứng. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image82.png?raw=true)

**Shell**

- Lệnh ***shell*** sẽ khởi chạy shell dòng lệnh thông thường trên hệ thống đích. Nhấn CTRL+Z sẽ giúp quay lại Meterpreter.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image83.png?raw=true)

**Task 5: Post-Exploitation Challenge**

- Các lệnh như ***getsystem*** và ***hashdump*** cung cấp đòn bẩy và thông tin quan trọng cho việc leo thang đặc quyền và di chuyển theo chiều ngang. 

- Có thể sử dụng lệnh ***load*** để tận dụng các công cụ bổ sung như Kiwi hoặc thậm chí toàn bộ ngôn ngữ Python.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image84.png?raw=true)

- Khi bất kỳ công cụ bổ sung nào được tải bằng lệnh ***load***,sẽ thấy các tùy chọn mới trên menu trợ giúp. Ví dụ bên dưới hiển thị các lệnh được thêm vào mô-đun Kiwi:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image85.png?raw=true)

- Điều này sẽ thay đổi theo menu được tải, việc sử dụng lệnh ***help*** là cần thiết.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image86.png?raw=true)

=> Mô phỏng hành vi xâm phạm ban đầu đối với SMB (Khối tin nhắn máy chủ) (sử dụng module ***exploit/windows/smb/psexec***): 

    Username: ballen
    Password: Password1

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image87.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image88.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image89.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image90.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image91.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image92.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image93.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Metasploit/images/image94.png?raw=true)





















