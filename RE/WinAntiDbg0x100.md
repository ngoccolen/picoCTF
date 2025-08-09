# Mô tả thử thách
<img width="960" height="733" alt="image" src="https://github.com/user-attachments/assets/b1267f28-db20-4ace-8d6f-531988c99210" />
# Solution
<img width="1060" height="305" alt="image" src="https://github.com/user-attachments/assets/529324d3-1913-4243-aa43-0445b20162e0" />
Đây là bài liên quan đến kĩ thuật anti-debugging. Anti-debugging là kĩ thuật dùng để ngăn cản hoặc gây khó khăn trong quá trình phân tích chương trình bằng các công cụ debug như ollydbg, x64dbg,..
Ở đây ta sẽ dùng ida để gỡ lỗi
Ta tìm hàm IsDebuggerPresent(). IsDebuggerPresent là 1 API của windows, nằm trong kernel32.dll nhằm để kiểm tra xem chương trình hiện tại có bị debugger gắn vào không. Vậy để kiểm tra, ta đặt breakpoint tại ``` text  eax, eax``` (test thực hiện phép AND theo bit giữa 2 toán hạng, nhưng không lưu kết quả vào thanh ghi, mà chỉ cập nhật các flags trong CPU, dùng để kiểm tra xem 1 giá trị có bằng 0 không , nếu eax=0 thì nó sẽ nhảy đến hàm in flag, nếu eax=1 thì in thông báo debugger detected)
<img width="739" height="261" alt="image" src="https://github.com/user-attachments/assets/a376e2f9-8aed-4941-ba45-2331944a8fb4" />
Sau khi đặt breakpoint, ta chạy chương trình, để ý thấy eax đang là 1
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5008d1bd-1a7d-4dcc-b3f0-9eaeb5400f0f" />
Patch giá trị eax thành 0, rồi chạy chương trình
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/efad8759-3ab3-4c6a-86fb-7580ea7da70e" />
Ta sẽ được thông báo in flag như này
<img width="1020" height="123" alt="image" src="https://github.com/user-attachments/assets/43c5352e-1b62-4f69-82cc-937b188f6a87" />







