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

- --exclude loại trừ dự liệu không muốn truyền , nếu càn loại ra nhiều file hoặc folder ở nhiều đường dẫn khác nhau thì mỗi cái cần thêm --exckude tương ứng.

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