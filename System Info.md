# System Info

## Linux release and system info

Quản trị viên hệ thống Linux cần lấy thông tin từ hệ thống. Dưới đây là một số lệnh hữu ích:

### Phát hành và phân phối Linux
```
[root@pak ~]# cat /etc/*release
CentOS Linux release 7.6.1810 (Core)
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

CentOS Linux release 7.6.1810 (Core)
CentOS Linux release 7.6.1810 (Core)
```

### Kernel version

```
[root@pak ~]# uname -r
3.10.0-957.21.3.el7.x86_64
```

### Thông tin bộ nhớ
```
[root@pak ~]# head /proc/meminfo
MemTotal:        1863248 kB
MemFree:          830328 kB
MemAvailable:    1496856 kB
Buffers:          527428 kB
Cached:           274948 kB
SwapCached:            0 kB
Active:           102840 kB
Inactive:         758712 kB
Active(anon):      68768 kB
Inactive(anon):     9308 kB
```

### Hệ thống tập tin

```
[root@pak ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   12G  1.4G   11G  13% /
devtmpfs                 898M     0  898M   0% /dev
tmpfs                    910M     0  910M   0% /dev/shm
tmpfs                    910M  9.6M  901M   2% /run
tmpfs                    910M     0  910M   0% /sys/fs/cgroup
/dev/sda1                509M  178M  331M  35% /boot
tmpfs                    182M     0  182M   0% /run/user/0
```

### Số lượng CPU
```
[root@pak ~]# cat /proc/cpuinfo | grep model | uniq -c
      1 model           : 58
      1 model name      : Intel(R) Core(TM) i7-3540M CPU @ 3.00GHz
    
```
### Hệ thống tập tin Proc

/proc hệ thống tập tin chứa các tệp ảo chỉ tồn tại trong bộ nhớ. Hệ thống tập tin này chứa các tập tin và thư mục bắt chước cấu trúc kernel và thông tin cấu hình. Nó không chứa các tệp thực nhưng thông tin hệ thống thời gian chạy (ví dụ: bộ nhớ hệ thống, thiết bị được gắn, cấu hình phần cứng, v.v.). Một số tệp quan trọng trong /proc là:

```
/proc/cpuinfo
/proc/interrupts
/proc/meminfo
/proc/mounts
/proc/partitions
/proc/version
/proc/<process-id-#>
/proc/sys
```
*Hệ thống tập tin Proc rất hữu ích vì thông tin mà nó báo cáo chỉ được thu thập khi cần thiết và không bao giờ cần lưu trữ trên đĩa.*

### Hostname

```
[root@pak proc]# cat /etc/hostname
pak.pop
```
**Đặt tên máy chủ mới**

`hostnamectl set-hostname tên_mới`

VD: hostnamectl set-hostname vqmanh.it

Sau đó reboot rồi sử dụng `hostnamectl` để kiểm tra

```
[root@vqmanh ~]# hostnamectl
   Static hostname: vqmanh.it
         Icon name: computer-vm
           Chassis: vm
        Machine ID: ce9ee67c86534b428acfb8ea4be08df0
           Boot ID: 57bef78cc99e4dfdbe338413abeecc3e
    Virtualization: vmware
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-957.21.3.el7.x86_64
      Architecture: x86-64
```

## Quản lý Processes trong Linux sử dụng Command Line

###  Xem processes trong Linux

- PID – Process ID. Mỗi Process có 5 ký tự số. Những số này có thể hết (hết số) và bắt đầu lại, nhưng tại bất kỳ thời điểm nào, không có hơn 1 PID trong hệ thống.
- PPID – Process Parent ID. ID của process mà khởi động process này.
- 2 commands phổ biến nhất được dùng là xem processes là `top` và `ps`. Khác biệt là top được dùng để tương tác nhiều còn ps được dùng trong scripts, kết hợp với các lệnh bash khác hoặc tương tự.

**`top` – command `top` là đơn giản và phổ biến nhất để hiển thị những process chiếm nhiều tài nguyên máy tính nhất.  Khi thực hiện command  top trong terminal, chúng ta sẽ thấy cửa sổ tương tự như sau:**

<img src=https://imgur.com/Kbm3Cdy.jpg>

`top` là ứng dụng, sau khi thực hiện lệnh, một layout hiện lên và danh sách process đang liên tục được cập nhật mỗi giây. Layout mới này có thể tương tác với bàn phím.
 Ví dụ:

- h or ? – Hiện cửa sổ help với các câu lệnh hữu dụng
space – Nhấn space trên bàn phím sẽ cập nhật bảng process ngay lập tức thay vì phải chờ vài giây.
- f – Thêm trường mới để hiển thị layout hoặc xóa những field nhất định vì vậy bạn sẽ không thấy nó hiển thị. .
- q – thoát ứng dụng top hoặc mở thêm cửa sổ mới của ứng dụng top. Ví dụ,sau khi dùng feature f.
- l – Bật/tắt thông tin trung bình tải và thời gian uptime
- m – Bật/tắt thông tin bộ nhớ
- P (Shift + p) – Sắp xếp process bằng CPU usage.
- s – Đổi đột trễ giữa các lần refresh (Bạn sẽ được hỏi bao nhiêu giây).
**Với command `top`, bạn có thể dùng các tùy chọn sau, ví dụ:**

- -d delay – xác định độ trễ
- -n number – refresh trang bao nhiêu lần, sau đó thoát.
- -p pid – chỉ hiển thị và giám sát process với đúng process ID được chọn
- -q – refresh mà không có delay.

**Những lợi ích của việc sử dụng command  `top`**

- Hiển thị processes liên quan đến một user, bạn có thể dùng lệnh sau: top -u user

- Để xóa processes đang chạy, sau khi vào ứng dụng top, tìm pid của một process bạn muốn tắt, sau đó nhấn nút k (một command bằng keyboard khác). Bạn cũng sẽ được hỏi process ID (điền số ID bạn muốn tắt)
- Bạn có thể lưu lại cấu hình của top command hiện tại bằng shortcut Shift + W. Settings sẽ được lưu trong  /root/.toprc

**`ps` – Một command hữu ích khác để hiển thị processes trong Linux. Sau đây là một số tùy chọn thường được dùng với command `ps`:**

- -e – Hiện tất cả processes.
- -f – Toàn bộ danh sách được format.
- -r – Chỉ hiện những process đang chạy.
- -u – chỉ định username (hoặc nhiều usernames).
- –pid – lọc dựa trên PID.
- –ppid – lọc dựa trên parent PID.
- -C – lọc dựa trên tên và command.
- -o – Hiện thông tin liên quan đến từ khóa cách nhau bởi khoảng trắng hoặc dấu phẩy

**Sau đây là một số ví dụ bạn có thể dùng với command “ps”:**

- ps -ef – Liệt kê process đang chạy bây giờ. (Một command tương tự là ps aux)
- ps -f -u user1,user2 – Sẽ hiển thị tất cả process dựa rên UID (user id hoặc username).
- ps -f –pid id – Hiển thị tất cả processes dựa trên process ID (pid). Điền PID hoặc PPID thay vào chỗ id. Có thể được dùng với PPID để lọc process dựa trên parent ID.
- ps -C command/name – Lọc Processes dựa trên tên của nó hoặc command
- ps aux –sort=-pcpu,+pmem – Hiển thị process đang dùng nhiều tài nguyên nhất của CPU.
- ps -e -o pid,uname,pcpu,pmem,comm – Được dùng để lọc column được chỉ định.
- ps -e -o pid,comm,etime – Việc này sẽ hiển thị thời gian đã được dùng của process.