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


##**4. What is the title of the webpage that was viewed the most? (Three words)**

- Có rất nhiều cách để làm câu này, với mình khi không sử dụng autopsy thì mình check history.db và kết quả thu được:

![image](https://user-images.githubusercontent.com/42565778/189099256-78b463d6-592a-4500-9bd8-e9df89cb56bd.png)

- Hoặc các bạn tận dụng plugin có sẵn ở autopsy để lọc thông tin trong webhistory:
 
 ![image](https://user-images.githubusercontent.com/42565778/189099424-34a16676-68c1-4ff7-af05-575b599c2075.png)


`submit: kirby with.....`


##**5. What is the title of the first podcast that was downloaded?**


##**6. What is the name of the WiFi network this device connected to? (Two words)**


##**7. What is the name of the skin/color scheme used for the game emulator? This should be a filename.**


##**8. How long did the News App run in the background?**



##**9. What was the first app download from AppStore? (Two words)**


##**10. What app was used to jailbreak this device?**


##**11. How many applications were installed from the app store?**


##**12. How many save states were made for the emulator game that was most recently obtained?**



##**13. What language is the user trying to learn?**


##**14. The user was reading a book in real life but used their IPad to record the page that they had left off on. What number was it?**


##**15. If you found me, what should I buy?**


##**16. There was an SMS app on this device's dock. Provide the name in bundle format: com.provider.appname**

- Với câu hỏi này mình nghĩ ngay tới việt check các thông tin các gói cài đặt có trong Application...db điều này hoàn toàn có thể và theo yêu càu đề bài về việc app liên quan đến quản lý SMS chính vì thế sẽ lọc các thông tin liên quan đến SMS để submit kết quả.

![image](https://user-images.githubusercontent.com/42565778/189102197-93e53a1e-4d29-40f4-8d40-5dffd56a0293.png)

`submit: com.....`

##**17. A reminder was made to get something, what was it?**

