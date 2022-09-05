Nhóm IR của chúng tôi đã lấy đĩa của điện thoại Android. Phân tích kết xuất và trả lời các câu hỏi được cung cấp.
Tools
[DB Browser for SQLite](https://sqlitebrowser.org/)
[Epochconverter](https://www.epochconverter.com/)
Detail chall: [Here](https://cyberdefenders.org/blueteam-ctf-challenges/69)

 Tôi đã vượt qua thử thách này khoảng 60%, bởi vì một số câu hỏi khá là lạ với một người mới tìm hiểu Forensic Mobile. Toàn bộ chall làn này có 8 câu hỏi và vượt qua thử thách khi bạn hoàn thành tất cả các câu hỏi. Sau đây là cách mà tôi đã vượt qua thử thách( Những chia sẻ này là của cá nhân tôi, các bạn có thể tham khảo và góp ý)

- File nén sau khi tải về và tôi đã unzip ra thành các folder để dễ dàng trong quá trình điều tra.

![image](https://user-images.githubusercontent.com/42565778/188411285-5895f8d4-85bd-45ef-908e-03216a06b725.png)

- Chúng ta hãy cùng reveiw 1 lượt nội dung trong các folder có những gì

![image](https://user-images.githubusercontent.com/42565778/188412448-fec209fe-67e7-476e-8f22-b395aef0d2a2.png)
   + Agent Data: chứa các thông tin liên quan đến DB của phone: thông tin wifi, thông tin danh bạ người dùng, thông tin lịch trình người dùng.
   + Apps: chứa các thông tin các folder của từng phần mềm người dùng cài đặt, hay các phần mềm thực thi của hệ thống
   + Live Data: chứa các thông tin dữ liệu được hệ thống lưu lại tại từ thời điểm hoạt động cho tới thời điểm ghi nhận( Chỉ số này có thể mất khi máy bị khôi phục cài đặt gốc - theo mình nghĩ là thế).
   + Sdcard và share: là 2 folder được ghi lại thông tin từ folder "Tệp" hay "Quản lý bộ nhớ" trên máy nạn nhân.
Và bây giờ chúng ta sẽ đi vào giải các chall này nhé

**1. What is the email address of Zoe Washburne?**
- Zoe Washburne là tên của một người và tôi đang suy nghĩ rằng không biết anh ta là chủ sở hữu hay là một tên của người dùng trong danh bạ hoặc anh ấy là người tạo cuộc hẹn với nạn nhân.
=> chính vì thế thôi đã truy cập vào thư mục Agent và kiểm tra các thuộc tính có liên quan tới email.
- Sử dụng Db Browser để kiểm tra các thông tin, sau một hồi tìm kiếm và tôi đã tìm thấy thông tin của anh ta trong file "contacts"
- 
![image](https://user-images.githubusercontent.com/42565778/188414973-9bea1615-341f-4b64-930a-3c21f2b2a9c0.png)

_Submit: zoe........_

**2. What was the device time in UTC at the time of acquisition? (hh:mm:ss)**
- Câu hỏi này có vẻ liên quan tới một phần dữ liệu đang hiện hoạt được lưu trữ trên máy, tôi nghĩ rằng nó sẽ có trong folder "live data".
![image](https://user-images.githubusercontent.com/42565778/188416079-9adf56ee-76ea-4790-8a2d-12938baba2e6.png)

_submit: 18:...._

**3. What time was Tor Browser downloaded in UTC? (hh:mm:ss)**
- Câu hỏi này, tôi nghĩ rằng mình có thể tìm file apk(vì đây là  app được tải trên mobile) ở trong thư mục dowload và chỉ cần kiểm tra một số thông tin qua exiftool hoặc properties.
- 
![image](https://user-images.githubusercontent.com/42565778/188417150-3a0cd1cc-fb70-4514-b2b9-9ee06c01928a.png)

Cuối cùng tôi chỉ cần lấy thời gian Modified - 7 ra thời gian  cần tìm (bởi vì hệ thống của tôi đang lưu theo giờ VN có nghĩa là UTC +7). Kết quả là 19:42:26

![image](https://user-images.githubusercontent.com/42565778/188417469-f8e2b8f0-adbf-48b8-8407-6d61cdf017cd.png)
 
 - Kết quả hoàn toàn đúng, nhưng tôi nghĩ cách thu thập thông tin này hoàn toàn đang có vấn đề. Trong một số sự cố mà tôi đã điều tra việc fake thời gian hay time storm hoàn toàn có thể được. Việc submit đúng kết quả chỉ là một các ăn may và tôi phải đi tìm cách khác tổng quan hơn và có thể đúng trong mọi trường hợp. Tôi ưu tiên việc kiểm tra dấu thời gian trong database và quay lại với thư mục Agent để truy vấn file "Downloads.db".

![image](https://user-images.githubusercontent.com/42565778/188418206-d948cfa5-135c-468c-ba81-0fc98235a88b.png)

- Tôi đã tìm được thời gian đang được lưu dưới dạng unix epoch và chúng ta cần một tool để decode nó. Kết quả thu được đúng hoàn toàn khớp với đáp án.

![image](https://user-images.githubusercontent.com/42565778/188418982-6625c50e-ec64-4739-b2bb-9c19207db6bd.png)

 **4. What time did the phone charge to 100% after the last reset? (hh:mm:ss)**
- Câu hỏi này khá là khó và tôi đã tìm tới file chứa các thông tin của máy được lưu lại folder "Live data" nhưng một cách non nớt và tôi không tìm được key word đúng trong quá trình này. Key word cần lưu ý đó là "status" - bởi vì nó đang hỏi trạng thái pin, tôi đã thử tìm "battery.txt" hoặc "batterystats.txt". Theo gọi ý tôi đã tìm thông tin hiển thị "status=full"
- 
![image](https://user-images.githubusercontent.com/42565778/188420313-baae2cd4-adbc-4fb7-b3e6-c1e57c2d046a.png)

- thời gian được lưu reset là: "RESET:TIME: 2021-05-21-13-12-19" có nghĩa là khoảng 13h12m19s cho tới khi thiết bị báo đầy là khoảng "5m01s459ms (3) 100 status=full charge=2665", và khoảng sau 5m1s thì thiết bị này được sạc đầy(tôi chưa hiểu cơ chế reset time ở đây để làm gì @@!!). Tổng thời gian sạc tính từ lúc reset là 13:12:19 + 0:05:01 = 13:17:20

_submit: 13:17:..._
