# Command Injection

Lỗ hổng này tồn tại do các ứng dụng thường sử dụng functions trong các ngôn ngữ lập trình như ***PHP***, ***Python*** và ***NodeJS*** để truyền data và thực hiện ***system calls*** trên OS của máy. Ví dụ, lấy input data từ 1 Field và tìm kiếm mục nhập vào 1 file. Đoạn code dưới đây làm ví dụ: 

Trong đoạn code này, ứng dụng lấy data mà User nhập vào field có tên là ***$title*** để tìm kiếm tên bài hát trong directory:

![img](141)

1. Ứng dụng lưu trữ MP3 files trong 1 directory trên OS

2. User nhập tên bài hát muốn tìm. Ứng dụng lưu trữ thông tin Input này vào biến ***$title***

3. Dữ liệu trong biến ***$title*** được chuyển đến lệnh ***grep*** để tìm kiếm tệp văn bản có tên ***songtitle.txt*** để tìm tên bài hát User nhập.

4. Output của tìm kiếm trong tệp ***songtitle.txt*** này sẽ xác định xem ứng dụng có thông báo cho User rằng bài hát đó có tồn tại hay không

Hiện nay, loại thông tin này thường được lưu trữ trong Database; tuy nhiên, đây chỉ là 1 ví dụ về việc ứng dụng lấy Input từ User để tương tác với OS của ứng dụng.

=> Thay vì sử dụng ***grep*** để tìm kiếm tên bài hát trong ***songtitle.txt***, Attacker yêu cầu ứng dụng đọc data từ một file nhạy cảm hơn.


Ví dụ 1 đoạn mã Python: 

![img](142)

1. ***“flask”*** package được dùng để thiết lập Web Server.

2. Hàm sử dụng ***“subprocess”*** package để thực thi Command trên thiết bị.

3. Sử dụng 1 route trong Web Server sẽ thực thi bất kỳ điều gì được cung cấp. Ví dụ, để thực thi ***whoami***, truy cập http://flaskapp.thm/whoami

***Phát hiện Command Injection bằng hành vi của ứng dụng***

Ví dụ, các toán tử shell ***“;”*** , ***“&”*** và ***“&&”*** sẽ kết hợp hai (hoặc nhiều) lệnh hệ thống và thực thi cả hai. 

Command Injection có thể được phát hiện chủ yếu theo 1 trong 2 cách: 

- ***Blind command injection***: Kiểu Command Injection này không có Output trực tiếp từ ứng dụng khi kiểm tra Payload

- ***Verbose command injection***: Kiểu Command Injection này có phản hồi trực tiếp từ ứng dụng sau khi đã kiểm tra Payload

***Phát hiện Blind Command Injection***

Đối với kiểu Command Injection này, ta cần sử dụng Payload gây ra ***time delay***. Ví dụ, lệnh ***ping*** và lệnh ***sleep*** là Payload đáng để kiểm tra. Lấy ping làm ví dụ, ứng dụng sẽ treo trong x giây tùy theo số lượng ping chỉ định.

1 phương pháp khác để phát hiện Blind Injection là ***ép buộc 1 số Output***. Điều này có thể thực hiện bằng cách sử dụng toán tử chuyển hướng như ***“>”***. Ví dụ, có thể yêu cầu Web Application thực thi các lệnh như ***whoami*** và chuyển hướng lệnh đó đến 1 file. Sau đó sử dụng lệnh như ***cat*** để đọc nội dung tệp này. 

Lệnh ***curl*** là 1 cách tốt để kiểm tra Command Injection. Điều này là do có thể dùng curl để gửi data đến và đi từ 1 ứng dụng trong Payloads của mình. Lấy đoạn mã này làm ví dụ: 

***curl http://vulnerable.app/process.php%3Fsearch%3DThe%20Beatles%3B%20whoami***

***Phát hiện Verbose Command Injection***

Trong Linux có thể sử dụng: 

Nhập payload trực tiếp do kiểu command injection này sẽ in ra output trực tiếp.

- whoami

- ls

- ping

- sleep

- nc

Trong Windows có thể sử dụng:

- whoami

- dir

- ping

- timeout

***Remediating Command Injection***

***Vulnerable Functions:***

Trong PHP, nhiều Function tương tác với OS để thực thi lệnh thông qua shell: 

- Exec
- Passthru
- System

Đoạn mã ví dụ. Tại đây, ứng dụng chỉ chấp nhận và xử lý các số được nhập vào Form. Có nghĩa là bất kỳ lệnh nào như ***whoami*** sẽ không được xử lý.

![img](143)

1. Ứng dụng chỉ chấp nhận 1 mẫu ký tự cụ thể (các chữ số 0-9)

2. Sau đó, ứng dụng chỉ tiến hành thực thi dữ liệu hoàn toàn là số.

Các hàm nguy hiểm này lấy Input như chuỗi hoặc User data và sẽ thực thi bất kỳ điều gì được cung cấp trên hệ thống. Bất kỳ ứng dụng nào sử dụng các Function này mà không được kiểm tra sẽ dễ bị Command Injection.

***Input sanitisation (“vệ sinh” đầu vào)***

Dọn dẹp mọi Input từ User mà ứng dụng sử dụng là 1 cách tốt để ngăn chặn Command Injection. Đây là quá trình chỉ định định dạng hoặc kiểu Data mà User có thể submit. Ví dụ, trường nhập chỉ chấp nhận dữ liệu số, xóa bất kỳ ký tự đặc biệt nào như “>”, “&” và “/”

Ví dụ, hàm PHP dưới dây kiểm tra xem input có phải số hay không: 

![img](144)

***Bypassing Filters***

Ví dụ, 1 ứng dụng có thể loại bỏ dấu ngoặc kép; thay vào đó, ta có thể sử dụng giá trị thập lục phân của giá trị này để đạt kết quả tương tự.

Khi thực thi, mặc dù data đưa ra sẽ ở định dạng khác với định dạng mong đợi nhưng nó vẫn có thể được diễn giải và cho kết quả tương tự.

![img](145)

***Practical: Command Injection (Deploy)***

![img](146)

![img](147)






















