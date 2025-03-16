
## Lưu ý tất cả nội dưới đây chỉ là ảo tưởng hoá chỉ có mục đích học tập

### Chương 1 break the limit 

hm xin chào lại là tôi XTAE đây  vẫn như thường ngày tôi vào quán net để nghiên cứu vài tài liệu, tất nhiên là tôi không chơi game nhưng giá thành ở đây khá là đắt vì nó là nét mà, luôn được trang bị những con card chiến binh để các game thủ có thể gank những trận game su cà na mặc dù tôi chả bao giờ dùng đến card. Vì thế tôi muốn tìm cách khác giảm giá thành học tập của mình. Quay lại ngày hồi còn ở làng, các quán net của chỗ tôi khá cổ, tôi có thể dùng usb để boot hệ điều hành ưu thích của mình lên mà không bị tính phí enjoy. Nhưng các hệ thống máy hiện nay thường chặn người dùng boot HDH lên.
![[https://raw.githubusercontent.com/VHAE04/Story-in-dream/refs/heads/main/Câu%20truyện%20quán%20net/Pasted%20image%Pasted image 20250316205012.png]]
 Lượn lờ trên internet tôi biết được có những cách như là đóng băng hệ thống tính tiền của hệ thống quản lý để tài khoản của tôi không bị trừ hao thời gian dùng nhưng nó chỉ hoạt động với các hệ thống cũ, với các hệ thống quản lý mới các máy chủ sẽ kiểm tra các tiến trình ứng dụng quản lý nếu các tiến trình không hoạt động theo thời gian thực, máy chủ sẽ cho client tự động reset lại máy.
![[Pasted image 20250316204442.png]]
Với một một tư duy nguyên mẫu ta biết hệ thống quản lý sẽ gửi lệnh reset đến máy người dùng qua đường truyền mạng. Vậy làm sao để máy mình không bị reset, bạn đã đoán đúng ta chỉ tắt toàn bộ hệ thống quản lý + tắt đường truyền mạng vậy là ta có thể dùng máy tính một cách free.
Ơ vậy tắt đường truyền mạng thì bạn dùng nó kiểu gì. À đúng bạn còn nhớ cách mình boot hệ điều hành ưa thích của mình ở trên không. Hầu hết mọi quán net hiện nay đều có một hoặc vài cổng usb để người dùng có thể copy game, ứng dụng hoặc đơn giản chỉ là cắm bạn phím gaming của họ dùng. Lợi dụng điều đó ta có thể cắm adapter wifi của chúng ta vào để có thể kết nối wifi miễn phí của quán và dùng internet bình thường, mặc dù điều đó ảnh hưởng tới độ trễ của mạng nhưng với một người đi research như tôi thì điều đó cũng không quan trọng lắm.
![[Pasted image 20250316210315.png]]

### Chương 2 Only one node woking is needed

Sau một người gian ngồi khá nghiên cứu khá là chán, lúc rảnh rỗi tôi lại xem xét hệ thống này có lỗ hổng gì không. Vì hệ thống quán net thường cố gắng tối ưu hiệu năng cho người chơi game nên các bản hdh khá là cũ và thường không có AV hoặc EDR vì hầu hết các quán có các game crack , ngồi một lúc một đã tìm thấy một lỗ hổng nghiêm trọng SMBGhost ở chiếc máy tôi đang sử dụng nó cho phép RCE hệ thống. Và đúng như tôi dự đoán tất cả các máy ở đây đều có hệ điều hành giống nhau nghĩa là toàn bộ hệ thống đều bị dính SMBGhost. Điều này cho phép tôi có thể điều khiển bất kỳ máy tính nào và có thể cài các phần mềm độc lại vào đó.
![[Pasted image 20250316211810.png]]

Điều tôi lo nhất là tôi phải hiện diện ở quán net và tôi không muốn bị ăn đập khi tấn công vào một máy tính nào đó. Vậy làm sao để tôi có thể điều khiển từ một nơi nào đó để không phải ăn đập. Đơn giản vậy tôi chỉ cần tấn công vào các máy và thực hiện tạo tunnel về một hệ thống của tôi là xong.
![[Pasted image 20250316212631.png]]
Điều đang nói ở đây là sẽ chả có ai ở net cả ngày cả vì vậy các pc sẽ bị tắt và hệ thống sẽ boot lại hoàn toàn cho người khác, điều này đồng nghĩa các kết nối của tôi sẽ bị tắt theo thời gian.
![[Pasted image 20250316212804.png]]
Vậy làm sao để các máy bị tắt vẫn có thể kết nối đến tunnel của tôi. Đúng như tiêu đề chương "Only one node woking is needed" chỉ cần một node hoạt động tôi sẽ code một đoạn mã để tự động scan toàn bộ thiết bị trong dải mạng, kiểm tra các node máy up và tự động khai thác các máy chỉ định về kéo kết nối về lại tunnel. Vậy các máy khi bị nhiễm mã khai thác sẽ tự động đóng vai trò một node máy chủ lây nhiễm. Cho dù các máy node tắt đi và bật lại các node hoạt động còn lại sẽ tự động khai thác và lại biến nó thành một node của hệ thống.
Node 1 Thực hiện truyền mã khai thác cho node 2
Node 2 Sau khi nhận mã khai thác sẽ thực hiện mở port xác nhận, quét bộ toàn bộ mạng lan và xác nhận các máy đang hoạt động --> thực hiện rà quét port xác nhận ( kiểm tra máy đã tồn tại node chưa) --> nếu máy chưa mở port xác nhận thực hiện khai thác máy đó và biến máy đó vào một node của hệ thống.
Node 3, Node 4 tương tự
....( với một hệ thống thật thì số node khá là nhiều tầm hơn chục hoặc đến trăm từ đó với một hệ thống quán lý node như này tôi khá chắc là hệ thống của tôi không bị dán đoạn vì tôi thấy quán bán được khá nhiều combo đêm :D )
![[Pasted image 20250316213412.png]]

# Chương 3 Original path

Từ các hệ thống node cũ tôi tiên hành đào sâu vào hệ thống mạng tôi phát hiện ra các dải mạng của hệ thống server, hệ thống cân bằng tải,...
Tôi nhận ra server còn sử dụng một hdh còn cũ hơn cả các máy tính quản lý, từ đó lý dải tại sao các node của tôi không thể rà soát được đến nó.
Sau khi truy cập đường vào máy chủ tôi đã có thể có toàn quyền quản trị hệ thống. Thêm sửa xoá tài khoản người dùng tăng thời gian chơi bất kỳ, quản lý boot từ đó tôi có thể đính kèm mã của tôi trực tiếp vào boot của hệ thống.

![[Pasted image 20250316215447.png]]

Quyết định cuối cùng. 
Sau một thời gian khá là nghịch ngợm tôi quyết định báo cáo toàn bộ cho người quản lý quán. Dù giải thích gặp rất nhiều cho họ hiểu vể chuyên môn kỹ thuật nhưng dù sao họ cũng đánh giá được mức độ nghiêm trọng của nó. Và tôi đã được một tài khoản vjp của quán và chơi không giới hạn thời gian ^^. Cảm ơn quán.
