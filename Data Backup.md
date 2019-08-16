# Data Backup

## Sao lưu dữ liệu

Lệnh `rsync` được sử dụng để đồng bộ hóa toàn bộ cây thư mục. Về cơ bản, nó sao chép tập tin như lệnh cp. Ngoài ra, `rsync` kiểm tra xem tập tin đang được sao chép đã tồn tại chưa. Nếu tệp tồn tại và không có thay đổi về kích thước hoặc thời gian sửa đổi, `rsync` sẽ tránh được một bản sao không cần thiết và tiết kiệm thời gian. Hơn nữa, vì rsync chỉ sao chép các phần của tệp đã thực sự thay đổi, nên nó có thể rất nhanh.



### 1. Tính năng nổi bật của Rsync

- Rsync cho phép copy giữ nguyên thông số của file/folder như nguyên file gốc.

- Rsync nhanh hơn cp vì nó sử dụng giao thức remote-update, nó chỉ copy và thay thế những data thay đổi.

- Rynsc tiết kiệm băng thông do nó nén dữ liệu khi trasfer và giải nén tại đích.

- Không yêu cầu quyền super-user.

### 2. Cài đặt Rsync

Cài trên Red Had/ CentOS

`yum install rsync`

Cài trên Debian/Ubuntu

`apt-get install rsync`

### 3. Sử dụng Rsync

**Một số option cơ bản:**

- -v hiển thị trạng thái kết quả

- -r copy dữ liệu recursively, nhưng không đảm bảo thông số của file và thư mục

- -a cho phép copy dữ liệu recursively đồng thời giữ nghuyên được tất cả thông số và thư mục của file

- -z nén dữ kiệu khi trasfer giúp tiết kiệm băng thông nhưng mất thêm chút thời gian.

- -h trả về kết quả dễ đọc

- --delete xóa dữ liệu destination nếu source không tồn tại dữ liệu đó.

- --exclude loại trừ dự liệu không muốn truyền , nếu cần loại ra nhiều file hoặc folder ở nhiều đường dẫn khác nhau thì mỗi cái cần thêm --exckude tương ứng.

*Lưu ý: Chỉ dùng --delete khi bạn chắc chắn rằng "source" và "destination" đều đúng, nhất là về vị trí, nếu bạn để sai vị trí hoặc sai tên thư mục thì có thể dẫn đến mất dữ liệu toàn bộ khi dùng rsync!*

### Backup file và thư mục trên local

Cú pháp: `rsync -option nguồn_backup đích_đến`

VD: Backup thư mục con của /home đến /tmp/backup

<img src=https://imgur.com/QxBKWRF.jpg>


### Backup file và thư mục giữa các server

*Điều kiện: cả 2 server đều đã cài `rsync`*

**Backup thư mục từ Local lên Remote Server**

Cú pháp: `rsync -option nguồn_backup root@IP_Server:folder_đích`

VD: Backup thư mục con trên /home từ local sang thư mục /backup của server có địa chỉ là: 66.0.0.200.

<img src=https://imgur.com/DavIbJk.jpg>

Sang server backup kiểm tra

<img src=https://imgur.com/3feoz6q.jpg>



**Backup thư mục từ Remote Server về Local**

Cú pháp: `rsync -option root@IP_Server:nguồn_backup folder_đích `

VD: Backup thư mục con trên /backup của server remote sang thư mục /home của local.

<img src=https://imgur.com/8P0alTh.jpg>


# Compress the data

## Nén giữ liệu

Dữ liệu tệp thường được nén để tiết kiệm dung lượng ổ đĩa và giảm thời gian truyền tệp qua mạng. Linux sử dụng một số phương pháp để thực hiện việc nén này.

Command|	Usage
-------|----------
gzip|Tiện ích nén Linux được sử dụng thường xuyên nhất	
bzip2	 |Tạo các tệp nhỏ hơn đáng kể so với các tệp được tạo bởi gzip
xz|Tiện ích nén hiệu quả nhất về không gian được sử dụng trong Linux. Nó hiện được kernel.org sử dụng để lưu trữ tài liệu lưu trữ của nhân Linux
zip|Thường được yêu cầu kiểm tra và giải nén tài liệu lưu trữ từ các hệ điều hành khác

*Các kỹ thuật này khác nhau về hiệu quả của việc nén (tiết kiệm được bao nhiêu dung lượng) và thời gian để nén. Nói chung các kỹ thuật hiệu quả hơn mất nhiều thời gian hơn. Thời gian giải nén không thay đổi nhiều theo các phương pháp khác nhau.*


# Archiving data

## Lưu trữ dữ liệu

Lệnh `tar` cho phép bạn tạo ra hoặc trích xuất tập tin từ một tập tin lưu trữ, thường được gọi là một `tarball`. Đồng thời, bạn có thể tùy chọn nén trong khi tạo tệp lưu trữ và giải nén trong khi trích xuất nội dung của nó.

Dưới đây là một số ví dụ về việc sử dụng tar:

Command	|Usage
--------|-----
tar xvf mydir.tar|	Giải nén tất cả các tệp trong mydir.tar vào thư mục mydir
tar zcvf mydir.tar.gz mydir	|Tạo kho lưu trữ và nén bằng gzip
tar jcvf mydir.tar.bz2 mydir|	Tạo kho lưu trữ và nén với bz2
tar xvf mydir.tar.gz	|Trích xuất tất cả các tệp trong mydir.tar.gz vào thư mục mydir.
tar cvf mydir.tar	|Hiển thị nội dung vào thư mục mydir

# Copying disks

## Sử dụng lệnh `dd`

### 1. Tính năng của lệnh dd
Dùng để dùng để sao lưu dữ liệu, copy, chuyển đổi dữ liệu với những option tương ứng. Có thể sử dụng với những trường hợp cơ bản sau:

- Sao lưu hoặc phục hồi dữ liệu ổ cứng hoặc một phân vùng xá định trước.

- Sao lưu lại MBR trong máy. MBR là file rất quan trọng, nó chứa các lệnh để GRUB hoặc LILO nạp hệ điều hành.

- Chuyển chữ hoa sang chữ thường và ngược lại.

- Tạo file có kích thước cố định.

- Tạo file ISO.

### 2. Cú pháp

`dd if=nguồn of=đích option`

- nguồn ở đây là ổ đĩa, thư mục, file ta muốn sao lưu

- đích ở đây là ổ đĩa, thư mục, file ta muốn lưu data vào đó.

Các tùy chọn ta cơ bản:

- bs=byte quá trình đọc ghi bao nhiêu bytes 1 lần

- cbs=bytes chuyển đổi bao nhiêu bytes 1 lần

- count=x thực hiện x block trong quá trình thực hiện lệnh

- ibs/obs: chỉ ra số byte một lần đọc ghi

- skip=x bỏ qua x block đầu vào

- conv=option. Option có thể là các option sau:

    - ucase/lcase: Chuyển chữ thường thành chữ hoa và ngược lại.

    - noerror : Tiếp tục sao chép dữ liệu khi đầu vào bị lỗi.

    - rsync: Đồng bộ dữ liệu với ổ đang sao chép sang.

*Lưu ý: Đơn vị mặc định mỗi lần đọc được tính theo kb. Chúng ta có thể sử dụng một số tùy chọn sau để thay đổi định dạng:*

- c=1
- w=2
- b=512
- kB=1000
- K=1024
- MB=1000*1000
- M=1024*1024
- GB=1000 * 1000 * 1000
- G=1024 * 1024 * 1024

### 3. Ví dụ

**1. Để sao lưu bản ghi khởi động chính (MBR)**

`dd if=/dev/sda of=sda.mbr bs=512 count=1`

<img src=https://imgur.com/pfCfhEJ.jpg>

**2. Sao chép từ sda sang sdb, tiếp tục khi báo lỗi và đồng bộ dữ liệu sao chép**

`dd if=/dev/sda of=/dev/sdb conv=noerror, sync`

*Với cú pháp trên thì tất cả dữ liệu có trong đĩa thứ 2 trước đó sẽ bị xoá toàn bộ. Và một bản sao của đỉa thứ nhất sẽ được tạo trên đĩa thứ 2.*

**3. Tạo một file image cho ổ sda1**

`dd if=/dev/sda1 of=/root/sda1.img`

<img src=https://imgur.com/UVz9gBG.jpg>

Restore lại với `dd if=/root/sda1.img of=/dev/sda1`

**4. Tạo 1 file kích thước 100M**

`dd if=/dev/zero of=/root/file1 bs=100M count=1`

<img src=https://imgur.com/doSV0z8.jpg>

**5. Tạo bản sao lưu và nén**

`dd if=nguồn | gzip | dd of=đích `