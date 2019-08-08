# Basic Commands


### 1. Các chương trình thực thi nằm trong các thư mục sau:

```
/bin
/usr/bin
/sbin
/usr/sbin
/opt
```
VD: 

<img src=https://imgur.com/kyZEiUz.jpg>


**Sử dụng which để xác định vị trí của các chương trình**

`which tên_tiện_ích`

VD:

<img src=https://imgur.com/WaEKqzi.jpg>


**Nếu không tìm thấy chương trình, sử dụng `whereis` để tìm các gói trong một phạm vi rộng hơn của các thư mục hệ thống:**

VD:

<img src=https://imgur.com/Ny7aWk2.jpg>


### 2. Truy cập các thư mục

**Sử dụng lệnh `cd`**

| Command | Result |
----------|---------
cd| Di chuyển đến thư mục
cd ..| Di chuyển đến thư mục cha trước đó
cd -| Di chuyển đến thư mục thao tác trước đó

VD: 

<img src=https://imgur.com/Se6isAl.jpg>

### 3. Liệt kê danh sách hệ thống tập tin

| Command | Result |
----------|---------
ls	| Liệt kê nội dung của thư mục làm việc hiện tại
ls –a|	Liệt kê tất cả các tệp bao gồm các tệp và thư mục ẩn
tree|	Hiển thị chế độ xem dạng cây của hệ thống tệp
tree -d|Liệt kê các thư mục và loại bỏ tên tập tin liệt kê

VD:

<img src=https://imgur.com/MymTGCU.jpg>


<img src=https://imgur.com/tpxwosi.jpg>

### 4. Hard and Symbolic Links (Liên kết cứng và liên kết tượng trưng)

**Trong một hệ thống file**

- Mỗi file có một và chỉ một inode.
- Mỗi inode cũng chỉ có một số inode duy nhất.
- Nhưngmột file có thể có nhiều tên file tùy theo số hard link trỏ đến nó.


**Đối với Hard links: là một liên kết (link) trỏ đến vị trí lưu một file trên ổ cứng:**

`ln [file nguồn] [file đích]`

<img src=https://imgur.com/FTKZN85.jpg>

*Như các bạn đã thấy: khi tạo liên kết giữa file command.txt (file có sẵn) với file command-2.txt và command-3.txt, khi bạn chỉnh sửa trên file có sẵn thì các file liên kết đều nhận được chỉnh sửa đó. Khi sử dụng lệnh rm để xóa file thì làm giảm đi một hard link. Khi số lượng hard link giảm còn 0 thì không thể truy cập tới nội dung của file được nữa*

**Đối với Symbolic Links: (còn gọi là softlink) là một liên kết tạo một đường dẫn khác đến thư mục hoặc file gốc.**

`ln -s [file nguồn] [file đích]`

*Là liên kết không dùng đến inode entry mà chỉ đơn thuần là một shortcut. Nó sẽ tạo ra một inode mới và nội dung của inode này trỏ đến tên tập tin gốc.*

<img src=https://imgur.com/4OoZ89G.jpg>

*Sự khác biệt giữa Hard links và Symbolic Links: khi bạn xóa tệp gốc thì với Hard links tệp liên kết vẫn còn, còn Symbolic Links sẽ mất*

<img src=https://imgur.com/gtDHLx2.jpg>

Chú ý: 
- Hardlink chỉ tạo được với file nằm trên cùng một partition, không tạo được với thư mục hoặc với file nằm trên partition khác.
- Softlink tạo được với thư mục và tạo được với thư mục, file nằm trên partition khác.
