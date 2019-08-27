# Hệ thống tập tin mạng

- NFS (the Network File System) là một trong những phương pháp được sử dụng để chia sẻ dữ liệu trên các hệ thống vật lý. Nhiều quản trị viên hệ thống gắn các thư mục `home` của người dùng từ xa trên một máy chủ để cấp cho họ quyền truy cập vào cùng một tệp và tệp cấu hình trên nhiều hệ thống máy khách. Điều này cho phép người dùng đăng nhập vào các máy khác nhau nhưng vẫn có quyền truy cập vào cùng các tệp và tài nguyên.

- Trên một bản phân phối Linux chung, trình nền máy chủ NFS thường được bắt đầu bằng lệnh `service nfs start`. Tệp `/etc/exports` này chứa các thư mục và quyền mà máy chủ lưu trữ sẵn sàng chia sẻ với các hệ thống khác qua NFS. Một mục trong tập tin này có thể là `/shared *(rw)`. Mục này cho phép thư mục  `/shared` được gắn kết bằng NFS với quyền đọc và ghi `(rw)` và được chia sẻ với các máy chủ khác trong cùng miền. Sau khi sửa đổi tệp `/etc/exports`, bạn có thể sử dụng lệnh `exportfs -av` để thông báo cho Linux về các thư mục bạn cho phép được gắn từ xa bằng NFS.

- Trên máy khách, nếu muốn hệ thống tệp từ xa được gắn tự động khi khởi động hệ thống, tệp /etc/fstab sẽ được sửa đổi để thực hiện việc này. Ví dụ, một mục trong tệp `/etc/fstab` của khách hàng: `<servername>:/shared /mnt/nfs/shared nfs defaults 0 0`. Bạn cũng có thể gắn hệ thống tập tin từ xa mà không cần khởi động lại hoặc dưới dạng gắn kết một lần bằng cách sử dụng trực tiếp lệnh `mount`.

## Thực hành

### Bước 1: Cài đặt `nfs` trên máy chủ

 `yum install -y nfs-utils nfs-utils-lib`

### Bước 2: Tạo thư mục chia sẻ chung

`[root@vqmanh ~]#  mkdir /var/shared`

### Bước 3: Chỉnh sửa file `/etc/exports`

```
[root@vqmanh ~]# vi /etc/exports
/var/shared 66.0.0.0/24(no_root_squash,no_all_squash,rw,sync)
```
- **Trong đó:**

    - `/var/shared` là thư mục dùng chung
    - `66.0.0.0/24` là dải địa chỉ IP của khách hàng

    - `rw` cho phép client đọc ghi với thư mục
    - `ro` quyền chỉ đọc với thư mục
    - `sync` đồng bộ hóa thư mục dùng chung
    - `root_squash` ngăn remote root users
    - `no_root_squash` cho phép remote root users

*Chú ý: Khi khai báo quyền truy cập của client ta cần viết liền*

### Bước 4: Khởi động dịch vụ

```
[root@vqmanh ~]# systemctl start rpcbind
[root@vqmanh ~]# systemctl start nfs-server
[root@vqmanh ~]# systemctl enable rpcbind
[root@vqmanh ~]# systemctl enable nfs-server
Created symlink from /etc/systemd/system/multi-user.target.wants/nfs-server.service to /usr/lib/systemd/system/nfs-server.service.
```
Và tắt firewalld: `systemctl stop firewalld`

### Bước 5: Trên máy client, cài dịch vụ `nfs`

`yum install -y nfs-utils nfs-utils-lib`

### Bước 6: Tạo thư mục, sau đó `mount` thư mục được chia sẻ

```
[root@vqmanh ~]# mkdir -p /mnt/nfs
[root@vqmanh ~]# mount 66.0.0.199:/var/shared /mnt/nfs
```
Các bạn có thể kiểm tra với lệnh 
```
[root@vqmanh ~]# showmount -e
Export list for vqmanh:
/var/shared 66.0.0.0/24
```

```
[root@thuctap ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   20G   20G   70M 100% /
devtmpfs                 899M     0  899M   0% /dev
tmpfs                    910M     0  910M   0% /dev/shm
tmpfs                    910M  9.6M  901M   2% /run
tmpfs                    910M     0  910M   0% /sys/fs/cgroup
/dev/sda1                509M  176M  334M  35% /boot
tmpfs                    182M     0  182M   0% /run/user/0
66.0.0.199:/var/shared    20G   20G   70M 100% /mnt/nfs
```