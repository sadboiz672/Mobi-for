Jailbroken là một cuộc điều tra trường hợp iPad cho thấy các khía cạnh khác nhau của hệ thống IOS.

Tool(Khuyến nghị sử dụng theo cá nhân mình):

- [Autopsy](https://www.sleuthkit.org/autopsy/)
- [SqiliteDB Browser](https://sqlitebrowser.org/)
- [Epochconverter](https://www.epochconverter.com/)

 Detail chall: [Here](https://cyberdefenders.org/blueteam-ctf-challenges/35)
 
 Tôi đã vượt qua thử thách này với khoảng hơn 90%, và bài lab này là bài thứ 2 trong chương trình nghiên cứu về Forensic Mobile. Toàn bộ chall này có 17 câu hỏi xoay quanh chiếc Ipad đã Jaibreak và nhiệm vụ của tôi hoàn thành khi tôi giải được hết các thử thách trên. Sau đây là cách giải của riêng cá nhân tôi, các bạn xem và góp ý giúp tồi nhé!!!

- File sau khi giải nén ra sẽ nhận được nội dung như thế này, các bạn có thể sha1sum hoặc md5sum và lấy kết quả đó kiểm tra tính toàn vẹn với thông số được đưa ra trên trang chủ vì dung lượng của file này khá lớn khoảng hơn 3.3GB.

 ![image](https://user-images.githubusercontent.com/42565778/189091033-98871ce8-0822-46be-80a2-cd0b50854984.png)
 
 - Nếu các bạn mới tìm hiểu như mình thì có lẽ sẽ thấy cấu trúc tập lệnh này có gì đó khá là phúc tạp hơn cả cấu trúc của Android mà bài trước tôi đã làm. Chính vì thế chúng ta để làm được các bài tập dạng này cần phải tìm hiểu cấu trúc của nó thì mới có thể rút ngắn thời gian thu thập thông tin(tham khảo [tại đây](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html), hoặc một cách ngắn gọn hơn [tại đây](https://github.com/sadboiz672/Mobile_Forensic/blob/main/IOS/IOS-file-system.pdf), hoặc tới câu hỏi này nếu bạn khoogn biết làm có thể đặt những câu hỏi với tiền tố: "what is ... IOS forensic?" và nó sẽ tìm kiếm thông tin mà bạn cần.

- Trong quá trình làm mình mất khoảng thời gian đầu sử dụng các command: grep, find, tree... để tìm các file và nội dung vì tệp lệnh rất nhiều, nếu mò thì rất là lâu với người chưa thật sự hiểu sâu về cấu trúc file IOS như mình. Theo mình thì các bạn có thể sử dụng Autopsy để tìm kiếm thông tin, một con tool khá mạnh để thực hiện forensic. Nếu bạn chưa biết các add file có thể tham khảo:

![image](https://user-images.githubusercontent.com/42565778/189094253-f294918d-6880-470d-a8c6-ee3ef7fff7d0.png)
 
 khi thành công cấu các bạn sẽ nhận kết quả như này:
 
 ![image](https://user-images.githubusercontent.com/42565778/189094426-e80a69ed-2125-4a2b-9364-fd8413dc4d25.png)
 
 Và bây giờ thì cùng đi giải các chall thôi nào....
 
 - Đầu tiên chúng ta sẽ cùng lọc hết các fild db có trong folder đã tải về, và chắc hẳn sẽ cần thiết cho các câu hỏi liên quan đến số lượng app, phần trăm pin hoặc là thời gian app hoạt động,... truy vấn với cmd: find `-name *.db`
 
 ![image](https://user-images.githubusercontent.com/42565778/189096862-570fd84d-3729-4d22-b239-c218af1e71d0.png)


 
**1. What is the IOS version of this device?**

- Mình khá mất thời gian trong việc grep -r IOS hay là grep -r Version,... nhưng mà tất cả các kết quả đó hoàn toàn là thông tin mà mình mong muốn nên các bạn đừng thử mò như mình mà nên tìm hiểu :))
- Dựa vào tệp tài liệu mà mình tìm được, có đề cập đến một số chức năng của db đối với việc lưu trữ:

 ![image](https://user-images.githubusercontent.com/42565778/189097307-bf501ba5-9951-499c-8cbd-58f9987a8989.png)

- Chính vì thế mình sẽ tìm file có tên `General.log`

![image](https://user-images.githubusercontent.com/42565778/189098048-ed41eef9-df7c-4e68-8594-193f1c983401.png)


`Submit: 9.3....`

##**2. Who is using the iPad? Include their first and last name. (Two words)**

- Đối với câu hỏi này, mình nghĩ là sẽ có thông tin trong db và ban đầu câu này mình đã bỏ qua để tìm kiếm thông tin ở câu khác và vô tình thông tin của người sử dụng hoàn toàn có trong db. Tôi đã phát hiện ra người sở hữu thông qua việc check xem anh ta đã lên lịch những cuộc hẹn .....

![image](https://user-images.githubusercontent.com/42565778/189102789-cc02ba01-fabc-45c4-857c-c0c849f36b22.png)

`submit: Tim....`

##**3. When was the last time this device was 100% charged? Format: 01/01/2000 01:01:01 PM**

- Với câu hỏi này, rút kinh nghiệm từ các chall trước thì để tìm thông tin pin chúng ta cần phải tìm các db lưu trữ thông tin của bin với keyword như là: battery, power,... và cuối cùng một trong số đó ở đường dẫn là: `/private/var/containers/Shared/SystemGroup/4212B332-3DD8-449B-81B8-DBB62BCD3423/Library/BatteryLife/` 

![image](https://user-images.githubusercontent.com/42565778/189594108-28ec2667-8e7f-4f5e-8017-36b9380cd513.png)

và kết quả theo đáp án là: `1586976031.21529`

![image](https://user-images.githubusercontent.com/42565778/189594283-0e9636df-2222-4a65-9ba4-9558caa6bd69.png)

- Nhưng theo mình thì đây chưa phải là kết quả chính xác bởi vì dựa vào Rawlevel thì tại thời điểm của đáp án fullbattery mới là: 100.056364490371 thực tế nó cao hơn và là: 100.0657585721. Chính vì thế tác giả nên xem lại đáp án cho bài này.

`_submit: 04/15/202_`


##**4. What is the title of the webpage that was viewed the most? (Three words)**

- Có rất nhiều cách để làm câu này, với mình khi không sử dụng autopsy thì mình check history.db và kết quả thu được:

![image](https://user-images.githubusercontent.com/42565778/189099256-78b463d6-592a-4500-9bd8-e9df89cb56bd.png)

- Hoặc các bạn tận dụng plugin có sẵn ở autopsy để lọc thông tin trong webhistory:
 
 ![image](https://user-images.githubusercontent.com/42565778/189099424-34a16676-68c1-4ff7-af05-575b599c2075.png)


`submit: kirby with.....`


##**5. What is the title of the first podcast that was downloaded?**

- Đây là câu hỏi có liên quan tới các câu còn lại. Vì podcast này theo thông tin đã được tải về máy, cho nên để tìm thông tin thư mục mình sử dụng `grep -r podcast` để tìm kiếm file, sau cả quá trình lọc thì tìm thư mục podcast và chứa các video. Cùng truy cập và xem thông tin chi tiết.

![image](https://user-images.githubusercontent.com/42565778/189562093-6a8969b8-86cd-4fee-9725-20a225fae777.png)

- Ở đây có 2 file mp3 và 2 hình ảnh dạng thumbnail và để biết podcast nào được tải về trước thì xem dấu thời gian lưu trữ, và kết quả cuối cùng là...

![image](https://user-images.githubusercontent.com/42565778/189562228-5643adb1-c716-4fca-86db-f3ee42a6d371.png)

 `_submit: WHERE..._`

##**6. What is the name of the WiFi network this device connected to? (Two words)**

- Trong câu hỏi này mình có sử dụng hints nhỏ và có gợi ý là tra cứu cách  "forensic ios wifi" mình đã làm thử và tìm được câu trả lời..

![image](https://user-images.githubusercontent.com/42565778/189563668-8f6e3ed3-3eca-429d-a3f1-c0c168ad9c43.png)

- Giờ chỉ bằng 1 vài truy vấn để tìm kiếm xem thư mục đó ở đâu, mình sử dụng strings và grep để tìm kiếm các thông tin tên wifi nhưng khá là lạ khi ở đây nó lại không lưu thông tin là SSID mà lưu theo format MAC:wifi. Dù sao thì vẫn tìm được kết quả cuối cùng....

![image](https://user-images.githubusercontent.com/42565778/189563767-d58abd0c-ac59-41bd-a59e-2139e82c0961.png)

`_submit: black...._`

##**7. What is the name of the skin/color scheme used for the game emulator? This should be a filename.**

- Câu hỏi này mình hơi thắc mắc về việc tìm file name, và đã sử dụng đến hints thứ 2 trong câu hỏi này.

![image](https://user-images.githubusercontent.com/42565778/189564587-b0797adc-65ef-4b31-b496-2158bd62f375.png)

`_submit: Def...._`


##**8. How long did the News App run in the background?**

- Theo kêt quả tìm kiếm db liên quan tới battery ở câu trên nên mình không còn mất nhiều thời gian ở đây nữa:

![image](https://user-images.githubusercontent.com/42565778/189595321-1c0f72aa-6cb8-470c-b357-d5e67df06b95.png)

`_submit:197.8_`

##**9. What was the first app download from AppStore? (Two words)**

- Câu hỏi này mình cũng submit hên xui, có 1 dữ kiện là ứng dụng được cài đặt từ appstore nhưng mình đã lục lọi hết các tệp thực thi, các tệp được cài thêm và quay lại dùng autopsy để lọc metadata theo kích thước 

![image](https://user-images.githubusercontent.com/42565778/189596831-d7c98fdb-220d-4da3-84cf-f1ba51498f4e.png)

`_submit: Cooki...._`

##**10. What app was used to jailbreak this device?**

- Mình kiểm tra các app đã được cài đặt ở trong máy điện thoại, vì mình không dùng IOS và cũng không rõ các công cụ jaibreak là gì nên khi gặp app nào lạ thì mình để search ....

![image](https://user-images.githubusercontent.com/42565778/189566514-5ed325be-8e43-4a71-ad10-d9c10e2b8bd2.png)

`_submit: pho...._`

##**11. How many applications were installed from the app store?**


![image](https://user-images.githubusercontent.com/42565778/189565663-43783b15-d8fb-4e81-97cf-a68f8b79be13.png)

##**12. How many save states were made for the emulator game that was most recently obtained?**



##**13. What language is the user trying to learn?**

- Theo tìm kiếm từ podcast ở câu hỏi trên và mình đã tìm được thông tin từ podcast thứ 2, ngôn ngữ đang được học ở đây là...

![image](https://user-images.githubusercontent.com/42565778/189563085-2e734bf2-b89c-4bcb-9979-0ccfbfb46eb3.png)


![image](https://user-images.githubusercontent.com/42565778/189566797-46dca437-81fd-42aa-b137-04f3b7b35365.png)


`_submit: Span...._`

##**14. The user was reading a book in real life but used their IPad to record the page that they had left off on. What number was it?**

- Trong quá trình tìm kiếm, mình kiểm tra khá nhiều tệp tin rồi dữ liệu từ note hoặc video hoặc nhiều thông tin khác có thể khai thác được. Và cuối cùng mình mới để ý tới hints từ câu hỏi để khoanh vùng "record" và mình dùng chức năng lọc metadata từ autopsy để kiểm tra các thông tin có dạng wav, mp4,....

![image](https://user-images.githubusercontent.com/42565778/189562939-f9b0ab35-c0ba-43ec-a1ec-45dc4a2c9134.png)

`_submit: 85_`

##**15. If you found me, what should I buy?**
- Câu hỏi này khá là khó hiểu và tôi cần phải dùng hint để thực hiện thử thách, nhưng có vẻ hint cũng không giúp ích gì cho tôi khi mà tôi loay hoay tìm thông tin được lưu trong Note, thông tin được lưu trong lập lịch... và phải dùng hints tiếp theo...

 ![image](https://user-images.githubusercontent.com/42565778/189104541-ff828920-2e86-4670-a322-26639d2cf8a0.png)

- Và nó là note thay vì notes :)) wtf một sự bất cản của chính bản thân tôi cho nên đã bị trừ điểm vô ích haha :))) Câu chuyện về sự bất cẩn cũng không hề dừng lại trong khi có quá nhiều bảng trong db và tôi không chịu lướt cho hết thông tin các bảng :)) một cách vô ích và mất thời gian phải cẩn thận check lại từng bảng một và không để xót thông tin nào.

![image](https://user-images.githubusercontent.com/42565778/189105198-5012bc22-033e-47b9-9a1d-edbcb0dbb1e3.png)

- Nhưng lại có điều đặc biệt đây là dòng note không hề hoàn chỉnh và tôi cần phải gg xem nó là gì và dự đoán phần còn lại của flag.... theo như dự đoán của tôi từ còn thiếu cần phải điền là: 

![image](https://user-images.githubusercontent.com/42565778/189105479-67047aef-b0ab-4d2b-9351-b4517512aa65.png)

`submit: Crash......`

##**16. There was an SMS app on this device's dock. Provide the name in bundle format: com.provider.appname**

- Với câu hỏi này mình nghĩ ngay tới việt check các thông tin các gói cài đặt có trong Application...db điều này hoàn toàn có thể và theo yêu càu đề bài về việc app liên quan đến quản lý SMS chính vì thế sẽ lọc các thông tin liên quan đến SMS để submit kết quả.

![image](https://user-images.githubusercontent.com/42565778/189102197-93e53a1e-4d29-40f4-8d40-5dffd56a0293.png)

`submit: com.....`

##**17. A reminder was made to get something, what was it?**

- Mình nghĩ rằng các thông tin sẽ được Tim lưu trong note để ông ấy không quên, và chúng ta sẽ check bảng item để xem có thông tin gì. Điều này đã giúp tôi phát hiện đồ mà ông ấy cần mua là sữa :))
- 
![image](https://user-images.githubusercontent.com/42565778/189104079-f174568f-eb95-48b4-a330-f4399efe8239.png)

`submit: Mi...`
