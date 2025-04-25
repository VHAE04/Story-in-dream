# Lưu ý tất cả nội dưới đây chỉ là ảo tưởng hoá chỉ có mục đích học tập


# 🎯 Xác Suất Trong và AI trong Exploit: Từ Một lỗ hổng nhỏ `.git` đến 7 RCE.

## ☕ Một lỗ hổng nhỏ `.git`

Tôi vốn làm bảo mật  – kiểu check lỗ hổng định kỳ cho mấy bên. Một hôm trong lúc kiểm tra một site cũ, tôi thử path quen thuộc:

`/.git 


Boom – **lộ toàn bộ**. Source code, cấu hình, đường dẫn production.

![image](https://raw.githubusercontent.com/VHAE04/Story-in-dream/refs/heads/main/Lỗ%20hổng%20nhỏ%20.git/Pasted%20image%2020250424233340.png)

Lỗ hổng .git/HEAD là một trong những lỗi bảo mật phổ biến trên các website sử dụng Git mà không cấu hình bảo mật đúng, và có thể dẫn đến rò rỉ toàn bộ mã nguồn của dự án. Đây không phải là "lỗi của Git", mà là lỗi do cấu hình server không đúng.

Nếu thư mục .git bị để lộ trên server (tức là người truy cập có thể vào đường dẫn như: http://example.com/.git/HEAD), thì:

Hacker có thể biết repo đang dùng nhánh nào.

Tệ hơn: Hacker có thể lần theo .git/ để tải về toàn bộ lịch sử commit, source code, cấu hình server, các file .env, mật khẩu, API key, database config... 🤯



Tôi nghĩ bụng:

> “Giờ này rồi còn dính lỗi này? Nếu 1 trang bị, chắc chắn còn nhiều trang nữa.”

Thế là tôi bật terminal, lấy list domain cũ, thêm vài tool crawling, gom lại được cỡ **~>300k domain**.

## 🛠️ Git checker và cốc cà phê đắng

Tôi viết script nhỏ: chỉ cần gửi request kiểm tra tồn tại `.git/HEAD` ( hoặc bạn có thể dụng nuclei )

Chạy script xong, tôi đi làm cốc cà phê. 

Khi quay lại, terminal báo:

> **132 domain dính `.git`**

![image](https://raw.githubusercontent.com/VHAE04/Story-in-dream/refs/heads/main/Lỗ%20hổng%20nhỏ%20.git/Pasted%20image%2020250424235132.png)

Thế là tôi bật tool git-dumper, bắt đầu dump source. Và như đúng quy trình – tôi đi úp bát mì tôm.

## 🍜 Mì tôm và 77 bộ source

Khi quay lại sau bữa tối gọn nhẹ:

> **77 site đã dump được full source code**

![image](https://raw.githubusercontent.com/VHAE04/Story-in-dream/refs/heads/main/Lỗ%20hổng%20nhỏ%20.git/Pasted%20image%2020250424234836.png)

Có site dump ra cả cấu hình production, database, SMTP – nhưng không phải cái nào cũng khai thác được vì không có lối vào cho các config đấy.

## 🤖 AI thủ công – tự build, tự chạy

Tôi quyết định import toàn bộ đống source vào một con AI nhỏ mình từng build. Tôi train lại để nó tìm ra:

- File có chức năng **upload** mà không cần login
    
- Các đoạn code chứa hàm nguy hiểm như `eval`, `system`, `exec`
    
- Trường hợp path traversal qua biến `$path`, `$file`, `$url`
    

Con model khá nặng, và chạy lâu, tôi liền ra công viên suy nghĩ về cdoi.

Về tới nhà, tôi thấy terminal báo kết quả:


- Gần **10 web có chức năng upload không xác thực**
    
![image](https://raw.githubusercontent.com/VHAE04/Story-in-dream/refs/heads/main/Lỗ%20hổng%20nhỏ%20.git/Pasted%20image%2020250424234424.png)

Trong đó, **7 site tôi đã thử khai thác và có thể RCE**


> **Shell nổ. Lệnh thực thi.**

![image](https://raw.githubusercontent.com/VHAE04/Story-in-dream/refs/heads/main/Lỗ%20hổng%20nhỏ%20.git/Pasted%20image%2020250424234548.png)

## 📊 Kết luận xác suất 

Tôi scan 330K domain → 132 tồn tại flie `.git/HEAD` → 77 dump full source → 10 web suspect → 7 RCE.

Xác suất nhỏ? Ừ. Nhưng với **scale đủ lớn**, nhỏ cũng thành to.

- `.git` là lỗi ai cũng biết, nhưng vẫn còn tồn tại.
    
- AI không thay thế người – nhưng giúp bạn tiết kiệm **hàng giờ đọc code**.
    
- Xác suất trong exploit không phải là công thức khô khan – mà là **chiến lược**.
    

# Keyword

- nuclei
- .git/HEAD ( is vuln web )
- xác xuất thống kê
- scale
- file upload inject
- AI sẽ là bạn nếu ta biết tận dụng

> **Tấn công không cần phải 0day.  
> Chỉ cần bạn chịu để AI làm phần chán nhất.**
