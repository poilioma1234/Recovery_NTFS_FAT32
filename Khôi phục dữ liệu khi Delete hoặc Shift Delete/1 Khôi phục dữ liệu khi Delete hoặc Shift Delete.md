# 1. Khôi phục dữ liệu khi Delete hoặc Shift Delete

# Manual (FTK)

## Thực hành

![image.png](image.png)

Chuẩn bị 1 file .txt có nội dung như trên

### 1. Thực hiện khôi phục khi Delete

Khi một tập tin bị xóa mềm (chuyển vào Thùng rác), Windows sẽ đổi tên nó và tạo ra một cặp file đi liền với nhau, sử dụng chung một chuỗi 6 ký tự ngẫu nhiên

![image.png](image%201.png)

![image.png](image%202.png)

#### B1: Vào FTK để Add Evidence Item

![image.png](image%203.png)

#### B2: Chọn Logical Drive

![image.png](image%204.png)

#### B3: Chọn ổ E vừa mới xóa file .txt sau đó Finish

![image.png](image%205.png)

#### B4: Nhìn vào góc trên tay trái Evidence Tree, hãy mở ổ E ra và chọn [root]

![image.png](image%206.png)

#### B5: Chọn Mode HEX

![image.png](image%207.png)

#### B6: Nhấp vào $MFT

![image.png](image%208.png)

#### B7: Dùng Ctrl + F hoặc click chuột phải vào hex bên dưới và chọn Find để tìm kiến

![image.png](image%209.png)

#### B8: Tìm kiếm tên File .txt vừa xóa

![image.png](image%2010.png)

#### B9: Xác định tên file bị xóa

![image.png](image%2011.png)

#### B10: Vào thùng rác xem

![image.png](image%2012.png)

- **File `$R...` (`$RQBVF2A.txt`):** Chữ R đại diện cho Raw/Resource. Đây chính là file chứa **nội dung dữ liệu thực sự** của file đã bị xóa.
- **File `$I...` (`$IQBVF2A.txt`):** Chữ I đại diện cho Info/Index. Đây là **file metadata** được hệ thống tự động sinh ra. Nó có nhiệm vụ lưu trữ kích thước file gốc, thời gian file bị xóa (Deletion Time), và quan trọng nhất là **đường dẫn gốc của file**.

#### B11: Thực hiện khôi phục dữ liệu, chọn Export File …

![image.png](image%2013.png)

#### B12: Lưu vào Folder bạn muốn

![image.png](image%2014.png)

#### B13: Kiểm tra nội dung file

![image.png](image%2015.png)

### 2. Thực hiện khôi phục khi Shift Delete

**Resident** (dữ liệu nằm ngay trong MFT, không bị đẩy ra ngoài) là file dưới 1024 byte

**Non-Resident** (dữ liệu quá lớn phải lưu ở các cluster khác trên ổ cứng) trên 1024 byte, sẽ có địa chỉ nằm trong MFT.

#### Khôi phục dữ liệu đối với **Resident**

#### B1: Chuẩn bị 1 Ổ cứng E đã full format để tiện cho việc khôi phục dữ liệu, không dính dáng đến các dữ liệu cũ. Và chuẩn bị 1 file .txt không tới 1Kb.

![image.png](image%2016.png)

#### B2: Tiến hành Shift Delete

![image.png](image%2017.png)

![image.png](image%2018.png)

#### B3: Vào FTK chọn Add Evidence Item…

![image.png](image%2019.png)

#### B4: Select Source chọn Logical Drive

![image.png](image%2020.png)

#### B5: Select Drive chọn ổ E vừa Shift Delete file txt sau đó Finish

![image.png](image%2021.png)

#### B6: Chọn Mode Hex để tiện xem các dữ liệu Dec

![image.png](image%2022.png)

#### B7: Thực hiện nhấp vào [root] và nhấp vào $MFT

![image.png](image%2023.png)

#### B8: Nhấn tổ hợp phím Shift F để tìm kiếm file vừa xóa

![image.png](image%2024.png)

#### B9: Phân tích

![image.png](image%2025.png)

1. **0x10 đầu tiên** 

![image.png](image%2026.png)

Có 96 value dựa trên 60 00 = 96 Value 

Từ cái 0x10 đầu này ta có thể xác định ngày tạo, ngày chỉnh sửa cuối, và ngày truy cập cuối theo quy tắc **C-M-E-A** (Creation, Modification, Entry Modified, Access).

![image.png](image%2027.png)

Ngày tạo <99 DA 02 03 30 C3 DC 01>, 4/03/2026 vào lúc 1:06:29 PM

![image.png](image%2028.png)

Ngày chỉnh sửa <AF EE 86 22 30 C3 DC 01 >, 4/3/2026 vào lúc 1:07:21 PM

![image.png](image%2029.png)

Ngày cập nhật MFT <AF EE 86 22 30 C3 DC 01>, 4/3/2026 vào lúc 1:07:21 PM

![image.png](image%2030.png)

Lần truy cập cuối <AF EE 86 22 30 C3 DC 01>, 4/3/2026 vào lúc 1:07:21 PM

1. **0x30 tiếp theo xác định tên của File**

![image.png](image%2031.png)

đoạn 80 00 là value = 128 nên bôi đen đúng 128 value

1. **0x40 kế đến** 

![image.png](image%2032.png)

Ví dụ dễ hiểu về 0x40 là Object ID

![image.png](image%2033.png)

1. **0x80 là data của file** 

![image.png](image%2034.png)

40 00 có 64 value nên bôi đen hết 64 value tính từ 80 00

#### B10: Thực hiện khôi phục file, chuột phải vào phần dữ liệu của 0x80 vừa bôi đen và chọn Save Selection

![image.png](image%2035.png)

![image.png](image%2036.png)

![image.png](image%2037.png)

#### Khôi phục dữ liệu đối với Non-**Resident**

Non-**Resident** dữ liệu trong file trên 1024byte

#### B1: Chuẩn bị ổ E đã full format và 1 file .txt trên 1KB

![image.png](image%2038.png)

#### B2: Thực hiện Shift Delete

![image.png](image%2039.png)

#### B3: Vào FTK chọn Add Evidence Item…

![image.png](image%2040.png)

#### B4: Chọn Logical Drive

![image.png](image%204.png)

#### B5: Chọn ổ E vừa mới xóa file .txt sau đó Finish

![image.png](image%205.png)

#### B6: Nhìn vào góc trên tay trái Evidence Tree, hãy mở ổ E ra và chọn [root]

![image.png](image%206.png)

#### B7: Chọn Mode HEX

![image.png](image%207.png)

#### B8: Nhấp vào $MFT

![image.png](image%208.png)

#### B9: Dùng Ctrl + F hoặc click chuột phải vào hex bên dưới và chọn Find để tìm kiến

![image.png](image%209.png)

#### B10: Tìm kiếm tên File .txt vừa xóa

![image.png](image%2041.png)

#### B11: Xác định tên file bị xóa

![image.png](image%2042.png)

B12: Tính toán địa chỉ gốc của File

![image.png](image%2043.png)

1. 0x80 
    
    48 00 có 72 value trong đó sau 48 00 00 00 là 01 00 
    
    00 00 nghĩa là dữ liệu là **Resident** (nằm ngay trong MFT)
    
    01 00 nghĩa là dữ liệu là **Non-Resident** (bị đẩy ra ngoài cluster)
    
    → file hiện tại vượt quá 1024 nên data được lưu ngoài cluster
    
2. Nhìn vào dòng cuối <21 4C 88 05>, tại sao lại là dòng Hex này?
    
    Cuối dã 0x80 thường là lưu size và địa chỉ cluser 
    
    21 tách ra làm 2 phần là 2 và 1
    
    1 bên phải là size
    
    2 bên trái là Offset (vị trí)
    
    ![image.png](image%2044.png)
    

Giờ ta đã biết Size của file là 311,296 byte nghĩa là khoảng 304 KB

Và địa chỉ cụ thể là 5,799,936 byte

B13: Tìm dữ liệu từ địa chỉ đã tìm kiếm, ta thực hiện nhấp chuột vào ổ E và Ctrl + G, paste địa chỉ vào 

![image.png](image%2045.png)

![image.png](image%2046.png)

B14: Nhấp vào điểm bắt đầu data là 74 sau đó click chuột phải chọn Set Selection Length 

![image.png](image%2047.png)

Và nhập 311,296 byte vào và nhấp OK

![image.png](image%2048.png)

![image.png](image%2049.png)

B15: Chuột phải và chọn Save Selection

![image.png](image%2050.png)

Và nhấn chọn tên lẫn nơi lưu 

![image.png](image%2051.png)

B16: Kiểm tra file

![image.png](image%2052.png)

## Tools (GUI)

Disk Drill

EaseUS Data Recovery Wizard Free

Photorec_win (cli)

Recuva Wizard