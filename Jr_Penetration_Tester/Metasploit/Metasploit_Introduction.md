# Metasploit: Introduction

**Task 1: Introduction to Metasploit**

- Metasploit là framework khai thác được sử dụng rộng rãi nhất. Metasploit là một công cụ mạnh mẽ có thể hỗ trợ tất cả các giai đoạn của quá trình tham gia kiểm thử xâm nhập, từ thu thập thông tin đến khai thác.

- Metasploit Framework là một bộ công cụ cho phép thu thập, quét, khai thác, phát triển khai thác, hậu khai thác, v.v

**Task 2: Main Components of Metasploit**

- Khởi chạy Metasploit trong Kali Linux bằng lệnh ***msfconsole***

- Exploit: Một đoạn mã sử dụng lỗ hổng hiện có trên hệ thống đích.

- Vulnerability: Một lỗ hổng về thiết kế, mã hóa hoặc logic ảnh hưởng đến hệ thống mục tiêu. Việc khai thác lỗ hổng có thể dẫn đến tiết lộ thông tin bí mật hoặc cho phép kẻ tấn công thực thi mã trên hệ thống đích.

- Payload: Một Exploit sẽ tận dụng một Vulnerability. Tuy nhiên, nếu muốn việc khai thác đạt được kết quả như mong muốn (đạt được quyền truy cập vào hệ thống đích, đọc thông tin bí mật, v.v.), cần sử dụng Payload. Tải trọng là mã sẽ chạy trên hệ thống đích.

**Auxiliary**

- Có thể tìm thấy bất kỳ module hỗ trợ nào, như máy quét, trình thu thập thông tin,...

    root@ip-10-10-135-188:/opt/metasploit-framework/embedded/framework/modules# tree -L 1 auxiliary/
    auxiliary/
    ├── admin
    ├── analyze
    ├── bnat
    ├── client
    ├── cloud
    ├── crawler
    ├── docx
    ├── dos
    ├── example.py
    ├── example.rb
    ├── fileformat
    ├── fuzzers
    ├── gather
    ├── parser
    ├── pdf
    ├── scanner
    ├── server
    ├── sniffer
    ├── spoof
    ├── sqli
    ├── voip
    └── vsploit

    20 directories, 2 files

**Encoders**

- Bộ mã hóa sẽ cho phép mã hóa phần Exploit và Payload với hy vọng rằng giải pháp Signature-based antivirus có thể bỏ sót chúng.

- Signature-based antivirus và security solutions có cơ sở dữ liệu về các mối đe dọa đã biết. Họ phát hiện các mối đe dọa bằng cách so sánh các tệp đáng ngờ với cơ sở dữ liệu này và đưa ra cảnh báo nếu có sự trùng khớp. 

    root@ip-10-10-135-188:/opt/metasploit-framework/embedded/framework/modules# tree -L 1 encoders/
    encoders/
    ├── cmd
    ├── generic
    ├── mipsbe
    ├── mipsle
    ├── php
    ├── ppc
    ├── ruby
    ├── sparc
    ├── x64
    └── x86

    10 directories, 0 files

**Evasion**

- Mặc dù Encoders sẽ mã hóa Payload nhưng chúng không được coi là nỗ lực trực tiếp nhằm trốn tránh phần mềm chống vi-rút. Mặt khác, các mô-đun "Evasion" sẽ thử điều đó nhưng ít nhiều thành công.

    root@ip-10-10-135-188:/opt/metasploit-framework/embedded/framework/modules# tree -L 2 evasion/
    evasion/
    └── windows
        ├── applocker_evasion_install_util.rb
        ├── applocker_evasion_msbuild.rb
        ├── applocker_evasion_presentationhost.rb
        ├── applocker_evasion_regasm_regsvcs.rb
        ├── applocker_evasion_workflow_compiler.rb
        ├── process_herpaderping.rb
        ├── syscall_inject.rb
        ├── windows_defender_exe.rb
        └── windows_defender_js_hta.rb

    1 directory, 9 files

**Exploits**

- Khai thác, tổ chức gọn gàng theo hệ thống mục tiêu:

    root@ip-10-10-135-188:/opt/metasploit-framework/embedded/framework/modules# tree -L 1 exploits/
    exploits/
    ├── aix
    ├── android
    ├── apple_ios
    ├── bsd
    ├── bsdi
    ├── dialup
    ├── example_linux_priv_esc.rb
    ├── example.py
    ├── example.rb
    ├── example_webapp.rb
    ├── firefox
    ├── freebsd
    ├── hpux
    ├── irix
    ├── linux
    ├── mainframe
    ├── multi
    ├── netware
    ├── openbsd
    ├── osx
    ├── qnx
    ├── solaris
    ├── unix
    └── windows

    20 directories, 4 files

**NOPs**

- NOP (No Operation) không làm gì cả, theo nghĩa đen. Chúng được thể hiện trong dòng CPU Intel x86 với 0x90, theo đó CPU sẽ không làm gì trong một chu kỳ. Chúng thường được sử dụng làm bộ đệm để đạt được kích thước tải trọng nhất quán.

    root@ip-10-10-135-188:/opt/metasploit-framework/embedded/framework/modules# tree -L 1 nops/
    nops/
    ├── aarch64
    ├── armle
    ├── cmd
    ├── mipsbe
    ├── php
    ├── ppc
    ├── sparc
    ├── tty
    ├── x64
    └── x86

    10 directories, 0 files

**Payloads**

- Tải trọng là mã sẽ chạy trên hệ thống đích.

- Việc Exploit sẽ tận dụng lỗ hổng trên hệ thống mục tiêu, nhưng để đạt được kết quả mong muốn, cần Payload 

- Chạy lệnh trên hệ thống đích đã là một bước quan trọng nhưng có kết nối tương tác cho phép gõ các lệnh sẽ được thực thi trên hệ thống đích thì sẽ hiệu quả hơn. Dòng lệnh tương tác như vậy được gọi là "shell".

- Metasploit cung cấp khả năng gửi các payload khác nhau có thể mở shell trên hệ thống đích.

    root@ip-10-10-135-188:/opt/metasploit-framework/embedded/framework/modules# tree -L 1 payloads/
    payloads/
    ├── adapters
    ├── singles
    ├── stagers
    └── stages

    4 directories, 0 files

=> 4 directories khác nhau trong phần Payload: adapters, singles, stagers và stages.

    - Adapters: Bao bọc single payload để chuyển đổi chúng sang các định dạng khác nhau. Ví dụ: một single payload có thể được gói trong Powershell adapter để tạo ra lệnh Powershell thực thi payload đó.

    - Singles: Là các payload độc lập, tự chạy mà không cần tải thêm thành phần nào (ví dụ: thêm người dùng, khởi chạy notepad.exe).

    - Stagers: Thiết lập kênh kết nối giữa Metasploit và hệ thống đích, thường dùng cho staged payloads. Staged payloads sẽ tải lên một phần nhỏ trước, sau đó tải tiếp phần còn lại, giúp giảm kích thước ban đầu.

    - Stages: Là phần payload được tải xuống bởi stager, cho phép sử dụng payload lớn hơn

- Metasploit phân biệt single payload (inline payload) và staged payload qua tên:

    - Single payload: Ví dụ generic/shell_reverse_tcp, có dấu "_" giữa "shell" và "reverse".

    - Staged payload: Ví dụ windows/x64/shell/reverse_tcp, có dấu "/" giữa "shell" và "reverse".

**Post**

- Post modules sẽ hữu ích ở giai đoạn cuối cùng của quá trình thử nghiệm xâm nhập được liệt kê ở trên, sau khai thác:

    root@ip-10-10-135-188:/opt/metasploit-framework/embedded/framework/modules# tree -L 1 post/
    post/
    ├── aix
    ├── android
    ├── apple_ios
    ├── bsd
    ├── firefox
    ├── hardware
    ├── linux
    ├── multi
    ├── networking
    ├── osx
    ├── solaris
    └── windows

    12 directories, 0 files

**Task 3: Msfconsole**

![img](0)

![img](1)

=> hỗ trợ hầu hết các lệnh Linux, bao gồm cả lệnh clear (để xóa màn hình đầu cuối), nhưng sẽ không cho phép sử dụng một số tính năng của dòng lệnh thông thường (ví dụ: không hỗ trợ chuyển hướng đầu ra), như được thấy bên dưới:

![img](2)

=> Lệnh help:

![img](3)

- Có thể sử dụng lệnh history để xem các lệnh đã gõ trước đó.

![img](4)

- Msfconsole hoạt động theo ngữ cảnh, nghĩa là các thiết lập tham số chỉ áp dụng trong phạm vi mô-đun hiện tại. Nếu chuyển sang mô-đun khác, các tham số đã đặt (ví dụ: RHOSTS) sẽ bị mất và cần được thiết lập lại.

Ví dụ: khi sử dụng exploit ms17_010_eternalblue, đặt RHOSTS, nhưng nếu chuyển sang mô-đun khác (như scanner), phải đặt lại RHOSTS vì các thay đổi trước đó chỉ áp dụng trong ngữ cảnh của ms17_010_eternalblue.

Ví dụ: 

- Gõ lệnh use exploit/windows/smb/ms17_010_eternalblue.

- Dấu nhắc lệnh sẽ thay đổi từ msf6 thành msf6 exploit(windows/smb/ms17_010_eternalblue), cho thấy đang ở trong ngữ cảnh của exploit này.

![img](5)

- EternalBlue là một exploit được cho là do Cơ quan An ninh Quốc gia Hoa Kỳ (NSA) phát triển, khai thác lỗ hổng trong giao thức SMBv1 trên các hệ thống Windows. SMB (Server Message Block) là giao thức dùng để chia sẻ file và in ấn trong mạng Windows.

- Mặc dù lời nhắc đã thay đổi, nhận thấy vẫn có thể chạy các lệnh đã đề cập trước đó

![img](6)

- Sử dụng lệnh ***show options*** để xem rõ hơn bối cảnh làm việc: 

![img](7)

- Lệnh show options hiển thị các tùy chọn cần thiết cho mô-đun đang được sử dụng. Đầu ra của lệnh này thay đổi tùy theo ngữ cảnh. Ví dụ, với exploit ms17_010_eternalblue, cần đặt các biến như RHOSTS và RPORT. Trong khi đó, với mô-đun post-exploitation, chỉ cần đặt SESSION ID (một kết nối hiện có với hệ thống đích mà mô-đun sẽ sử dụng).

![img](8)

- Lệnh show có thể được dùng trong bất kỳ ngữ cảnh nào, theo sau là loại mô-đun (như auxiliary, payload, exploit, v.v.) để liệt kê các mô-đun có sẵn. Ví dụ, lệnh show payloads liệt kê các payload tương thích với exploit ms17-010 Eternalblue.

        msf6 exploit(windows/smb/ms17_010_eternalblue) > show payloads

        Compatible Payloads
        ===================

        #   Name                                        Disclosure Date  Rank    Check  Description
        -   ----                                        ---------------  ----    -----  -----------
        0   generic/custom                                               manual  No     Custom Payload
        1   generic/shell_bind_tcp                                       manual  No     Generic Command Shell, Bind TCP Inline
        2   generic/shell_reverse_tcp                                    manual  No     Generic Command Shell, Reverse TCP Inline
        3   windows/x64/exec                                             manual  No     Windows x64 Execute Command
        4   windows/x64/loadlibrary                                      manual  No     Windows x64 LoadLibrary Path
        5   windows/x64/messagebox                                       manual  No     Windows MessageBox x64
        6   windows/x64/meterpreter/bind_ipv6_tcp                        manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager
        7   windows/x64/meterpreter/bind_ipv6_tcp_uuid                   manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support 

- có thể rời khỏi bối cảnh bằng lệnh back.

![img](9)

- có thể lấy thêm thông tin về bất kỳ mô-đun nào bằng cách gõ lệnh info trong ngữ cảnh của nó:

        msf6 exploit(windows/smb/ms17_010_eternalblue) > info

            Name: MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
            Module: exploit/windows/smb/ms17_010_eternalblue
        Platform: Windows
            Arch: 
        Privileged: Yes
            License: Metasploit Framework License (BSD)
            Rank: Average
        Disclosed: 2017-03-14

        Provided by:
        Sean Dillon 
        Dylan Davis 
        Equation Group
        Shadow Brokers
        thelightcosine

        Available targets:
        Id  Name
        --  ----
        0   Windows 7 and Server 2008 R2 (x64) All Service Packs

        Check supported:
        Yes

        Basic options:
        Name           Current Setting  Required  Description
        ----           ---------------  --------  -----------
        RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:'
        RPORT          445              yes       The target port (TCP)
        SMBDomain      .                no        (Optional) The Windows domain to use for authentication
        SMBPass                         no        (Optional) The password for the specified username
        SMBUser                         no        (Optional) The username to authenticate as
        VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
        VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.

        Payload information:
        Space: 2000

        Description:
        This module is a port of the Equation Group ETERNALBLUE exploit, 
        part of the FuzzBunch toolkit released by Shadow Brokers. There is a 
        buffer overflow memmove operation in Srv!SrvOs2FeaToNt. The size is 
        calculated in Srv!SrvOs2FeaListSizeToNt, with mathematical error 
        where a DWORD is subtracted into a WORD. The kernel pool is groomed 
        so that overflow is well laid-out to overwrite an SMBv1 buffer. 
        Actual RIP hijack is later completed in 
        srvnet!SrvNetWskReceiveComplete. This exploit, like the original may 
        not trigger 100% of the time, and should be run continuously until 
        triggered. It seems like the pool will get hot streaks and need a 
        cool down period before the shells rain in again. The module will 
        attempt to use Anonymous login, by default, to authenticate to 
        perform the exploit. If the user supplies credentials in the 
        SMBUser, SMBPass, and SMBDomain options it will use those instead. 
        On some systems, this module may cause system instability and 
        crashes, such as a BSOD or a reboot. This may be more likely with 
        some payloads.

        References:
        https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/MS17-010
        https://cvedetails.com/cve/CVE-2017-0143/
        https://cvedetails.com/cve/CVE-2017-0144/
        https://cvedetails.com/cve/CVE-2017-0145/
        https://cvedetails.com/cve/CVE-2017-0146/
        https://cvedetails.com/cve/CVE-2017-0147/
        https://cvedetails.com/cve/CVE-2017-0148/
        https://github.com/RiskSense-Ops/MS17-010

        Also known as:
        ETERNALBLUE

        msf6 exploit(windows/smb/ms17_010_eternalblue) > 

- Ngoài ra, cũng có thể sử dụng lệnh info kèm theo đường dẫn của mô-đun trong msfconsole (ví dụ: info exploit/windows/smb/ms17_010_eternalblue). Lệnh này không chỉ hiển thị menu trợ giúp mà còn cung cấp thông tin chi tiết về mô-đun, bao gồm tác giả, nguồn tham khảo, và các thông tin liên quan khác.

**Search**

- Một trong những lệnh hữu ích nhất trong msfconsole là search. Lệnh này giúp tìm kiếm trong cơ sở dữ liệu của Metasploit Framework để tìm các mô-đun liên quan đến từ khóa hoặc tham số tìm kiếm. Có thể tìm kiếm bằng số CVE, tên exploit (ví dụ: eternalblue, heartbleed), hoặc hệ thống mục tiêu.

![img](10)

- Kết quả từ lệnh search cung cấp một cái nhìn tổng quan về các mô-đun được tìm thấy. Cột "Name" không chỉ hiển thị tên mô-đun mà còn cho biết loại mô-đun (ví dụ: auxiliary, exploit) và danh mục của nó (ví dụ: scanner, admin, windows, Unix). Có thể sử dụng bất kỳ mô-đun nào trong kết quả tìm kiếm bằng cách dùng lệnh use kèm theo số thứ tự của mô-đun (ví dụ: use 2 thay vì use auxiliary/admin/smb/ms17_010_command).

- Một thông tin quan trọng khác là cột "Rank", đánh giá độ tin cậy của exploit. Bảng dưới đây mô tả các mức độ đánh giá này:

    - ExcellentRanking: Khai thác cực kỳ đáng tin cậy, ít gây ảnh hưởng đến hệ thống mục tiêu.

    - GreatRanking: Khai thác đáng tin cậy nhưng có thể yêu cầu điều kiện cụ thể.

    - GoodRanking: Khai thác có độ tin cậy trung bình, có thể không hoạt động trong mọi trường hợp.

    - NormalRanking: Khai thác có độ tin cậy thấp hơn, thường yêu cầu điều kiện phức tạp.

    - AverageRanking: Khai thác có độ tin cậy thấp, thường không ổn định.

    - LowRanking: Khai thác kém tin cậy, dễ gây lỗi hoặc không hoạt động.

    - ManualRanking: Khai thác yêu cầu can thiệp thủ công và không tự động.

- có thể tùy chỉnh chức năng search bằng cách sử dụng các từ khóa như type và platform. Ví dụ, nếu muốn kết quả tìm kiếm chỉ bao gồm các mô-đun auxiliary, có thể thêm điều kiện type:auxiliary. Dưới đây là ví dụ về lệnh search type:auxiliary telnet, kết quả sẽ chỉ hiển thị các mô-đun auxiliary liên quan đến "telnet":

![img](11)

**Task 4: Working with modules**

- The regular command prompt: không thể sử dụng lệnh Metasploit ở đây.

![img](12)

- The msfconsole prompt: msf6 (hoặc msf5 tùy thuộc vào phiên bản đã cài đặt) là lời nhắc msfconsole. Như có thể thấy, không có ngữ cảnh nào được đặt ở đây, vì vậy các lệnh theo ngữ cảnh cụ thể để đặt tham số và chạy mô-đun không thể được sử dụng ở đây.

![img](13)

- A context prompt: Khi đã quyết định sử dụng một mô-đun và sử dụng lệnh set để chọn nó, msfconsole sẽ hiển thị ngữ cảnh. Có thể sử dụng các lệnh theo ngữ cảnh cụ thể (ví dụ: đặt RHOSTS 10.10.x.x) tại đây.

![img](14)

- The Meterpreter prompt: Meterpreter là một payload quan trọng. Điều này có nghĩa là tác nhân Meterpreter đã được tải vào hệ thống đích và kết nối lại. Có thể sử dụng các lệnh cụ thể của Meterpreter tại đây.

![img](15)

- A shell on the target system: Sau khi quá trình khai thác hoàn tất, có thể có quyền truy cập vào shell lệnh trên hệ thống đích. Đây là một dòng lệnh thông thường và tất cả các lệnh được gõ ở đây đều chạy trên hệ thống đích.

=> lệnh show options sẽ liệt kê tất cả các tham số có sẵn.

![img](16)

![img](17)

- Các thông số thường dùng trong Metasploit bao gồm:

1. RHOSTS: "Remote Host" – Địa chỉ IP của hệ thống mục tiêu. Có thể nhập một địa chỉ IP duy nhất, một phạm vi mạng (sử dụng ký hiệu CIDR như /24, /16) hoặc một dải IP (ví dụ: 10.10.10.x – 10.10.10.y). Cũng có thể sử dụng một tệp chứa danh sách các mục tiêu, mỗi dòng một địa chỉ IP, bằng cách dùng file:/path/to/target_file.txt:

![img](18)

2. RPORT: "Remote Port" – Cổng trên hệ thống mục tiêu mà dịch vụ dễ bị tấn công đang chạy.

3. PAYLOAD: Tải trọng (payload) sẽ được sử dụng khi khai thác.

4. LHOST: "Localhost" – Địa chỉ IP của máy tấn công.

5. LPORT: "Local Port" – Cổng trên máy tấn công dùng để kết nối ngược (reverse shell). Có thể chọn bất kỳ cổng nào chưa được sử dụng bởi ứng dụng khác.

6. SESSION: Mỗi kết nối thành công với hệ thống mục tiêu sẽ có một Session ID. Tham số này được dùng trong các mô-đun post-exploitation để tương tác với hệ thống mục tiêu thông qua kết nối hiện có.

=> có thể thay đổi giá trị của bất kỳ tham số nào bằng lệnh set với giá trị mới. Để xóa giá trị của một tham số, sử dụng lệnh unset. Nếu muốn xóa tất cả các tham số đã đặt, dùng lệnh unset all.

![img](19)

- có thể sử dụng lệnh setg để đặt các giá trị sẽ được sử dụng cho tất cả các mô-đun. 

- nếu sử dụng lệnh set để đặt giá trị bằng mô-đun và chuyển sang mô-đun khác, cần đặt lại giá trị. Lệnh setg cho phép đặt giá trị để nó có thể được sử dụng theo mặc định trên các mô-đun khác nhau.

-  có thể xóa bất kỳ giá trị nào được đặt bằng setg bằng unsetg.

Ví dụ: 

1. sử dụng ms17_010_eternalblue có thể khai thác được

2. đặt biến RHOSTS bằng lệnh setg thay vì lệnh set

3. sử dụng lệnh back để thoát khỏi bối cảnh khai thác

4. sử dụng auxiliary (mô-đun này là một máy quét để phát hiện các lỗ hổng MS17-010)

5. Lệnh show options hiển thị tham số RHOSTS đã được điền địa chỉ IP của hệ thống đích.

![img](20)

**Using modules**

- Sau khi tất cả các tham số mô-đun được đặt,có thể khởi chạy mô-đun bằng lệnh exploit. 

- Metasploit cũng hỗ trợ lệnh run, đây là bí danh được tạo cho lệnh exploit vì từ exploit không có ý nghĩa khi sử dụng các mô-đun không khai thác (máy quét cổng, máy quét lỗ hổng, v.v.).

- Lệnh exploit có thể được sử dụng mà không cần bất kỳ tham số nào hoặc sử dụng tham số "-z". Lệnh exploit -z sẽ chạy exploit và chạy background session ngay khi nó mở.

![img](21)

**Sessions**

- Khi một lỗ hổng đã được khai thác thành công, một session sẽ được tạo.

- Đây là kênh liên lạc được thiết lập giữa hệ thống đích và Metasploit.

- sử dụng lệnh background để làm background cho lời nhắc session và quay lại lời nhắc msfconsole.

![img](22)

![img](23)

- Để tương tác với bất kỳ phiên nào, có thể sử dụng lệnh session -i theo sau là số session mong muốn.

**Task 5: Summary**

- Metasploit là một công cụ mạnh mẽ hỗ trợ quá trình khai thác

- Metasploit cung cấp nhiều mô-đun mà có thể sử dụng cho từng bước của quy trình khai thác.



















