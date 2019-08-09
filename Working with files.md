## Search for files

**1. Lệnh find**

`find đường_dẫn -name kí_tự_cần_tìm`

- Lệnh này sẽ cung cấp cho bạn một danh sách tất cả các tệp tin và thư mục trong đường dẫn hiện hành.
- Để dễ dàng tìm kiếm hơn bạn có thể kết hợp lệnh find cùng các tham số vd: tham số -name

***Tìm tất cả tệp có đuôi cần tìm, sử dụng `*`***

VD: tìm tất cả các tệp có đuôi `.log`

<img src=https://imgur.com/yggy1s0.jpg>

***Chỉ tìm kiếm các thư mục, sử dụng `-type d`***

VD: tìm các đường dẫn đến thư mục yum

<img src=https://imgur.com/YGTEfuN.jpg>

***Chỉ tìm kiếm các tệp, sử dụng `-type f`***


VD: tìm các đường dẫn đến tệp error

<img src=https://imgur.com/jvirEoJ.jpg>

***Tìm kiếm dựa trên kích thước, sử dụng `-size`***

`find / -size +10M` #Để tìm các tệp có kích thước lớn hơn 10 MB.



**2. Lệnh `locate`**

- Với lệnh `locate` sẽ tìm kiếm nhanh và chi tiết hơn lệnh find. Lệnh locate sẽ trả về một danh sách tất cả tên đường dẫn chứa nhóm có ký tự đặc biệt. Bạn có thể cập nhật nó bất cứ lúc nào bằng cách chạy `updatedb` với tư cách là người dùng root.

**Cài đặt**

`yum install -y mlocate`

`updatedb`

*Kết quả của locate đôi khi có thể dẫn đến một danh sách rất dài. Để có được danh sách ngắn hơn phù hợp hơn, chúng ta có thể sử dụng grepchương trình làm bộ lọc. Nó sẽ chỉ in các dòng có chứa một hoặc nhiều chuỗi được chỉ định*

VD: locate gz | grep bin #Sẽ liệt kê tất cả các tệp và thư mục có cả "gz" và "bin" trong tên của chúng.

<img src=https://imgur.com/ol9S6z6.jpg>



## Quản lý tập tin

### Sử dụng các tiện ích sau để xem tệp:

| Command | Usage |
----------|--------
cat| Được sử dụng để xem các tệp có kích thước vừa phải
tac|Được sử dụng để xem xét một tập tin ngược, bắt đầu với dòng cuối cùng
less|Được sử dụng để xem các tệp lớn hơn vì đây là chương trình phân trang; nó tạm dừng ở mỗi màn hình văn bản, cung cấp khả năng cuộn lại và cho phép bạn tìm kiếm và điều hướng trong tệp.
tail|Được sử dụng để in 10 dòng cuối cùng của tệp theo mặc định. Bạn có thể thay đổi số lượng dòng bằng cách thực hiện -n 15 hoặc chỉ -15 nếu bạn muốn xem 15 dòng cuối cùng thay vì mặc định
head|Theo mặc định, nó in 10 dòng đầu tiên của tệp	

***Tạo một tệp trống bằng cách sử dụng `touch`***

`touch <filename>`

***Tùy chọn -t cho phép bạn đặt dấu ngày và thời gian của tệp. Để đặt dấu thời gian thành thời gian cụ thể***

<img src=https://imgur.com/8VeSTWc.jpg>

*Tùy chọn nãy sẽ đặt tệp vqmanh.txt theo thứ tự `tháng-ngày-giờ`vào 10 giờ sáng ngày 9 tháng 8, `08-09-10:00`*

***Tạo thư mục với `mkdir`***

`mkdir /data`

***Tạo cây thư mục với tùy chọn `-p`***

`mkdir -p /data/nhanhoa/thuctap`

<img src=https://imgur.com/khBArOC.jpg>

## So sánh các tập tin

<img src=https://imgur.com/DtbOxdV.jpg>


<img src=https://imgur.com/4kRGG5V.jpg>

## The file streams

- Đầu vào chuẩn hay còn gọi là stdin.

- Đầu ra chuẩn, hay còn gọi là stdout.

- Báo lỗi chuẩn, hay stderr.