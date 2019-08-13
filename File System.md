
# Mount - mount a filesystem


**Thiết bị phải được gắn vào 1 thư mục trống bất kỳ có sẵn trên cây thư mục trước khi bạn có thể truy cập tới nó. Thư mục trống mà gắn với thiết bị kể trên được gọi là mount point.**

Trong đó mỗi thiết bị có các filesystem khác nhau như:

- FAT16, FAT32, NTFS:  thường gặp trong Windows.
- EXT2, EXT3, EXT4: thường gặp trong Linux.
- iso9660: định dạng của đĩa CD/DVD hoặc file ISO.

`mount -option device mount_point`

VD: mount ổ /dev/sda1 vào thư mục /mnt ta làm như sau:
` mount /dev/sda1 /mnt/`

Sau đó sử dụng lệnh `lsblk` để kiểm tra

 <img src=https://imgur.com/7a2aTsq.jpg>


**Option:**

- `mount -V` : in ra phiên bản đang sử dụng của mount.

- `mount -h` : Hiển thị trợ giúp về lệnh trên

- `mount và mount -l` : Khi gõ không option hoặc -l  thì hệ thống sẽ hiển thị toàn bộ các mount đang tồn tại trên hệ thống

- `mount -a` : Sẽ mount toàn bộ các điểm gắn trước đó vào /etc/fstab. Bởi lưu ý rằng khi sử dụng mount nếu ta không lưu lại trong /etc/fstab thì sau khi reboot hệ thống sẽ không nhận các điểm mount mà ta đã thiết lập trước đó. Vì vậy nếu thêm option trên vào thì vĩnh viễn mount đã được cấu hình kể cả khi reboot hệ thống.

**Các umount lệnh được sử dụng để tách các hệ thống tập tin từ các mount point.**


VD: 

<img src=https://imgur.com/8PVE1vu.jpg>

## Cấu hình mount trong /etc/fstab

**Bước 1: Lấy UUID của volume: `blkid`**

<img src=https://imgur.com/j72O2dX.jpg>

Mount bằng UUID của volume sẽ đảm bảo volume của bạn luôn được mount chính xác tới thư mục cấu hình, việc sử dụng các device name như /dev/vdb sẽ không thực sự đúng trong quá trình sử dụng lâu dài với nhiều thao tác gắn, gỡ volume khỏi server vì tên này có thể thay đổi.

**Bước 2: Thêm vào file /etc/fstab dòng lệnh:**

`UUID="5ed2948f-5865-4fbb-8b27-7472557d61da  /mnt  ext4  defaults  0  0`

**Bước 3: Lưu lại file và chạy lệnh:**

`mount -a`

*Nếu có lỗi phát sinh, không reboot lại server để tránh tình trạng server không thể khởi động. Kiểm tra cấu hình trong file /etc/fstab và chạy lại lệnh cho tới khi không có thông báo lỗi.*


# Cấu trúc hệ thống tập tin

Trên nhiều hệ thống, bao gồm cả Linux, hệ thống tập tin được cấu trúc như một cái cây. Bắt đầu từ thư mục gốc thường được gọi là root, đánh dấu sự khởi đầu của hệ thống tập tin phân cấp được ký hiệu là `/`

### 1. / - Root
- Đúng với tên gọi của mình: nút gốc (root) đây là nơi bắt đầu của tất cả các file và thư mục. Chỉ có root user mới có quyền ghi trong thư mục này. Chú ý rằng /root là thư mục home của root user chứ không phải là /.
### 2. /bin - Chương trình của người dùng
Các /binthư mục chứa tập tin nhị phân thực thi, lệnh thiết yếu được sử dụng trong chế độ đơn người dùng, và các lệnh cần thiết theo yêu cầu của tất cả người dùng hệ thống, chẳng hạn như `ps, ls, cp`. Các lệnh không cần thiết cho hệ thống ở chế độ một người dùng được đặt trong `/usr/bin` thư mục, trong khi `/sbin` thư mục được sử dụng cho các nhị phân thiết yếu liên quan đến quản trị hệ thống, chẳng hạn như `ifconfig` và `shutdown`. Ngoài ra còn có một `/usr/sbin` thư mục cho các chương trình quản trị hệ thống ít cần thiết hơn. Tất cả các thư mục nhị phân nằm dưới phân vùng gốc. Đôi khi `/usr` là một hệ thống tệp riêng biệt có thể không khả dụng trong chế độ một người dùng.
### 3. /sbin - Chương trình hệ thống
- Cũng giống như /bin, /sbin cũng chứa các chương trình thực thi, nhưng chúng là những chương trình của admin, dành cho việc bảo trì hệ thống. Ví dụ như: reboot, fdisk, iptables...
### 4. /etc - Các file cấu hình
- Thư mục này chứa các file cấu hình của các chương trình, đồng thời nó còn chứa các shell script dùng để khởi động hoặc tắt các chương trình khác. Ví dụ: tệp `resolv.conf` cho hệ thống biết nơi sẽ truy cập mạng để lấy tên máy chủ thành ánh xạ địa chỉ IP (DNS. File `passwd`, `shadow` và `group` để quản lý tài khoản người dùng. Một số bản phân phối Linux mở rộng nội dung của /etc. Ví dụ, Red Hat thêm thư mục /etc/sysconfig con chứa nhiều tệp cấu hình hơn.
### 5. /dev - Các file thiết bị
- Các phân vùng ổ cứng, thiết bị ngoại vi như USB, ổ đĩa cắm ngoài, hay bất cứ thiết bị nào gắn kèm vào hệ thống đều được lưu ở đây. Ví dụ: `/dev/sdb1` là tên của USB bạn vừa cắm vào máy, để mở được USB này bạn cần sử dụng lệnh mount với quyền root: `mount /dev/sdb1 /tmp`

### 6. /tmp - Các file tạm
- Thư mục này chứa các file tạm thời được tạo bởi hệ thống và các người dùng. Các file lưu trong thư mục này sẽ bị xóa khi hệ thống khởi động lại.
### 7. /proc - Thông tin về các tiến trình
- Thông tin về các tiến trình đang chạy sẽ được lưu trong /proc dưới dạng một hệ thống file thư mục mô phỏng. Ví dụ thư mục con /proc/{pid} chứa các thông tin về tiến trình có ID là pid (pid ~ process ID). Ngoài ra đây cũng là nơi lưu thông tin về về các tài nguyên đang sử dụng của hệ thống như: `/proc/version`, `/proc/uptime`...
### 8. /var - File về biến của chương trình
- Thông tin về các biến của hệ thống được lưu trong thư mục này. Như thông tin về log file: `/var/log`, các gói và cơ sở dữ liệu `/var/lib`...
### 9. /usr - Chương trình của người dùng
- Chứa các thư viện, file thực thi, tài liệu hướng dẫn và mã nguồn cho chương trình chạy của hệ thống. Trong đó /usr/bin chứa các file thực thi của người dùng như: at, awk, cc, less... Nếu bạn không tìm thấy chúng trong /bin hãy tìm trong /usr/bin
`/usr/sbin` chứa các file thực thi của hệ thống dưới quyền của admin
- Nếu bạn không tìm thấy chúng trong /sbin thì hãy tìm trong thư mục này. `/usr/lib` chứa các thư viện cho các chương trình trong /usr/bin và `/usr/sbin` `/usr/local` chứa các chương tình của người dùng được cài từ mã nguồn. Ví dụ như bạn cài apache từ mã nguồn, nó sẽ được lưu dưới `/usr/local/apache2`

### 10. /home - Thư mục người của dùng
- Thư mục này chứa tất cả các file cá nhân của từng người dùng. Ví dụ: `/home/pak`, `/home/vqmanh`
### 11. /boot - Các file khởi động
- Thư mục /boot chứa một vài tệp cần thiết để khởi động hệ thống. Đối với mỗi kernel thay thế được cài đặt trên hệ thống, có bốn tệp:

    - `vmlinuz` là hạt nhân Linux được nén, cần thiết để khởi động
    - `initramfs` là hệ thống tập tin ram ban đầu, cần thiết để khởi động
    - `config is` tập tin cấu hình kernel, chỉ được sử dụng để gỡ lỗi
    - `System.map` chứa bảng ký hiệu kernel, chỉ được sử dụng để gỡ lỗi

*Mỗi tệp này có một phiên bản kernel được gắn vào tên của nó.*
### 12. /lib - Thư viện hệ thống
Chứa các thư viện hỗ trợ cho các file thực thi trong /bin và /sbin. Các thư viện này thường có tên bắt đầu bằng ld* hoặc lib*.so.*. Ví dụ như `ld-2.11.1.so` hay `libncurses.so.5.7`
### 13. /opt - Các ứng dụng phụ tùy chọn
- Tên thư mục này nghĩa là optional (tùy chọn), nó chứa các ứng dụng thêm vào từ các nhà cung cấp độc lập khác. Các ứng dụng này có thể được cài ở `/opt` hoặc một thư mục con của `/opt`
### 14. /mnt - Thư mục để mount
- Đây là thư mục tạm để mount các file hệ thống. Ví dụ như `mount /dev/sda2 /mnt`
### 15. /media - Các thiết bị gắn có thể gỡ bỏ
- Thư mục tạm này chứa các thiết bị như CdRom `/media/cdrom`. floppy `/media/floopy` hay các phân vùng đĩa cứng `/media/Data` (hiểu như là ổ `D:/Data` trong Windows)
### 16. /srv - Dữ liệu của các dịch vụ khác
- Chứa dữ liệu liên quan đến các dịch vụ máy chủ như `/srv/svs`, chứa các dữ liệu liên quan đến CVS.


