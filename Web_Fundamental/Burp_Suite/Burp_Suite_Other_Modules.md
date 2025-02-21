# Burp Suite: Other Modules

***Decoder: Overview***

Đúng như tên gọi, nó không chỉ giải mã data intercepted mà còn cung cấp chức năng mã hóa data, chuẩn bị cho việc truyền data đến mục tiêu. Decoder cũng cho phép tạo các giá trị băm của data, cũng như cung cấp tính năng Smart Decode, cố gắng giải mã data được cung cấp theo cách đệ quy cho đến khi nó trở lại dạng plaintext

Để truy cập Decoder, điều hướng đến tab Decoder từ menu trên cùng để xem các tùy chọn có sẵn: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image42.png?raw=true)

Giao diện này đưa ra 1 số tùy chọn: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image43.png?raw=true)

1. Vùng này đóng vai trò là không gian làm việc để nhập hoặc dán data yêu cầu decoding hoặc encoding.

2. Ở đầu list bên phải, có 1 tùy chọn để xử lý input dưới dạng text hoặc hexadecimal byte values.

3. Khi di chuyển xuống dưới danh sách, dropdown sẽ xuất hiện để encode, decode, hoặc hash input.

4. Tính năng Smart Decode, nằm ở cuối, cố gắng auto-decode input

Khi nhập data vào input field, giao diện sẽ tự sao chép để hiển thị output hoạt động. Sau đó, có thể áp dụng các phép biến đổi tiếp theo bằng cách sử dụng các tùy chọn tương tự:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image44.png?raw=true)

***Decoder: Encoding/ Decoding***

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image45.png?raw=true)

- Plain: Đề cập đến văn bản thô trước khi áp dụng bất kỳ chuyển đổi nào. 

- URL: URL encoding được dùng để đảm bảo truyền data an toàn trong URL của web request. Nó liên quan đến việc thay thế các ký tự cho mã ký tự ASCII của chúng ở hexadecimal format, đứng trước ký hiệu %. Phương pháp này rất quan trọng đối với bất kỳ loại thử nghiệm web application nào.

Ví dụ, mã hóa ký tự gạch chéo (/), có mã ký tự ASCII là 47, sẽ được chuyển thành 2F ở dạng hexadecimal, do đó trở thành %2F trong URL encoding.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image46.png?raw=true)

- HTML: HTML encoding thay thế các ký tự đặc biệt bằng ký hiệu và (&), theo sau là hexadecimal number hoặc tham chiếu đến ký tự being escaped và kết thúc bằng dấu (;). Phương pháp này đảm bảo hiển thị an toàn các ký tự đặc biệt trong HTML, giúp ngăn chặn các tấn công như XSS.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image47.png?raw=true)

- Base64: 1 phương pháp encode thường được sử dụng, chuyển đổi mọi data sang định dạng tương thích với ASCII

- ASCII Hex: Chuyển đổi data giữa ASCII và hexadecimal. Ví dụ, từ “ASCII” có thể được chuyển đổi thành hexadecimal number “4153434949”.

- Hex, Octal, and Binary: Các phương pháp encode này chỉ áp dụng cho input là số, chuyển đổi giữa các biểu diễn decimal, hexadecimal, octal (base eight) và binary.

- Gzip: Gzip nén data, giảm kích thước file và page trước khi truyền qua browser. Decoder tạo điều kiện thuận lợi cho việc mã hóa và giải mã thủ công data gzip, mặc dù nó thường không hợp lệ với ASCII / Unicode. Ví dụ: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image48.png?raw=true)

***Hex Format***

Mặc dù việc nhập data ở định dạng ASCII là có lợi nhưng đôi khi cần phải chỉnh sửa input data theo từng byte. Đây là lúc “Hex View” tỏ ra hữu ích: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image49.png?raw=true)

Tính năng này cho phép xem và thay đổi data ở định dạng hexadecimal byte, 1 tool quan trọng khi làm việc với binary file hoặc data không phải ASCII khác.

***Smart Decode***

Cuối cùng, tùy chọn Smart Decode. Tính năng này cố gắng tự động giải mã văn bản được encode. VD, 

    &#x42;&#x75;&#x72;&#x70;&#x20;&#x53;&#x75;&#x69;&#x74;&#x65; 

được tự động nhận dạng dưới dạng mã hóa HTML và được giải mã tương ứng.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image50.png?raw=true)

Mặc dù không hoàn hảo, tính năng này có thể là giải pháp nhanh chóng để giải mã các khối dữ liệu không xác định.

***Decoder: Hashing***

Decoder còn cung cấp khả năng tạo hash sum cho data. 

Hashing là quá trình 1 chiều, biến đổi data thành chữ ký duy nhất. Để 1 hàm đủ điều kiện làm thuật toán băm, output mà nó tạo ra phải không thể đảo ngược. Thuật toán băm thành thạo đảm bảo rằng mọi input data sẽ tạo ra 1 hàm băm hoàn toàn duy nhất. 

Ví dụ, sử dụng thuật toán MD5 để tạo giá trị băm cho văn bản ***“MD5sum”*** sẽ trả về ***4ae1a02de5bd02a5515f583f4fca5e8c***. Việc sử dụng cùng 1 thuật toán cho ***“MD5SUM”*** sẽ mang lại hàm băm hoàn toàn khác mặc dù input data rất giống nhau: ***13b436b09172400c9eb2f69fbd20adad***.

=> Do đó, hàm băm thường được dùng để xác minh tính toàn vẹn của file và documents, vì ngay cả 1 thay đổi nhỏ đối với file cũng có thể thay đổi đáng kể hàm băm.

Hơn nữa, hàm băm được sử dụng để lưu trữ password 1 cách an toàn vì quy trình băm 1 chiều làm cho password tương đối an toàn, ngay cả khi DB bị xâm phạm. Khi user tạo password, ứng dụng sẽ băm và lưu trữ pass đó. Trong quá trình Login, ứng dụng sẽ băm password đã gửi và so sánh nó với hàm băm được lưu trữ; nếu khớp thì password đúng. Sử dụng phương pháp này, ứng dụng không bao giờ cần lưu trữ password ở dạng plaintext.

***Hashing in Decoder***

Decoder cho phép ta tạo giá trị băm cho data trực tiếp trong Burp Suite; nó hoạt động tương tự các tùy chọn encoding/decoding.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image51.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image52.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image53.png?raw=true)


***Comparer: Overview***

Comparer, như tên của nó, cho phép ta so sánh hai phần dữ liệu, bằng các từ ASCII hoặc bằng byte.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image54.png?raw=true)

1. Các mục cần so sánh. Khi load data vào Comparer, nó sẽ xuất hiện dưới dạng các rows trong các bảng này

2. Ở phía trên bên phải, có các tùy chọn dán data từ clipboard (Paste), tải data từ file (Load), xóa hàng hiện tại (Remove) và xóa tất cả datasets (Clear)

3. Cuối cùng, ở phía dưới bên phải, có thể chọn so sánh 2 datasets theo words hoặc bytes

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image55.png?raw=true)

1. Dữ liệu được so sánh chiếm phần lớn cửa sổ; nó có thể được xem ở định dạng text hoặc hex

2. Phím so sánh nằm ở phía dưới cùng bên trái, hiển thị màu nào thể hiện data đã sửa đổi, xóa và thêm

3. Sync views checkbox nằm ở dưới cùng bên phải. Khi được chọn, nó đảm bảo rằng cả 2 datasets sẽ đồng bộ hóa các định dạng

***Comparer: Example***

Thử Login và send to repeater: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image56.png?raw=true)

Chuyển response này sang comparer: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image57.png?raw=true)

Thử login lại với username và password:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image58.png?raw=true)

Chuyển response thứ 2 sang comparer và so sánh: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image59.png?raw=true)

***Sequencer: Overview***

Sequencer cho phép ta đánh giá entropy hoặc tính ngẫu nhiên của tokens. Tokens là các chuỗi được sử dụng để xác định thứ gì đó, lý tưởng nhất là phải được tạo theo cách bảo mật bằng mật mã. Các Tokens này có thể là session cookie hoặc Cross-SIte Request Forgery (CSRF) tokens, được dùng để bảo vệ việc gửi form.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image60.png?raw=true)

Có 2 cách chính để thực hiện phân tích tokens với Sequencer: 

- Live Capture: Phương pháp phổ biến hơn và là tab phụ mặc định cho Sequencer. Live Capture cho phép ta chuyển request sẽ tạo Token tới Sequencer để phân tích. Ví dụ, ta có thể muốn chuyển POST request tới  login endpoint tới Sequencer, biết rằng server sẽ response bằng cookie. Với request được chuyển đến, ta có thể hướng dẫn Sequencer bắt đầu live capture. Sau đó, nó sẽ tự động thực hiện cùng 1 request hàng nghìn lần, lưu trữ các mẫu Token được tạo để phân tích. Sau khi thu thập đủ mẫu, ta dừng Sequencer và cho phép nó phân tích Token thu được.

- Manual Load: Cho phép tải trực tiếp danh sách tokens được tạo trước vào Sequencer để phân tích. Sử dụng Manual Load nghĩa là ta không cần thực hiện hàng nghìn request tới mục tiêu, việc này có thể gây tốn tài nguyên. Tuy nhiên, nó yêu cầu ta phải có 1 list lớn token được tạo trước.

***Sequencer: Live Capture***

Intercept request đến http://10.10.86.85/admin/login/ và Send to Sequencer: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image61.png?raw=true)

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image62.png?raw=true)

Ở phần Token Location Within Response, chọn form field và chọn loginToken vì ta đang testing loginToken:

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image63.png?raw=true)

Click Start: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image64.png?raw=true)

Cửa sổ mới hiển thị cho biết quá trình, hiển thị số token đã bắt được. Đợi đến khi thu khoảng 10000 tokens thì click pause và analyze now

Sau khi phân tích: 

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image65.png?raw=true)

***Sequencer: Analysis***

- Overall result: đưa ra đánh giá rộng rãi về tính bảo mật của cơ chế tạo token. Trong TH này, mức entropy cho biết tokens được tạo 1 cách an toàn.

- Effective entropy: Đo lường tính ngẫu nhiên của tokens. Entropy hiệu quả của 117 bit tương đối cao, cho thấy tokens đủ ngẫu nhiên và do đó, an toàn đối với các cuộc tấn công prediction hoặc brute-force.

- Reliability: Mức ý nghĩa 1 % ngụ ý rằng độ tin cậy của kết quả là 99%. Mức độ tin cậy này khá cao, mạng lại sự bảo đảm về tính chính xác của ước tính entropy hiệu quả.

- Sample: phần này cung cấp thông tin chi tiết về các token sample được phân tích trong quá trình thử nghiệm entropy, bao gồm số lượng tokens và đặc điểm của chúng.

***Organizer: Overview***

Organizer module của Burp Suite được thiết kế giúp ta lưu trữ và chú thích các bản sao của HTTP request mà có thể xem lại sau này.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image66.png?raw=true)

Request được lưu trong 1 table, chứa các cột như index number, time mà request được tạo, workflow status, Burp tool mà request được gửi từ đó, HTTP method, server hostname, URL path, URL query string, số lượng tham số trong request, HTTP status code của response, độ dài của response tính bằng byte và bất kỳ ghi chú nào mà ta đã thực hiện.

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image67.png?raw=true)

Để xem request và response: 

- Click vào bất kỳ Organizer item nào

- Request và Response đều là read-only. Ta có thể search từ, cụm từ trong request và response

![img](https://github.com/DucThinh47/TryHackMe/blob/main/Web_Fundamental/Burp_Suite/images/image68.png?raw=true)










