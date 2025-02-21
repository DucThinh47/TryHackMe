# Burp Suite: Repeater

***What is Repeater?***

=> Về bản chất, Repeater cho phép ***sửa đổi*** và ***gửi lại*** các request bị chặn đến mục tiêu. Cho phép nhận các request được ghi lại trong Burp Proxy và thao tác chúng, gửi chúng nhiều lần nếu cần. Ngoài ra, có thể tạo các request từ đầu theo cách thủ công, tương tự như sử dụng công cụ dòng lệnh .
***curl***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image7.png?raw=true)

1. ***Request List***: Nằm ở phía bên trái của tab, hiển thị danh sách các request Repeater.

2. ***Request Controls***: Đặt ngay bên dưới Request List, các điều khiển này cho phép ***gửi request***, ***hủy request*** và ***điều hướng*** qua lịch sử request.

3. ***Request and Response View***: Chiếm phần lớn giao diện, phần này hiển thị các chế độ xem request và response. Có thể chỉnh sửa request trong chế độ xem Request rồi forward Request đó, trong khi response tương ứng sẽ được hiển thị trong chế độ xem Response. 

4. ***Layout Options***: Nằm ở phía bên phải của chế độ xem Request / Response, các tùy chọn này cho phép tùy chỉnh bố cục ở chế độ xem Request và Respóne. Thiết lập mặc định là bố cục ***side-by-side*** (ngang), tuy nhiên cũng có thể chọn bố cục dọc hoặc kết hợp chúng trong các tab riêng biệt.

5. ***Inspector***: Được đặt ở phía bên phải, cho phép phân tích và sửa đổi request theo cách trực quan hơn so với trình soạn thảo thô. 

6. ***Target***: Nằm phía trên Inspector, chỉ định địa chỉ IP hoặc domain mà request được gửi tới. Khi các request được gửi đến Repeater từ các thành phần Burp Suite khác, trường này sẽ tự động được điền.

***Basic Usage***

Mặc dù việc tạo request thủ công là 1 tùy chọn, nhưng cách phổ biến hơn là ghi lại request bằng Proxy module và sau đó truyền request đó đến Repeater để chỉnh sửa thêm và ghi lại.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image8.png?raw=true)

Cả 2 phần ***Target*** và ***Inspector*** hiện đều hiển thị thông tin liên quan, mặc dù chưa có response. Khi nhấp vào nút ***Send***, Response sẽ xuất hiện: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image9.png?raw=true)

Khi muốn sửa đổi request, ví dụ, thay đổi ***Connection header*** hành ***“open”*** thay vì ***“close”*** sẽ mang lại response với header ***Connection: keep-alive***.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image10.png?raw=true)

=> Repeater module cũng có thể xem được History. 

***Message Analysis Toolbar***

Repeater cung cấp nhiều tùy chọn trình bày request và response khác nhau, từ output thập lục phân đến trang được hiển thị đầy đủ.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image11.png?raw=true)

1. ***Pretty***:  tùy chọn mặc định, nhận response thô và áp dụng các cải tiến nhỏ về định dạng để cải thiện khả năng đọc

2. ***Raw***: Tùy chọn này hiển thị response chưa sửa đổi nhận được trực tiếp từ server

3. ***Hex***: Bằng cách chọn chế độ xem này, có thể kiểm tra response ở dạng biểu diễn cấp byte, điều này đặc biệt hữu ích khi xử lý các tệp nhị phân

4. ***Render***: Tùy chọn cho phép trực quan hóa trang như khi nó xuất hiện trong browser. Mặc dù không được sử dụng phổ biến trong Repeater vì ta thường tập trung vào mã nguồn, nhưng nó vẫn cung cấp tính năng có giá trị

5. ***\n***: hiển thị các ký tự không hiển thị được bằng tùy chọn ***Pretty*** hoặc ***Raw***. (ví dụ: \r, \n).

***Inspector***

Inspector là tính năng bổ sung cho chế độ xem Request và Response trong Repeater module. Nó cũng được sử dụng để có được bản phân tích các request và response được sắp xếp trực quan, cũng như để thử nghiệm xem những thay đổi được thực hiện bằng Inspector cấp cao hơn ảnh hưởng như thế nào đến các phiên bản raw tương đương.

Inspector có thể được sử dụng trong cả Proxy module và Repeater module.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image12.png?raw=true)

Trong số các thành phần này, các phần liên quan đến Request thường có thể được sửa đổi, cho phép thêm, chỉnh sửa và xóa các mục. Ví dụ, trong phần ***Request attributes***, ta có thể thay đổi các thành phần liên quan đến location, method và protocol của request. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image13.png?raw=true)

Các thành phần có thể chỉnh sửa khác: 

- ***Request Query Parameters***: Tham số này đề cập đến data được gửi đến server qua URL. Ví dụ: trong GET request như https://admin.tryhackme.com/?redirect=false, tham số ***redirect*** được đặt là false.

- ***Request Body Parameters***: Tương tự như Query Parameters, nhưng dành riêng cho POST request. Mọi data được gửi như 1 phần của POST request sẽ được hiển thị trong phần này, cho phép ta sửa đổi các tham số trước khi gửi lại.

- ***Request Cookies***: Chứa danh sách các cookie có thể sửa đổi được gửi cùng với mỗi request.

- ***Request Headers***:  Cho phép xem, truy cập và sửa đổi (bao gồm thêm hoặc xóa) bất kỳ header nào được gửi cùng với request.

- ***Response Headers***:  Phần này hiển thị các header được server trả về để đáp ứng request. Không thể sửa đổi nó vì không có quyền kiểm soát các header được server trả về. Lưu ý, phần này chỉ hiển thị sau khi gửi request và nhận được response.

***Practical Example***

Repeater đặc biệt phù hợp với các tác vụ request gửi lặp đi lặp lại các request tương tự, thường có những sửa đổi nhỏ. 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image14.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image15.png?raw=true)

=> Thay id product thành một giá trị không hợp lệ như ***-1***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image16.png?raw=true)

***Extra-mile Challenge***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image17.png?raw=true)

=> Thử gửi Request đến ***/about/2'***:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image18.png?raw=true)

=> Response chứa dòng sau: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image19.png?raw=true)

=> Vì đã biết tên bảng và số cột, có thể sử dụng truy vấn ***UNION*** để chọn tên column cho bảng people trong Database mặc định ***information_schema***. Sử dụng request: 

***/about/0 UNION ALL SELECT column_name,null,null,null,null FROM information_schema.columns WHERE table_name="people"***

- ***/about/0***:

=> ID không hợp lệ, đảm bảo truy vấn gốc không trả về kết quả nào. 

=> Mục đích: Để kết quả từ truy vấn chèn xuất hiện đầu tiên.

- ***UNION ALL SELECT column_name, null, null, null, null***:

=> UNION ALL: Kết hợp kết quả từ hai truy vấn.

=> SELECT column_name: Lấy tên các cột từ bảng people.

=> null, null, null, null: Thêm 4 giá trị null để đảm bảo số lượng cột phù hợp với truy vấn gốc, tránh lỗi.

- ***FROM information_schema.columns WHERE table_name="people"***:

=> information_schema.columns: Đây là bảng hệ thống chứa thông tin về các cột trong cơ sở dữ liệu.

=> WHERE table_name="people": Chỉ lấy thông tin về các cột trong bảng people.

Truy vấn này sẽ tìm tên các cột trong bảng ***people***. Sử dụng truy vấn ***UNION*** để kết hợp kết quả từ truy vấn gốc và truy vấn chèn. Sử dụng ID không hợp lệ để đảm bảo kết quả từ truy vấn chèn xuất hiện đầu tiên. Thêm các giá trị ***null*** để phù hợp với số lượng cột trong truy vấn gốc.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image20.png?raw=true)

=> Thành công lấy tên cột đầu tiên là ***id***.

=> Sử dụng hàm ***group_concat()***, có thể hợp nhất tất cả các tên cột thành 1 đầu ra duy nhất:

***/about/0 UNION ALL SELECT group_concat(column_name),null,null,null,null FROM information_schema.columns WHERE table_name="people"***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image21.png?raw=true)

=> Đã có thông tin về: 

- ***table name***: people

- ***target column***: notes

- ***id của CEO***: 1

=> Payload cuối: 

***0 UNION ALL SELECT notes,null,null,null,null FROM people WHERE id = 1***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image22.png?raw=true)












