Định dạng PDF là tiêu chuẩn thực tế trong việc trao đổi tài liệu trực tuyến. Tuy nhiên, sự phổ biến như vậy cũng đã thu hút bọn tội phạm mạng phát tán phần mềm độc hại đến những người dùng không nghi ngờ. Khả năng tạo các tệp pdf độc hại để phân phối phần mềm độc hại là một chức năng đã được tích hợp trong nhiều bộ công cụ khai thác. Do người dùng ít thận trọng hơn trong việc mở tệp PDF, tệp PDF độc hại đã trở thành một vectơ tấn công khá thành công. Lưu lượng mạng được ghi lại trong lala.pcap chứa lưu lượng mạng liên quan đến một cuộc tấn công tệp PDF độc hại điển hình, trong đó người dùng không nghi ngờ sẽ mở một trang web bị xâm phạm, chuyển hướng trình duyệt web của người dùng đến URL của tệp PDF độc hại. Khi trình cắm PDF của trình duyệt mở PDF, phiên bản chưa vá lỗi của Adobe Acrobat Reader được khai th

# TOOLs:
- [De4js](https://lelinhtinh.github.io/de4js/)
- [pdfid](https://github.com/Rafiot/pdfid)
- [pdfparter](https://github.com/smalot/pdfparser)
- [peepdf](https://github.com/jesparza/peepdf)
- Wireshark
- Network Minner
- Detail chall: [Here](https://cyberdefenders.org/blueteam-ctf-challenges/47)

Thử thách này mình đã vượt qua khoảng 80% và để hoàn thiện thử thách mình vừa làm và vừa tìm hiểu thêm về các kỹ thuật và cài đặt 1 số tool Forensic PDF files, có 1 số câu khá dễ dàng để submit, nhưng đó chỉ là bề nổi của tảng băng chìm- điều cơ bản của một chuyên viên Forensic cần biết. Sau đây là cách mà cá nhân mình đã vượt qua thử thách này!!!

- Vì đây là một thử thách kết hợp với gói tin cho nên đầu tiên chúng ta phải thực hiện phân tích gói tin để tìm kiếm các thông tin có liên quan đến chủ đề này. 

## 1. How many URL path(s) are involved in this incident?


## 2. What is the URL which contains the JS code?


## 3. What is the URL which contains the JS code?


## 4. What is the MD5 hash of the PDF file contained in the packet?


## 5. What is the MD5 hash of the PDF file contained in the packet?


## 6. How many filtering schemes are used for the object streams?


## 7. What is the number of the 'object stream' that might contain malicious JS code?


## 8. Analyzing the PDF file. What 'object-streams' contain the JS code responsible for executing the shellcodes? The JS code is divided into two streams. Format: two numbers separated with ','. Put the numbers in ascending order


## 9. The JS code responsible for executing the exploit contains shellcodes that drop malicious executable files. What is the full path of malicious executable files after being dropped by the malware on the victim machine?


## 10. The PDF file contains another exploit related to CVE-2010-0188. What is the URL of the malicious executable that the shellcode associated with this exploit drop?


## 11. How many CVEs are included in the PDF file?

