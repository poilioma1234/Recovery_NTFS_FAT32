# Phục hồi dữ liệu trên hệ thống tập tin NTFS/FAT32

Bài viết này của mình sẽ không đi quá sâu vào lý thuyết, vì mình biết ai cũng sẽ ngán những bài viết dài dòng và đọc những khái niệm không hiểu sẽ bỏ ngay.

Những hướng mình khôi phục dữ liệu như:

- Khôi phục dữ liệu khi Delete hoặc Shift Delete
- Khôi phục dữ liệu khi Quick Format
- Khôi phục dữ liệu khi dính ransomeware (Ransomware không vô hiệu hóa/xóa bản sao lưu Volume Shadow Copy (VSS))

---

## Khái niệm cơ bản

![image.png](image.png)

![image.png](image%201.png)

Đối với những người quan tâm đến việc khôi phục dữ liệu thôi, không điều tra thì ta nên biết cơ bản những thứ sau:

- Header sẽ chứa trạng thái đối với file 01 00 (file đang hoạt động) hoặc 00 00 (file đã bị xóa), đối với folder 02 00 (folderđã bị xóa) hoặc 03 00 (Folder đang hoạt động).
- 0x10 cơ bản là ngày được tạo, ngày thay đổi và đọc cuối cùng.
- 0x30 là tên file.
- 0x80 là dữ liệu và chứa size lẫn địa chỉ nếu file lớn hơn 1024 byte.

Còn đối với những bạn chơi CTF hướng Forensic thì sẽ quan tâm hơn về 0x50, 0x60, …

# Kết quả đạt được

Khi bị ghi đè (overwrite) thì khả năng cứu dữ liệu gần như bằng 0 trên mọi hệ tập tin. Tuy nhiên, với các trường hợp xóa hoặc Quick Format thông thường, cấu trúc phục hồi của NTFS và FAT32 sẽ có sự khác biệt về độ vẹn toàn của file.

Và đối với các Ransomeware đời cũ không có vô hiệu hóa/xóa bản sao lưu Volume Shadow Copy (VSS) thì sẽ có thể khôi phục lại dữ liệu. Đối với các ransomeware đời mới hơn thì chắc chỉ có recovery bằng Backup đã lưu, hoặc trả tiền cho hacker quá:), decrypt được AES-256 hay RSA thì tôi cũng nể:). 

---

HappyHacking! TNWAN
