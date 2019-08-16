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