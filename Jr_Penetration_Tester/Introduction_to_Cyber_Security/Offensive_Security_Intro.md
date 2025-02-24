# Offensive Security Intro

**Task 1: What is Offensive Security?**

*“Để đánh bại 1 hacker, bạn cần suy nghĩ như 1 hacker”*

**Task 2: Hacking your first machine**

Một ứng dụng web ngân hàng giả có tên Fakebank, có thể hack 1 cách an toàn:

Truy cập http://10.10.50.219

![img](0)

Sử dụng tool ***gobuster*** để tìm kiếm thư mục, trang ẩn trên website FakeBank:

![img](1)

=> Truy cập endpoint ***/bank-transfer***:

![img](2)

Chuyển 2000$ từ bank account 2276 sang bank account của tin tặc (account số 8881):

![img](3)

Nếu thành công, số dư mới sẽ được hiển thị:

![img](4)





