# Linux swap memory

- Phân vùng swap là một khoảng trống trên một ổ cứng đã được chỉ định là nơi mà hệ điều hành có thể lưu trữ dữ liệu tạm thời khi nó không có thể giữ trong bộ nhớ RAM.

- Phân vùng swap được sử dụng khi hệ thống của bạn cần sử dụng bộ nhớ RAM và không có đủ bộ nhớ RAM để sử dụng. Nếu hệ thống cần thêm dung lượng bộ nhớ, các trang không hoạt động trong bộ nhớ RAM sẽ được chuyển sang phân vùng swap nhằm giải phóng bộ nhớ RAM cho các mục đích sử dụng khác.

### Kiểm tra swap

Trước khi tạo swap chúng ta cần phải xem máy chủ của chúng ta đã có sẵn dung lượng swap chưa. Chúng ta có thể có nhiều phân vùng swap. Bằng cách chạy lệnh sau:

`swapon -s`

VD:

<img src=https://imgur.com/cdNb5af.jpg>

*Nếu kết quả trả về không có thông tin nào có nghĩa hệ thống chúng ta chưa có phân vùng swap chúng ta cũng có thể kiểm tra phân vùng swap với tiện ích free cho chúng ta thấy mức sử dụng bộ nhớ chung của hệ thống.*

<img src=https://imgur.com/EGdWhd6.jpg>

### Kiểm tra dung lượng đĩa 

`df -h`

<img src=https://imgur.com/wMMfqUA.jpg>

## Tạo swap dùng swapfile

Bước 1: Tạo một file sẽ được sử dụng làm không gian swap. Cách thực hiện để tạo một file swap chúng ta sử dụng lệnh `fallocate`. 

VD: `fallocate -l 2G /swapfile`

<img src=https://imgur.com/J59oCmX.jpg>

Hoặc sử dụng lệnh:

`dd if=/dev/zero of=/swapfile bs=1024 count=1024`

*Bạn có thể thay đỗi count=1024 bằng giá trị khác để tạo file swap với dung lượng bạn mong muốn.*

VD: 

<img src=https://imgur.com/pGf1MRH.jpg>

Bước 2: Đảm bảo chỉ người dùng root mới có thể đọc và ghi file swap. Thực hiện như sau: 

`chmod 600 /swapfile`

Bước 3: Thiết lập phân vùng swap trên tệp. Sử dụng lệnh sau:

`mkswap /swapfile`

Bước 4: Thiết lập swap tự động được kích hoạt mỗi khi reboot

`echo /swapfile none swap defaults 0 0 >> /etc/fstab`

hoặc `echo /swapfile none swap defaults 0 0 | sudo tee -a /etc/fstab`

Sau đó sử dụng lệnh `swapon -a`