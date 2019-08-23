# TỔNG QUAN VỀ LVM

##  ĐỊNH NGHĨA

**1. Logical Volume Manager (LVM) là phương pháp quản lý các storage device, các logical volume dễ dàng ,linh hoạt hơn .Với kỹ thuật Logical Volume Manager (LVM) bạn có thể thay đổi kích thước mà không cần phải sửa lại partition table của OS.**

**2. Cấu trúc LVM**

<img src=https://imgur.com/mqY5kGJ.jpg>

**LVM phân các lớp trên các ổ cứng vật lý. Bao gồm các thành phần sau:**

- **Hard drives – Drives**
: Thiết bị lưu trữ dữ liệu, ví dụ như trong linux nó là /dev/sda

- **Partition**
: Partitions là các phân vùng của Hard drives, mỗi Hard drives có 4 partition, trong đó partition bao gồm 2 loại là primary partition và extended partition:

    - **Primary partition:** Phân vùng chính, có thể khởi động , mỗi đĩa cứng có thể có tối đa 4 phân vùng này.

    - **Extended partition**: Phân vùng mở rộng

**Physical Volumes:**
- Kí hiệu : pv...
- Là những thành phần cơ bản được sử dụng bởi LVM dể xây dựng lên các tầng cao hơn . Một Physical Volume không thể mở rộng ra ngoài phạm vi một ổ đĩa. Chúng ta có thể kết hợp nhiều Physical Volume thành Volume Groups

**Volume Group**

- 1 Volume group bao gồm nhiều các Physical Volume gộp thành. Các LE được cấp bởi các PE của các Physical Volume.

- Volume Group được sử dụng để tạo ra các Logical Volume, trong đó người dùng có thể tạo, thay đổi kích thước, lưu trữ, gỡ bỏ và sử dụng.


***Lưu ý:***
- Extent trên Physical Volume được gọi là Physical extent.

- Extent trên Logical Volume được gọi là các Logical extent.

**Logical Volumes:**

- Kí hiệu: lv...(có thể là lvm... trong hệ thống)

- Một Logical Volume là một ánh xạ mà LVM duy trì giữa các Logical và Physical extent.
- Có chức năng là các phân vùng trong ổ cứng vật lý, nhưng có thể linh hoạt hơn. Là thành phần chính để người dùng và các phần mềm tương tác.

# Logical Volume Manager layout

- Logical Volume(s): /dev/fileserver/share, /dev/fileserver/backup, /dev/fileserver/media
- Volume Group(s): fileserver
- Physical Volume(s): /dev/sdb1, /dev/sdc1, /dev/sdd1, /dev/sdc1


Trên môi trường VMware, các bạn có thể add thêm ổ cứng. Sau đó `reboot` lại hệ thống

<img src=https://imgur.com/b8rb2ZK.jpg>

Sau đó kiểm tra với `lsblk`

<img src=https://imgur.com/OUdNmjs.jpg>

Khi add thêm ổ, hệ thống sẽ xuất hiện `sdb` đây chính là ổ chúng ta mới thêm.

## Create a LVM layout

**1. Tạo các partition** 

`fdisk /dev/sdb`

```
[root@vqmanh ~]#  fdisk /dev/sdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0xf25481ad.

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-41943039, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-41943039, default 41943039): +5G
Partition 1 of type Linux and of size 5 GiB is set

Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

**Trong đó:**

- Command`n` để bắt đầu tạo partition.

- Chọn `p` để tạo partition primary

- Chọn `1` để đặt partition number

- First sector để mặc định

- Last sector để `+1G` (có các tùy chọn KB, MB ,GB)

- Sau khi hoàn thành các bước trên nhấn `t` để đổi định dạng partition

- Chọn `8e` để thay đổi định dạng partition

- Sau khi hoàn thành ấn `w` để lưu và thoát

Kiểm tra:

<img src=https://imgur.com/dELSoQf.jpg>

**2. Tạo Physical Volum**

 ```
 [root@vqmanh ~]# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.
```

Kiểm tra bằng lệnh pvs hoặc pvdisplay xem các physical volume đã được tạo chưa:

<img src=https://imgur.com/KFNKXMp.jpg>

**3. Tạo Volume group**

*Để tạo 1 `Volume group` các bạn có thể add thêm 1 ổ `sdc`. Tạo `partition`, `physical volum` như với ổ `sdb`.*

Sau khi tạo các Physical Volume ta gộp các PV đó thành 1 Volume Group bằng lệnh sau:

`vgcreate tên_vg /dev/sdb1 /dev/sdc1 `

Dùng các lệnh `vgs` hoặc `vgdisplay` để kiểm tra:

<img src=https://imgur.com/2Zka6jc.jpg>

**3. Tạo Logical Volume**

Từ một Volume group , ta tạo các Logical Volume để sử dụng bằng lệnh sau: 

` lvcreate -L size_lv -n tên_lv tên_vg `

**Trong đó:**

- `-L`: Chỉ ra dung lượng của logical volume
- `-n`: Chỉ ra tên của logical volume

Kiểm tra bằng lệnh `lvs` hoặc `lvdisplay`

<img src=https://imgur.com/2hCrO71.jpg>

**4. Định dạng Logical Volume**

Format các `Logical Volume` thành các định dạng ext2,ext3,ext4,...:

`mkfs -t ext4 /dev/tên_vg/tên_lv`

```
[root@vqmanh ~]# mkfs -t ext4 /dev/vg-vqmanh/lv-vqmanh
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
196608 inodes, 786432 blocks
39321 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=805306368
24 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```
**5. Mount và sử dụng:**

Bước 1: Ta tạo một thư mục để mount `Logical Volume`:

`mkdir lvm_vqmanh`

Bước 2: Tiến hành mount `logical volume` *lv-vqmanh* vào thư mục *lvm_vqmanh*

```
[root@vqmanh ~]# mkdir lvm_vqmanh
[root@vqmanh ~]# mount /dev/vg-vqmanh/lv-vqmanh lvm_vqmanh
```

Bước 3: Kiểm tra

```
[root@vqmanh ~]# df -h
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/centos-root             20G   20G   72M 100% /
devtmpfs                           899M     0  899M   0% /dev
tmpfs                              910M     0  910M   0% /dev/shm
tmpfs                              910M  9.6M  901M   2% /run
tmpfs                              910M     0  910M   0% /sys/fs/cgroup
/dev/sda1                          509M  176M  334M  35% /boot
tmpfs                              182M     0  182M   0% /run/user/0
/dev/mapper/vg--vqmanh-lv--vqmanh  2.9G  9.0M  2.8G   1% /root/lvm_vqmanh
```
**6. Thay đổi dung lượng Volume Group**

***Việc thay đổi kích thước của `Volume Group` chính là việc nhóm thêm `Physical Volume` hay thu hồi `Physical Volume` ra khỏi `Volume Group`***

Các bạn add thêm partition như bước trên, sau đó gõ lệnh `partprobe` để cập nhật.

Ta thêm 1 partition vào Volume Group như sau:

```
[root@vqmanh ~]# vgextend /dev/vg-vqmanh /dev/sdb2
  Volume group "vg-vqmanh" successfully extended
```
Chúng ta có thể xóa 1 Physical Volume ra khỏi Volume Group như sau:

````
[root@vqmanh ~]# vgreduce /dev/vg-vqmanh /dev/sdb2
  Removed "/dev/sdb2" from volume group "vg-vqmanh"
````

**7. Thay đổi dung lượng Logical Volume**


*Lệnh `vgdisplay` ta có thể thấy volume group còn trống và có thể resizable được.*

<img src=https://imgur.com/0ihX0tp.jpg>

Tăng kích thước logical volume bằng lệnh sau với `-L` là lựa chọn dung lượng:

<img src=https://imgur.com/CT4inRw.jpg>

Sau khi tăng kích thước cho Logical Volume thì Logical Volume đã được tăng nhưng file system trên volume này vẫn chưa thay đổi, bạn phải sử dụng lệnh sau để thay đổi:

```
[root@vqmanh ~]#  resize2fs /dev/vg-vqmanh/lv-vqmanh
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/vg-vqmanh/lv-vqmanh is mounted on /root/lvm_vqmanh; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/vg-vqmanh/lv-vqmanh is now 799744 blocks long.
```
Sau đó tiến hành format lại Logical Volume:

`mkfs.ext4 /dev/vg-vqmanh/lv-vqmanh`