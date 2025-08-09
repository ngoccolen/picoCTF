# Mô tả thử thách
<img width="960" height="733" alt="image" src="https://github.com/user-attachments/assets/b1267f28-db20-4ace-8d6f-531988c99210" />
# Solution
<img width="1060" height="305" alt="image" src="https://github.com/user-attachments/assets/529324d3-1913-4243-aa43-0445b20162e0" />
Đây là bài liên quan đến kĩ thuật anti-debugging. Anti-debugging là kĩ thuật dùng để ngăn cản hoặc gây khó khăn trong quá trình phân tích chương trình bằng các công cụ debug như ollydbg, x64dbg,..
Ở đây ta sẽ dùng ida để gỡ lỗi
Ta tìm hàm IsDebuggerPresent(). IsDebuggerPresent là 1 API của windows, nằm trong kernel32.dll nhằm để kiểm tra xem chương trình hiện tại có bị debugger gắn vào không. Vậy để kiểm tra, ta đặt breakpoint tại `bash text  eax, eax`

