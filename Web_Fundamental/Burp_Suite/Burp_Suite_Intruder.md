# Burp Suite: Intruder

Burp Suite’s Intruder module là 1 tool mạnh mẽ cho phép thực hiện các cuộc tấn công tự động và có thể tùy chỉnh.

Intruder đặc biệt hữu ích cho các tác vụ như ***fuzzing*** và ***brute-forcing***, trong đó các giá trị khác nhau cần được kiểm tra đối với mục tiêu.

Bằng cách sử dụng 1 request đã được ghi lại (thường là từ Proxy module). Intruder có thể ***gửi nhiều request*** với các giá trị được thay đổi 1 chút dựa trên cấu hình do user xác định.

Intruder phục vụ nhiều mục đích khác nhau, ví dụ như ***brute-forcing login forms*** bằng cách thay đổi các trường ***username*** và ***password*** bằng các giá trị từ wordlists hoặc thực hiện các cuộc tấn công fuzzing bằng cách sử dụng wordlists để kiểm tra thư mục con, endpoints hoặc virtual hosts. 

Có thể nói Intruder có thể so sánh với các tool như ***Wfuzz*** hoặc **ffuf***.

![img](23)

Có 4 ***sub-tabs*** trong Intruder:

- ***Positions***: Tab này cho phép chọn attack type và định cấu hình nơi muốn chèn payload vào request template.

- ***Payloads***: Tại đây, có thể chọn các giá trị để chèn vào các vị trí được xác định trong Positions tab.

- ***Resource Pool***: Tab này không đặc biệt hữu ích trong Burp Suite Community version. Nó cho phép phân bổ tài nguyên giữa các tác vụ tự động khác nhau trong Burp Professional.

- ***Settings***: Cho phép cấu hình hành vi attack. Nó chủ yếu đề cập đến cách Burp xử lý kết quả và bản thân cuộc attack

***Lưu ý***: Thuật ngữ ***fuzzing*** đề cập đến quá trình kiểm tra ***chức năng*** hoặc ***sự tồn tại*** bằng cách áp dụng 1 tập hợp dữ liệu cho 1 tham số. Ví dụ: fuzzing endpoints trong web application bao gồm việc lấy từng từ trong wordlist là thêm nó vào URL request (vd: http://10.10.159.252/WORD_GOES_HERE) để quan sát response từ server.

Khi sử dụng Burp Suite Intruder để thực hiện 1 cuộc tấn công, bước đầu tiên là kiểm tra các ***vị trí trong request*** muốn chèn payload.

![img](24)

***Lưu ý***, Burp Suite Intruder tự động cố gắng xác định các vị trí có khả năng xảy ra nhất nơi payload có thể được chèn vào. Các vị trí này được đánh dấu bằng màu xanh lá cây và được bao quanh bởi các dấu ***(§)***.

- ***Add §*** cho phép xác định vị trí chèn payload mới theo cách thủ công

- ***Clear §*** cho phép loại bỏ tất cả vị trí đã xác định

- ***Auto §*** cố gắng xác định các vị trí có khả năng xảy ra nhất dựa trên request

Trong tab ***Payloads*** của Burp Suite Intruder, có thể tạo, chỉ định và định cấu hình payload cho cuộc tấn công:

![img](25)

- ***Payload Sets***: 

    - cho phép chọn vị trí muốn cấu hình payload set và chọn loại payload.

    - Khi sử dụng các kiểu tấn công chỉ cho phép 1 payload set duy nhất (***Sniper*** hoặc ***Battering Ram***), “Payload Set” dropdown chỉ có 1 tùy chọn, bất kể số lượng đã được xác định.

    - Nếu sử dụng các kiểu attack yêu cầu nhiều payload sets (***Pitchfork*** hoặc ***Cluster Bomb***), sẽ có 1 mục trong dropdown cho mỗi vị trí.

    - Lưu ý: khi chỉ định các số trong “Payload Set” dropdown cho nhiều vị trí, hãy thực hiện theo thứ tự ***từ trên xuống dưới, từ trái sang phải***. Ví dụ: với 2 vị trí (***username=§pentester§&password=§Expl01ted§***), mục đầu tiên trong “Payload Set” dropdown sẽ đề cập đến trường username và mục thứ hai sẽ đề cập đến trường password.

- ***Payload settings***:

    - Phần này cung cấp các tùy chọn cụ thể cho loại payload đã chọn cho payload set hiện tại.

    - Mỗi payload type sẽ có bộ tùy chọn và chức năng riêng

![img](26)

- ***Payload Processing***:

    - Trong phần này, ta có thể xác định các quy tắc được áp dụng cho từng payload trong tập hợp trước khi nó được gửi đến target

    - Ví dụ, ta có thể viết hoa mọi word, bỏ qua payload phù hợp với regex pattern hoặc áp dụng các phép biến đổi hoặc lọc khác

- ***Payload Encoding***: 

    - Phần này cho phép tùy chỉnh các tùy chọn mã hóa cho payload.

    - Mặc định, Burp Suite áp dụng mã hóa URL để đảm bảo việc truyền payload an toàn. Tuy nhiên, có thể có trường hợp muốn điều chỉnh hành vi mã hóa.

Intruder cung cấp ***4 attack type***:

- ***Sniper***: tùy chọn mặc định và được sử dụng phổ biến nhất. Nó duyệt qua các payload, chèn từng payload một vào từng vị trí được xác định trong request. Các cuộc tấn công Sniper lặp đi lặp lại qua tất cả các payload theo ***kiểu tuyến tính***, cho phép thử nghiệm chính xác và tập trung.

- ***Battering ram***: Kiểu tấn công này khác với Sniper ở chỗ nó gửi tất cả payload cùng 1 lúc, mỗi payload được chèn vào vị trí tương ứng của nó. Kiểu tấn công này rất hữu ích khi kiểm tra các ***điều kiện tương tranh*** hoặc khi ***payload cần được gửi đồng thời***.

- ***Pitchfork***: Kiểu tấn công cho phép thử nghiệm đồng thời nhiều vị trí với payload khác nhau. Nó cho phép tester xác định nhiều payload sets, mỗi payload set được liên kết với một vị trí cụ thể trong request. Có hiệu quả khi có các tham số riêng biệt cần thử nghiệm riêng

- ***Cluster bomb***: Kiểu tấn công này kết hợp giữa cách tiếp cận Sniper và Pitchfork. Nó thực hiện 1 cuộc tấn công giống như Sniper trên từng vị trí, nhưng đồng thời kiểm tra tất cả payload từ mỗi set. Kiểu tấn công này hữu ích khi ***nhiều vị trí có payload khác nhau*** và muốn ***thử nghiệm tất cả cùng nhau***.

***Sniper*** là kiểu tấn công mặc định, được sử dụng phổ biến nhất trong Burp Suite Intruder. Nó đặc biệt hiệu quả với các cuộc tấn công vào 1 vị trí, ví dụ như ***password brute-force*** hoặc ***fuzzing for API endpoints***. Intruder sẽ ***chèn từng payload*** vào từng vị trí được xác định trong request.

Ví dụ POST request như sau: 

    POST /support/login/ HTTP/1.1
    Host: 10.10.159.252
    User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 37
    Origin: http://10.10.159.252
    Connection: close
    Referer: http://10.10.159.252/support/login/
    Upgrade-Insecure-Requests: 1

    username=§pentester§&password=§Expl01ted§

=> Trong ví dụ trên, có 2 vị trí được xác định cho body parameters: ***username*** và ***password***. Trong 1 cuộc tấn công Sniper, Intruder lấy từng payload từ payload set và thay thế nó lần lượt vào từng vị trí được xác định.

Giả sử có 1 wordlist gồm 3 từ làm payload: ***burp***, ***suite*** và ***intruder***. Intruder sẽ tạo ra 6 request:

| Request number | Request body                         |
|----------------|--------------------------------------|
| 1              | username=burp&password=Expl01ted     |
| 2              | username=suite&password=Expl01ted    |
| 3              | username=intruder&password=Expl01ted |
| 4              | username=pentester&password=burp     |
| 5              | username=pentester&password=suite    |
| 6              | username=pentester&password=intruder |

=> Sniper attack có lợi khi muốn thử nghiệm với các cuộc tấn công ở 1 vị trí, sử dụng các payloads khác nhau cho từng vị trí.

Kiểu tấn công ***Battering Ram*** trong Burp Suite Intruder khác với Sniper ở chỗ nó ***đặt cùng 1 payload ở mọi vị trí***, thay vì thay thế lần lượt từng payload vào từng vị trí.

Ví dụ request: 

    POST /support/login/ HTTP/1.1
    Host: 10.10.159.252
    User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 37
    Origin: http://10.10.159.252
    Connection: close
    Referer: http://10.10.159.252/support/login/
    Upgrade-Insecure-Requests: 1
    username=§pentester§&password=§Expl01ted§

Sử dụng ***Battering Ram attack*** với wordlist (burp, suite và intruder), Intruder sẽ tạo ra 3 request: 

| Request number | Request body                        |
|----------------|-------------------------------------|
| 1              | username=burp&password=burp         |
| 2              | username=suite&password=suite       |
| 3              | username=intruder&password=intruder |


Như được hiển thị trong bảng, mỗi payload từ wordlist được chèn vào mọi vị trí cho mỗi request được thực hiện.

=> Kiểu tấn công Battery Ram rất hữu ích khi muốn kiểm tra ***cùng 1 payload đối với nhiều vị trí*** cùng 1 lúc.

Kiểu tấn công ***Pitchfork*** trong Burp Suite Intruder tương tự như việc có nhiều Sniper attack chạy cùng lúc. Trong khi Sniper sử dụng 1 payload set để kiểm tra đồng thời tất cả các vị trí thì Pitchfork sử dụng 1 payload set cho mỗi vị trí (tối đa là 20) và lặp lại tất cả các vị trí đó cùng 1 lúc.

Để hiểu rõ hơn, xem lại ví dụ brute-force ở trên, nhưng lần này với 2 wordlist:

- Wordlist đầu tiên chứa usernames: joel, harriet và alex. 

- Wordlist thứ hai chứa passwords: J03I, Emma1815 và Sk1ll.

Sử dụng 2 wordlist này để thực hiện tấn công ***Pitchfork*** vào Login form:

| Request number | Request body                       |
|----------------|------------------------------------|
| 1              | username=joel&password=J031        |
| 2              | username=harriet&password=Emma1815 |
| 3              | username=alex&password=Skill       |

Như được hiển thị trong table trên, Pitchfork lấy mục đầu tiên từ mỗi wordlist và thay thế chúng vào request, mỗi mục 1 vị trí. Sau đó, nó lặp lại quy trình này cho request tiếp theo bằng cách lấy mục thứ hai từ mỗi wordlist và thay thế nó. Intruder tiếp tục việc lặp lại này cho đến khi 1 hoặc tất cả các wordlist hết mục. Điều quan trọng cần lưu ý là Intruder ***ngừng test*** ngay khi 1 trong wordlists hoàn tất. Do đó, trong Pitchfork attack, các payload set có cùng độ dài là điều lý tưởng.

Kiểu tấn công ***Cluster Bomb*** trong Burp Suite Intruder cho phép chọn nhiều payload sets, mỗi bộ cho mỗi vị trí (tối đa là 20). Không giống như Pitchfork, trong đó tất cả payload sets sẽ được kiểm tra đồng thời. Cluster Bomb lặp lại qua từng payload sets riêng lẻ, đảm bảo mọi tổ hợp payload có thể đều được kiểm tra.

Như Task 8, sử dụng 2 wordlists:

- Usernames: joel, harriet và alex
- Passwords: J03l, Emma1815 và Sk1ll

Trong vd này, giả sử  ***không biết pass nào thuộc về user nào***. Ta có 3 username và 3 password, nhưng không xác định được ánh xạ. Trong trường hợp này, có thể sử dụng Cluster Bomb attack để thử mọi cách kết hợp các giá trị. 

| Request number | Request body                       |
|----------------|------------------------------------|
| 1              | username=joel&password=J031        |
| 2              | username=harriet&password=J031     |
| 3              | username=alex&password=J031        |
| 4              | username=joe&password=Emma1815     |
| 5              | username=harriet&password=Emma1815 |
| 6              | username=alex&password=Emma1815    |
| 7              | username=joe&password=Sk1ll        |
| 8              | username=harriet&password=Sk1ll    |
| 9              | username=alex&password=Sk1ll       |

=> Cluster Bomb attack đặc biệt hữu ích cho các tình huống ***credential brute-forcing***, trong đó không xác định được ánh xạ giữa usernames và passwords.

***Practical Example***

Tải xuống wordlist: 

![img](27)

Intercept request: 

![img](28)

![img](29)

Chuyển sang tab Payload: 

![img](30)

Với payload set tương ứng thì load wordlist tương ứng:

- payload set 1 tương ứng với wordlist username.txt
- payload set 2 tương ứng với wordlist password.txt

Click Start attack: 

![img](31)

=> Vì Burp đã gửi 100 request, cần xác định request nào thành công.

=> Vì tất cả Response status code không phân biệt các lần thử thành công hay thất bại (đều là 302), cần sử dụng ***Length*** để phân biệt.

![img](32)

=> Nhấp vào Length để sắp kết quả theo Length, tìm request có response length ngắn hơn, cho biết nỗ lực đăng nhập thành công

![img](33)

=> Thử đăng nhập với username: ***m.rivera*** và pass là ***letmein1***.

![img](34)

=> Login thành công

Khi login thành công, click vào 1 ticket để xem đầy đủ ticket, phát hiện ra URL có dạng:
http://10.10.159.252/support/ticket/NUMBER

=> Vé được gán số ID số nguyên thay vì ID phức tạp khó đoán

=> 2 tình huống có thể xảy ra:

- ***Access Control***: endpoint có thể được config đúng cách để hạn chế quyền access chỉ vào các ticket được chỉ định cho user hiện tại. Trong TH này, chỉ có thể xem đc ticket đc liên kết với account này

- ***IDOR Vulnerability***: ngoài ra, endpoint có thể thiếu các biện pháp kiểm soát truy cập thích hợp, dẫn đến lỗ hổng được gọi là Insecure Direct Object References (IDOR). Nếu đúng, có thể khai thác hệ thống và đọc tất cả request hiện có, bất kể user là ai.

=> Sử dụng Intruder để fuzzing endpoint ***/support/ticket/NUMBER***. 

=> Điều chỉnh position chỉ test ID vé:

![img](35)

Điều chỉnh payload type thành ***numbers*** trong khoảng ***1 - 100***, step là 1:

![img](36)

=> Start attack:

![img](37)

=> Tìm các request có status code là 200 và xem response…

![img](38)

***Extra Mile Challenge***

Thử intercept và send request http://10.10.159.252/admin/login/

=> Response như sau:

![img](39)

Trong response, nhận thấy bên cạnh các trường username và password, hiện có 1 bộ Session cookie cũng như CSRF (Cross-Site Request Forgery) token ở dạng field ẩn. Refresh page và thấy cả session cookie và loginToken thay đổi theo từng lần refresh.

=> Mỗi lần login, cần trích xuất giá trị hợp lệ cho cả session cookie và loginToken.

![img](40)

Để làm, sử dụng ***Burp Macros*** để xác định 1 tập hợp hành động (macro) lặp lại sẽ được thực thi trước mỗi request. Macro này sẽ trích xuất các giá trị duy nhất cho session cookie và loginToken, thay thế chúng trong mọi request tiếp theo.

![img](41)

=> Chọn 2 position là username và password

=> sang tab payload và load username.txt và password.txt tương ứng

Với username và password parameters được xử lý, bây giờ ta cần tìm cách lấy loginToken và cookie phiên luôn thay đổi. Không may, “recursive grep” không hoạt động ở đây do redirect response, không thể thực hiện hoàn toàn trong Intruder

=> xây dựng 1 macro

Macro cho phép thực hiện lặp đi lặp lại cùng 1 nhóm hành động. Trong TH này, ta chỉ muốn gửi GET request tới /admin/login/






 
















