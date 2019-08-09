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
- Nhưng một file có thể có nhiều tên file tùy theo số hard link trỏ đến nó.


**Đối với Hard links: là một liên kết (link) trỏ đến vị trí lưu một file trên ổ cứng:**

`ln <path/tên file> <tên hard link>`

- Nếu đổi tên, xóa hoặc di chuyển file gốc sang thư mục khác, hard link vẫn mở được file đó vì nó vẫn trỏ đến vị trí lưu file cố định trên ổ cứng.
- Tên hard link có thể khác tên file gốc, hard link có thể nằm trong một thư mục khác với thư mục của file gốc. Vì vậy một file có thể có nhiều tên file nằm ở các thư mục khác nhau. Khi truy cập vào hard link (ví dụ nhấn chuột) sẽ truy cập đến file (mở hoặc chạy).
- Nếu đồng thời mở một file từ các hard link và tên file gốc, khi sửa ở một bản, các bản khác cũng sẽ thay đổi theo sau khi refresh hoặc reload vì thực chất là sửa trên cùng một file.
- Nếu xóa hard link hoặc xóa tên file gốc nhưng còn một hard link, file vẫn không bị xóa. File chỉ bị xóa khi không còn cái gì trỏ đến vị trí lưu nó. Như vậy muốn xóa một file, phải xóa tên file và tất cả các hard link của nó.
- Hard link không tạo được với thư mục và không tạo được với file nằm trên một partition khác.

<img src=https://imgur.com/FTKZN85.jpg>

*Như các bạn đã thấy: khi tạo liên kết giữa file command.txt (file có sẵn) với file command-2.txt và command-3.txt, khi bạn chỉnh sửa trên file gốc thì các file liên kết đều nhận được chỉnh sửa đó. Khi sử dụng lệnh rm để xóa file thì làm giảm đi một hard link. Khi số lượng hard link giảm còn 0 thì không thể truy cập tới nội dung của file được nữa*

**Đối với Symbolic Links: (còn gọi là softlink) là một liên kết tạo một đường dẫn khác đến thư mục hoặc file gốc.**

`ln -s <path/tên file> <tên soft link>`

- Là liên kết không dùng đến inode entry mà chỉ đơn thuần là một shortcut. Nó sẽ tạo ra một inode mới và nội dung của inode này trỏ đến tên tập tin gốc.
- Ví dụ file gốc passwd có đường dẫn là /etc/passwd. Trong thư mục /home/zxc tạo một soft link đặt tên là “mậtkhẩu” trỏ đến file đó. Như vậy đường dẫn mới đến file /etc/passwd là /home/zxc/mậtkhẩu. Khi truy cập đến một trong hai đường dẫn trên đều là truy cập đến file passwd.
- Nếu đổi tên, xóa hoặc chuyển file gốc sang thư mục khác thì soft link mất tác dụng, không truy cập được đến file đó nữa. Khác với hard link, khi xóa file có soft link, file bị xóa thật.
 Có thể tạo soft link với thư mục và file nằm trên partition khác.

<img src=https://imgur.com/4OoZ89G.jpg>

<img src=https://imgur.com/gtDHLx2.jpg>


