# Mô tả thử thách
<img width="955" height="801" alt="image" src="https://github.com/user-attachments/assets/ef7ac631-ce15-4a98-99b4-03ac74e2407e" />

## Phân tích chương trình
Sau khi tải 2 file về và nén, ta vào trong 1 trò chơi với dòng chữ trên cùng "Escape to find the flag"
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f7249fac-cc11-415f-a6c3-5ef1034fd985" />
Ở trong game, ta có thể di chuyển lên xuống trái phải với các phím W,S,A,D, nhảy, leo thang, đổi vũ khí
Mục tiêu của ta là nhảy lên để thoát khỏi bức tường để đến được lá cờ trắng trên kia
<img width="1100" height="619" alt="image" src="https://github.com/user-attachments/assets/805d4f0a-92e3-4af7-beb3-b18011dc1b20" />
Dùng dnSpy để phân tích file Assembly-CSharp.dll, tệp DLL chứa mã C# cho logic của trò chơi
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f19f51e0-2ddf-4746-bfec-ec17e823b23b" />
Ở đây ta quan tâm đến đoạn code xử lý quy tắc nhảy 
<img width="1009" height="161" alt="image" src="https://github.com/user-attachments/assets/b983821a-5283-4d97-b90f-c45454af2ce2" />
Đoạn code này chỉ rằng chỉ khi đứng trên mặt đất, nhân vật mới được phép nhảy lên, tức là 1 khi đã nhảy lên không trung, khi chưa chạm đất thì không thể nhảy cao hơn được nữa. Vì vậy, ta vào edit method, sau đó xóa dòng ```bash && this.characterController.isGrounded``` đi và lưu tệp đã chỉnh sửa. Sau đó mở lại trò chơi
<img width="1076" height="132" alt="image" src="https://github.com/user-attachments/assets/7bfd0a31-ba47-4a44-b28b-bd09d832b684" />
Giờ thì ta có thể nhảy giữa không trung để qua tường và đến với lá cờ trắng, khi chạm vào lá cờ thì flag sẽ hiện ra
<img width="1920" height="1080" alt="Screenshot (238)" src="https://github.com/user-attachments/assets/4edbf556-7bb1-4547-b9ee-758bdfa27994" />

## picoCTF{WELCOME_TO_UNITY!!}







