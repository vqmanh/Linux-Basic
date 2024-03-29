# NFS (Network File System)

<img src=https://imgur.com/XZTwsOa.png>

## Nội dung 

[A. Tổng quan về NFS](#A)

- [A1. Một số khái niệm](#A1)

- [A2. Cách hoạt động](#A2)

- [A3. Các phiên bản](#A3)

- [A4. Ưu nhược điểm](#A4)

    - [Các trường hợp dùng NFS](#TH)

- [A5.  File cấu hình, dịch vụ](#A5)

[B. LAB](#B)

- [B1. Mô hình](#B1)
- [B2. IP Planing](#B2)
- [B3. Triển khai](#B3)

[C. NFS 4 ACL Tool](#C)

<a name = "A"></a>
## A. Tổng quan về NFS

- NFS là một trong những phương pháp được sử dụng để chia sẻ dữ liệu trên các hệ thống vật lý. Được phát triển bởi SunMicrosystems và năm 1984, cho phép người dùng xem, tùy chọn lưu trữ và cập nhật trên máy tính từ xa.
- NFS là hệ thống cung cấp dịch vụ chia sẻ file phổ biến trong hệ thống mạng Linux và Unix.
- Dịch vụ NFS cho phép các NFS client mount một phân vùng của NFS server như phân vùng cục bộ của nó.
- Dịch vụ NFS không được security nhiều, vì vậy cần thiết phải tin tưởng các client được permit mount các phân vùng của NFS server.

- NFS hoạt động theo mô hình client/server. Một server đóng vai trò storage system, cho phép nhiều client kết nối tới để sử dụng dịch vụ.
- Client và Server sử dụng RPC (Remote Procedure Call) để giao tiếp với nhau.
- NFS sử dụng cổng 2049.
- Cho phép bạn quản lý không gian lưu trữ ở một nơi khác và ghi vào không gian lưu trữ này từ nhiều clients.
- NFS cung cấp một cách tương đối nhanh chóng và dễ dàng để truy cập vào các hệ thống từ xa qua mạng và hoạt động tốt trong các tình huống mà các tài nguyên chia sẻ sẽ được truy cập thường xuyên.
- Dung lượng file mà NFS cho phép client truy cập lớn hơn 2GB.

<a name = "A1"></a>
### A1. Một số khái niệm

- **Virtual filesystem (VFS)** là một kỹ thuật tự động chuyển hướng tất cả các truy xuất đến NFS-mount file một cách thông suốt trên Remote Server. 

- **Stateless Operation** là những chương trình đọc và ghi tập tin trên hệ thống tập tin cục bộ dựa vào hệ thống để theo dõi và ghi nhận vị trí đọc dữ liệu thông qua con trỏ địa chỉ pointer. 
- **Caching** trên NFS Client để lưu lại một số dữ liệu cần thiết vào hệ thống cục bộ. 
- **NFS Background Mounting** chỉ định khoảng thời gian đợi với tham số gb trong trường hợp Remote Server không tồn tại. 
- **Hard and Soft Mounts** có ý nghĩa rằng quá trình mount file luôn được tiến hành và quá trình sử dụng RPC để mount remote file system. 

- **RPC (Remote Procedure Call)** – Thủ tục gọi hàm từ xa là một kỹ thuật tiến bộ cho quá trình kết nối từ Client đến Server để sử dụng các ứng dụng và dịch vụ. RPC cho phép client có thể kết nối tới 1 dịch vụ sử dụng dynamic port nằm ở một máy tính khác.

<a name = "A2"></a>
### A2. Cách hoạt động

- Để truy cập dữ liệu được lưu trữ trên 1 máy chủ, server sẽ triển khai các quy trình nền NFS để cung cấp dữ liệu cho khách hàng. Quản trị viên máy chủ xác định những gì cần cung cấp và đảm bảo có thể nhận ra các máy khách được xác nhận.
Từ client, yêu cầu quyền truy cập vào dữ liệu đã xuất, bằng cách sử dụng lệnh `mount`.

- Server NFS tham chiếu tệp cấu hình `/etc/export` để xác định xem máy khách có được phép truy cập vào bất kỳ hệ thống nào không. Sau khi xác minh, tất cả hoạt động tập tin và thư mục được phép sử dụng trên Client. 

- NFS sử dụng thủ tục RPC (Remote Procedure Calls) để gửi, nhận yêu cầu giữa máy trạm và máy chủ nên dịch vụ portmap (dịch vụ quản lý yêu cầu RPC) cần phải được khởi động trước. 
- Để NFS hoạt động Linux cần khởi động ít nhất ba tiến trình sau: 
    - **Portmapper:** tiến trình này không làm việc trực tiếp với dịch vụ NFS mà tham gia quản lý các yêu cầu RPC từ máy trạm gửi đến.

    - **Mountd:** tiến trình này sẽ ánh xạ tập tin trên máy chủ tới thư mục mà máy trạm yêu cầu. Bỏ ánh xạ khi máy trạm phát ra lệnh umount.

    - **Nfs:** là tiến trình chính, thực thi nhiệm vụ của giao thức NFS, có nhiệm vụ cung cấp cho máy trạm các tập tin hoặc thư mục được yêu cầu.


<a name = "A3"></a>
### A3. Các phiên bản

- NFSv2: Tháng 3 năm 1989
    - Có thể sử dụng cả TCP và UDP qua mạng IP (cổng 2049)
    - Ban đầu chỉ hoạt động trên UDP
- NFSv3: Tháng 6 năm 1995
    - An toàn và mạnh mẽ hơn khi xử lý lỗi so với v2
    - Sử dụng cả TCP và UDP qua cổng 2049
    - Vẫn là phiên bản được sử dụng rộng rãi nhất
- NFSv4: Tháng 4 năm 2003
    - Hoạt động thông qua tường lửa và trên internet
    - Hỗ trợ ACL (Danh sách các câu lệnh chỉ ra loại packet nào được chấp nhận, hủy bỏ dựa vào địa chỉ nguồn, đích hoặc số port)
    - Sử dụng giao thức TCP là bắt buộc
- NFSv4.1: Tháng 1 năm 2010
    - Khả năng cung cấp quyền truy cập song song có thể mở rộng vào các tệp được phân phối giữa nhiều máy chủ
- NFSv4.2: Tháng 11 năm 2016
    - Sao chép và sao chép phía máy chủ
    - Một lợi thế lớn của NFSv4 so với các phiên bản trước đó là chỉ có một cổng IP được sử dụng để chạy dịch vụ, giúp đơn giản hóa việc sử dụng giao thức trên tường lửa.

- NFSv2 và v3 dựa trên RPC ( Remote Procedure Call) , RPC được điểu khiển bởi portmap .

- Tất cả các version của NFS sử dụng TCP , nhưng NFSv2 và v3 có thể dùng UDP để thiết lập kết nối stateless giữa client và server.

- NFSv4 không tương tác với portmapper , rpc.mountd , rpc.lockd , rpc.statd vì những thứ đấy được chuyển vào kernal .NFSv4 listen các request tại well known TCP port 2049


<a name = "A4"></a>
### **A4. Ưu nhược điểm**
- **Ưu điểm:**

    - NFS là 1 giải pháp chi phí thấp để chia sẻ tệp mạng.
    - Dễ cài đặt vì nó sử dụng cơ sở hạ tầng IP hiện có.
    - Cho phép quản lý trung tâm, giảm nhu cầu thêm phần mềm cũ và dụng lượng đĩa trên các hệ thống người dùng cá nhân.
- **Nhược điểm:**

    - NFS vốn không an toàn, chỉ nên sử dụng trên 1 mạng đáng tin cậy sau Firewall.
    - NFS bị chậm trong khi lưu lượng mạng lớn.
    - Client và server tin tưởng lần nhau vô điều kiện.
    - Tên máy chủ có thể là giả mạo (tự xưng là máy khác).

<a name = "TH"></a>
### Các trường hợp dùng NFS

- Ứng dụng hỗ trợ: VDI, Oracle, VMware ESXi, SAS Grid, SAP HANA, TIBCO, OpenStack, Docker, etc
- Các khách hàng lớn.
- Đơn giản, dễ quản lý.
- Không cần client OS file system.
- Dễ dàng mở rộng, thu hồi.
- Dễ dàng di chuyển các storage.
- Chạy trên Ethernet.
- Hiệu suất lớn, độ trễ thấp. Hiệu suất tốt hơn iSCSl trong vài trường hợp.

**Trong quá trình vận hành có thể xảy ra một số trường hợp sau:**

- Có 2 client đồng thời mount cùng một thư mục trên server: cả 2 đều có thể đồng thời chỉnh sửa cùng một file => hệ thống sẽ thông báo cho client sử dụng sau biết rằng có một client khác đang chỉnh sửa file. Nếu client này thực hiện sửa thì file sẽ lưu lại theo client nào thực hiện lưu cuối cùng.
- NFS server bị shutdown hoặc dịch vụ tắt. Client sẽ bị treo máy và chờ đến khi được kết nối trở lại. Việc kết nối được thực hiện ngầm, trong suốt với người dùng.



<a name = "A5"></a>
### A5.  File cấu hình, dịch vụ

***Có ba tập tin cấu hình chính, bạn sẽ cần phải chỉnh sửa để thiết lập một máy chủ NFS: `/etc/exports`, `/etc/hosts.allow` và `/etc/hosts.deny`***

#### 1. File `/etc/export`

*Các bạn có thể sử dụng lệnh `vi /etc/export` để chỉnh sửa file*

Cú pháp cấu hình trong file:

`dir host1(options) host2(options) hostN(options) …`

Trong đó:

- `dir` : thư mục hoặc file system muốn chia sẻ.
- `host` : một hoặc nhiều host được cho phép mount dir. Có thể được định nghĩa là một tên, một nhóm sử dụng ký tự, * hoặc một nhóm sử dụng 1 dải địa chỉ mạng/subnetmask...
- `options` : định nghĩa 1 hoặc nhiều options khi mount.

Options|	Description
------|---------
rw	|quyền đọc và viết
ro	|quyền chỉ đọc
noaccess|	cấm truy cập vào các thư mục cấp con của thư mục được chia sẻ
sync	|đồng bộ hóa thư mục dùng chung
root_squash|	ko cho đặc quyền root
no_root_squash	|cho phép đặc quyền root
async	|Tùy chọn này cho phép máy chủ NFS vi phạm giao thức NFS và trả lời các yêu cầu trước khi bất kỳ thay đổi nào được thực hiện bởi yêu cầu đó đã được cam kết lưu trữ ổn định
secure	|Tùy chọn này yêu cầu các yêu cầu bắt nguồn trên một cổng Internet nhỏ hơn IPPORT_RESERVED (1024). (Mặc định)
insecure|	Tùy chọn này chấp nhận tất cả các cổng
wdelay	|Trì hoãn cam kết một yêu cầu ghi vào đĩa nếu nó nghi ngờ rằng một yêu cầu ghi liên quan khác có thể đang được tiến hành hoặc có thể đến sớm. (Mặc định)
subtree_check|	Tùy chọn này kiểm tra cây con
all_squash	|Ánh xạ tất cả các uids và gids cho người dùng ẩn danh. Hữu ích cho NFS xuất các thư mục FTP công cộng, thư mục bộ đệm tin tức

Ví dụ 1 file cấu hình mẫu:
```
/usr/local *.vqmanh.vn(ro) 
/home 66.0.0.0/24(rw) 
/var/tmp 66.0.0.199(rw) 66.0.0.200(rw)
```

- Dòng thứ nhất: Cho phép tất cả các host với tên miền định dạng “somehost”.vqmanh.vn được mount thư mục /usr/local với quyền chỉ đọc.
- Dòng thứ hai: Cho phép bất kỳ host nào có địa chỉ IP thuộc subnet 66.0.0.0/24 được mount thư mục /home với quyền đọc và ghi.
- Dòng thứ ba: Cho phép 2 host được mount thư mục /var/tmp với quyền đọc và ghi.

#### 2. File `/etc/hosts.allow` và `/etc/hosts.deny`

Hai tập tin này định nghĩa các quy tắt(luật) truy cập vào hệ thống ở tầng ứng dụng mạng. Dựa trên điều khiển TCPWappers và  là một dạng tường lửa ở tầng ứng dụng.

Thứ tự ưu tiên của /etc/hosts.allow trước, rồi đến /etc/hosts.deny. Nghĩa là nếu có trong hosts.allow thì không cần tìm trong hosts.deny nữa.

Cú pháp trong hosts.allow và hosts.deny như sau:

`deamon: client` 

Trong đó:
- `deamon`: là dịch vụ cần áp đặt luật. Nếu để là ALL sẽ áp dụng cho mọi dịch vụ.
- `client`: là địa chỉ ip nguồn, host nguồn

***Chú ý:***
- Nếu có nhiều client ta sử dụng dấu `‘,’` để ngăn cách
- Nếu muốn áp đặt cho một lớp mạng ta khai báo lớp mạng và kết thúc với dấu `‘.’`

Ví dụ:

`portmap: 66.0.0. , 66.0.0.5`

`sshd: 192.168.1. , 192.168.3.2`


Ví dụ:
 Chặn mọi truy cập từ client có IP 192.168.10.10

`ALL: 192.168.10.10`

Ví dụ:
 Chặn tất cả client truy cập vào vsftp

`vsftpd: ALL`

- Kiểm tra file host.allow – nếu client phù hợp với 1 quy tắc được liệt kê tại đây thì nó có quyền truy cập.
- Nếu client không phù hợp với 1 mục trong host.allow server chuyển sang kiểm tra trong host.deny để xem thử client có phù hợp với 1 quy tắc được liệt kê trong đó hay không (host.deny). Nếu phù hợp thì client bị từ chối truy cập.
- Nếu client phù hợp với các quy tắc không được liệt kê trong cả 2 file thì nó sẽ được quyền truy cập.



#### 3. Các dịch vụ có liên quan

Để sử dụng dịch vụ NFS, cần có các daemon (dịch vụ chạy ngầm trên hệ thống) sau:

- **Portmap**: Quản lý các kết nối, dịch vụ chạy trên port 2049 và 111 ở cả server và client.
- **NFS**: Khởi động các tiến trình RPC (Remote Procedure Call) khi được yêu cầu để phục vụ cho chia sẻ file, dịch vụ chỉ chạy trên server.
- **NFS lock**: Sử dụng cho client khóa các file trên NFS server thông qua RPC.



#### 4. Khởi động portmapper

- NFS phụ thuộc vào tiến trình ngầm quản lý các kết nối (`portmap` hoặc `rpc.portmap`), chúng cần phải được khởi động trước.

- Nó nên được đặt tại `/sbin` nhưng đôi khi trong `/usr/sbin`. Hầu hết các bản phân phối linux gần đây đều khởi động dịch vụ này trong kịch bản khởi động (boot scripts – tự khởi động khi server khởi động) nhưng vẩn phải đảm bảo nó được khởi động đầu tiên trước khi bạn làm việc với NFS (chỉ cần gõ lệnh `netstat -anp |grep portmap` để kiểm tra).

#### 5. Các tiến trình ngầm

Dịch vụ NFS được hỗ trợ bởi 5 tiến trình ngầm:

- **rpc.nfsd**: thực hiện hầu hết mọi công việc.
- **rpc.lockd and rpc.statd**: quản lý việc khóa các file.
- **rpc.mountd**: quản lý các yêu cầu gắn kết lúc ban đầu.
- **rpc.rquotad**: quản lý các hạn mức truy cập file của người sử dụng trên server được truy xuất.
- **lockd**: được gọi theo yêu cầu của nfsd. Vì thế bạn cũng không cần quan tâm lắm tới việc khởi động nó.
- **statd**: thì cần phải được khởi động riêng.


<a name = "B"></a>
## B. LAB
<a name = "B1"></a>
### B1. Mô hình

<img src=https://imgur.com/GaTE1sc.jpg>

<a name = "B2"></a>
### B2. IP Planning

Tên máy ảo|	Hệ điều hành|	IP address|	Subnet mask|	Default gateway
------|-----|----------|--------|-----
NFS Server| CentOS7|66.0.0.199|/24|66.0.0.1
Client|CentOS7|66.0.0.200|/24|66.0.0.1

<a name = "B3"></a>
### B3. Triển khai 

**Trên NFS Server**

**Bước 1: Cài gói `nfs-until`**

`yum install nfs-utils -y`

**Bước 2: Tạo thư mục chia sẻ**

`[root@vqmanh ~]# mkdir /datachung`

**Bước 3: Sửa file /etc/exports**

`[root@vqmanh ~]# echo "/datachung 66.0.0.200(rw,no_root_squash)" >> /etc/exports`

*Ở đây mình cho  quyền đọc ghi và remote root user*

**Bước 4: Khởi động dịch vụ**

```
[root@vqmanh ~]# systemctl start rpcbind
[root@vqmanh ~]# systemctl start nfs-server
[root@vqmanh ~]# systemctl enable rpcbind
[root@vqmanh ~]# systemctl enable nfs-server
```

**Để kiểm tra các port sử dụng bởi NFS:**

`rpcinfo -p`

<img src=https://imgur.com/9ik3b1N.jpg>

- Port của dịch vụ Portmapper là 111

- Port của dịch vụ NFS là 2049   

*Chú ý: Nếu thay đổi trong `/etc/exports`, các thay đổi đó có thể chưa có hiệu lực ngay lập tức, bạn phải thực thi lệnh **exportfs -ra** để bắt `nfs` cập nhật lại nội dung file `/etc/exports`*

**Bước 5: Cấu hình firewall để NFS client được phép truy cập**

```
[root@vqmanh ~]# firewall-cmd --permanent --add-service=nfs
success
[root@vqmanh ~]# firewall-cmd --permanent --add-service=mountd
success
[root@vqmanh ~]# firewall-cmd --permanent --add-service=rpc-bind
success
[root@vqmanh ~]# firewall-cmd --permanent --add-port=2049/tcp
success
[root@vqmanh ~]# firewall-cmd --permanent --add-port=2049/udp
success
[root@vqmanh ~]# firewall-cmd --reload
success
```

**Kiểm tra mountpoint trên server**

```
[root@vqmanh ~]# showmount -e localhost
Export list for localhost:
/datachung 66.0.0.200
```

**Cấu hình NFS Client**

**Bước 1: Cài 2 packet nfs-utils và nfs-utils-lib**

`yum install nfs-utils nfs-utils-lib -y`

**Kiểm tra mountpoint trên server từ client**

```
[root@vqmanh ~]# showmount -e 66.0.0.199
Export list for 66.0.0.199:
/datachung 66.0.0.200
```
**Bước 2: Mount thư mục được chia sẻ vào thư mục local**

Cách 1: Mount mềm

 Cú pháp: `mount IP_server:/Thư_mục_chia_sẻ Thư_mục_trên_client`

  `[root@vqmanh ~]# mount 66.0.0.199:/datachung /mnt/`



**NFS có 2 chế độ mount:**

- Mount cứng là ghi trực tiếp vào file /etc/fstab

- Mount mềm là mount bằng lệnh thông thường và bị mất khi máy tính được khởi động lại

Cách 2: Mount cứng

`echo "66.0.0.199:/datachung /mnt nfs rw,sync,hard,intr 0 0" >> /etc/fstab`

Kiểm tra:

```
[root@vqmanh ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   27G  1.2G   26G   5% /
devtmpfs                 899M     0  899M   0% /dev
tmpfs                    910M     0  910M   0% /dev/shm
tmpfs                    910M  9.6M  901M   2% /run
tmpfs                    910M     0  910M   0% /sys/fs/cgroup
/dev/sda1               1014M  182M  833M  18% /boot
tmpfs                    182M     0  182M   0% /run/user/0
66.0.0.199:/datachung     20G   20G   68M 100% /mnt
```

Vì chúng ta cho quyền client có thể đọc ghi nên ta có thể thử tạo 1 file.

VD: 

```
[root@vqmanh mnt]# touch vqmanh.txt
[root@vqmanh mnt]# echo "cai dat nfs" > vqmanh.txt
```

Sau đó bạn có thể kiểm tra bên NFS Server.

### Đối với Client là Window

**Mở dịch vụ NFS theo hình dưới đây:**

<img src=https://imgur.com/7Nyz4ir.jpg>

**Sau đó truy cập CMD và gõ lệnh**

***Cú pháp: `mount [NFS server's hostname hoặc IP NFS server]:/[thư mục được share] [Ổ muốn mount]:\`***

<img src=https://imgur.com/3f0Ypx0.jpg>

**Sau đó kiểm tra**

<img src=https://imgur.com/5fR5BAb.jpg>

Sử dụng lệnh `nfsstat` hiển thị số liệu thống kê lưu về hoạt động của máy khách và máy chủ NFS.

***Lưu ý: Nếu chúng ta có 2 client cùng truy cập vào 1 file để sửa đổi thì file đó sẽ lưu lại của người có thao tác lưu cuối cùng.***

<a name = "C"></a>
## C. NFS 4 ACL Tool

### Tổng quan về điều khiển truy cập file chia sẻ với NFSv4 ACLs

- Danh sách điều khiển truy cập POSIX ACL cung cấp định nghĩa mịn hơn về quyền truy cập file và thư mục so với cách phân loại đơn giản user/group/other mà vẫn thường dùng với lệnh chmod. Hơn thế nữa, NFSv4 ACLs mịn hơn POSIX ACL, dùng cho điều khiển truy cập file chia sẻ của hệ thống file mạng NFSv4.

- Một ACL(Access Control List) là một danh sách phép kết hợp với một file hoặc thư mục, bao gồm một hay nhiều mục điều khiển truy cập (ACEs – Access Control Entries). Một NFSv4 ACL được kí hiệu là `acl_spec`, bao gồm các mục NFSv4 ACE được kí hiệu là `ace_spec`. Các `ace_spec` trong `acl_spec` ngăn cách nhau bởi dấu phẩy (,) hoặc tab, nhưng thường được soạn thảo mỗi `ace_spec` trên một dòng. Một `ace_spec` gồm có 4 trường cách nhau bởi dấu hai chấm (:)

- Server không nhất thiết hỗ trợ đầy đủ các thuộc tính NFSv4 ACL. Mặc dù khách đặt các thuộc tính, server có trách nhiệm diễn dịch các yêu cầu và chỉ thực hiện các hoạt động quản trị tin tưởng nơi server. Nó chuyển đổi NFSv4 ACL sang POSIX ACL và loại bỏ các thuộc tính không hỗ trợ. 


**Để xem ACL, sử dụng lệnh sau:**

`nfs4_getfacl file`

**Cài đặt Công cụ ACL NFS 4 trên NFS Client**

` yum -y install nfs4-acl-tools`

***Kiểm tra thư mục mount***

```
[root@vqmanh ~]# df -hT /mnt
Filesystem            Type  Size  Used Avail Use% Mounted on
66.0.0.199:/datachung nfs4   20G   20G   50M 100% /mnt
[root@vqmanh ~]# ll /mnt/
total 4
-rw-r--r--. 1 root root  0 Sep  3 15:47 haha.txt
-rw-r--r--. 1 root root 27 Sep  3 15:44 vqmanh.txt
````

**Hiển thị ACL của một tệp hoặc thư mục trên hệ thống tệp NFSv4**

```
[root@vqmanh ~]# nfs4_getfacl /mnt/vqmanh.txt

# file: /mnt/vqmanh.txt
A::OWNER@:rwatTcCy
A::GROUP@:rtcy
A::EVERYONE@:rtcy
[root@vqmanh ~]# nfs4_getfacl /mnt/haha.txt

# file: /mnt/haha.txt
A::OWNER@:rwatTcCy
A::GROUP@:rtcy
A::EVERYONE@:rtcy
```

**Mỗi mục có nghĩa như sau:**
-  **ACE** - 4 mục kiểm soát truy cập
    - **(ACE Type):(ACE Flags):(ACE Principal):(ACE Permissions)**
    = **kiểu:các_cờ:chủ_thể:các_quyền**

Description

ACE Type|   |	 
--------|---
A|	A = Allow : Cho-phép: cho phép `chủ_thể` thực hiện hành động với các_quyền
D|	D = Deny : Từ-chối: không cho phép `chủ_thể` thực hiện hành động với các_quyền
U|Kiểm-tra: ghi nhật ký (tùy thuộc hệ thống) bất kỳ nỗ lực truy cập bởi `chủ_thể` với `các_quyền`. Yêu cầu đi với một hoặc cả hai cờ `truy-cập-thành-công` và `truy-cập-thất-bại`.
L|Báo-động: tạo ra báo động hệ thống (tùy thuộc hệ thống) tại bất kỳ nỗ lực truy cập bởi `chủ_thể` với `các_quyền`. Yêu cầu đi với một hoặc cả hai cờ `truy-cập-thành-công` và `truy-cập-thất-bại`.`

- **A** biểu thị "Cho phép" có nghĩa là ACL này cho phép người dùng hoặc nhóm thực hiện các hành động yêu cầu quyền. Bất cứ điều gì không được phép rõ ràng đều bị từ chối theo mặc định.

- Lưu ý: **D** có thể biểu thị một Từ chối ACE. Mặc dù đây là một tùy chọn hợp lệ, nhưng loại ACE này không được đề xuất vì bất kỳ quyền nào không được cấp phép đều tự động bị từ chối có nghĩa là từ chối của ACE có thể là dư thừa và phức tạp.

**ACE Flags**

- Có 3 loại cờ: cờ nhóm (g), cờ kế thừa (d,f,n,i) và cờ quản trị (S,F). Kiểu Cho-phép hoặc Từ-chối có thể không có cờ, nhưng kiểu Kiểm-tra hoặc Báo-động phải đi với ít nhất một trong hai cờ truy-cập-thành-công và truy-cập-thất-bại.
- Các ACE được kế thừa từ ACL của thư mục cha vào lúc tạo file hay thư mục con. Theo đó, cờ kế thừa chỉ có thể được sử dụng trong các ACE trong ACL của một thư mục, và do đó bị tước bỏ khỏi các ACE kế thừa trong ACL của một file mới.

ACE Flags|   |	 
--------|---
g|Cờ nhóm: có thể sử dụng trong bất kỳ ACE, chỉ ra chủ_thể là một nhóm.
d|	Directory-Inherit : Cờ kế thừa: có thể sử dụng trong bất kỳ ACE thư mục, kế-thừa-thư-mục: thư mục con tạo mới sẽ kế thừa ACE.
f|	File-Inherit : kế-thừa-file: file tạo mới sẽ kế thừa ACE, ngoại trừ cờ kế thừa. Thư mục con tạo mới sẽ kế thừa ACE. Nếu kế-thừa-thư-mục không chỉ ra trong ACE cha, chỉ-kế-thừa sẽ được thêm tới ACE kế thừa. 
n|	No-Propogate-Inherit : không-truyền-kế-thừa: thư mục con tạo mới sẽ kế thừa ACE, ngoại trừ cờ kế thừa.
i|Inherit-Only : chỉ-kế-thừa: ACE không quan tâm tới việc kiểm tra quyền, nhưng nó di truyền được. Tuy nhiên, cờ chỉ-kế-thừa bị tước bỏ khỏi các ACE kế thừa.
S|truy-cập-thành-công: kích hoạt một báo động/kiểm tra khi chủ_thể được phép thực hiện một hành động được phủ bởi các_quyền.
F|truy-cập-thất-bại: kích hoạt một báo động/kiểm tra khi chủ_thể bị ngăn chặn thực hiện một hành động được phủ bởi các_quyền.



**ACE Principal**

- `chủ_thể` là người sử dụng hoặc nhóm, hoặc là một trong ba chủ_thể đặc biệt: OWNER@, GROUP@, EVERYONE@ mà tương ứng tương tự với user/group/other dùng trong lệnh chmod.

- User
    - VD: user@nfsdomain.org

- Special principals
    - OWNER@
    - GROUP@
    - EVERYONE@
- Group
    - Lưu ý: Khi `principal` là một group, bạn cần thêm cờ nhóm `g`
    - VD: **A:g:group@nfsdomain:rxtncy**

**ACE Permissions**

Quyền truy cập:

- Không chỉ có ba quyền đọc, viết, thực hiện như POSIX, NFSv4 ACLs có 13 quyền cho file và 14 quyền cho thư mục, `các_quyền` là một chuỗi kí tự, mỗi kí tự đại diện cho một quyền có ý nghĩa như sau:

ACE Permissions|Function
----------|---------
r	|Đọc dữ liệu của tập tin / Danh sách tập tin trong thư mục
w	|Ghi dữ liệu vào tệp / Tạo tệp mới trong thư mục
a|	Thêm dữ liệu (file) / tạo thư mục con (thư mục)
x	|Thực thi tập tin / Thay đổi thư mục
d	|Xóa tập tin hoặc thư mục
D	|Xóa các tập tin hoặc thư mục con trong thư mục
t	|Đọc thuộc tính: đọc thuộc tính của file / thư mục (là thuộc tính cơ bản của file / thư mục, được trình bày với ls -l, stat)
T	|ghi thuộc tính của tập tin / thư mục
n	|đọc các thuộc tính được đặt tên của tập tin / thư mục
N	|ghi các thuộc tính được đặt tên của tập tin / thư mục
c	|Đọc ACL: đọc NFSv4 ACL của file / thư mục
C	|Viết chủ: thay đổi chủ và nhóm của file / thư mục
o	|Thay đổi quyền sở hữu của tập tin / thư mục
y|Đồng bộ: cho phép khách sử dụng đồng bộ I/O với server
 
**ACE Permissions Aliases**

Trong trường hợp sử dụng đơn giản, chữ viết tắt có thể được sử dụng làm bí danh thể hiện phép một cách chung chung. Đó là các phép đọc ( R ), viết ( W ) và thực hiện ( X ) quen thuộc với các bit chế độ truy cập file của lệnh chmod.

Aliases| |   
------------|-------
R	|R = rntcy : Read
W	|W = watTNcCy : Write
X	|X = xtcy : Execute

### Sử dụng NFSv4 ACL

Để thiết lập một ACE, sử dụng lệnh này:

`nfs4_setfacl`

Đặt, soạn thảo NFSv4 ACL của file hoặc thư mục chia sẻ. Cách sử dụng như sau:

`nfs4_setfacl [OPTIONS] file`

- file đại diện cho file hoặc thư mục.

- Có một lệnh bổ sung là `nfs4_editfacl`, tương đương với `nfs4_setfacl -e`

### Commands


COMMAND	|FUNCTION
-----|---------
-a acl_spec [index]|thêm các ACE từ `acl_spec` tới ACL của file. Các ACE được chèn tại ví trí thứ index (mặc định là 1) của ACL của file.
-x acl_spec |xóa các ACE khớp từ `acl_spec`, hoặc xóa ACE thứ index từ ACL của file.
-A file [index]	|thêm các ACE từ `acl_spec` trong `acl_file` tới ACL của file. Các ACE được chèn tại ví trí thứ index (mặc định là 1) của ACL của file.
-X file 	|xóa các ACE khớp từ `acl_spec` trong `acl_file` từ ACL của file.
-s acl_spec	|đặt ACL thành `acl_spec` (thay thế ACL hiện tại)
-S file	|đặt ACL của file tới acl_spec trong `acl_file`.
-m from_ace to_ace	|sửa đổi ACL của file bằng cách thay `from_ace` với `to_ace`.
-e, --edit|soạn thảo ACL của file. Nếu có nhiều file được chỉ ra, trình soạn thảo sẽ được gọi lần lượt cho từng file.
--version|trình bày phiên bản chương trình và thoát ra.

### Options

OPTION|	NAME|	FUNCTION
-------|-------|-------
-R|	--recursive	|áp dụng đệ qui tới các file và thư mục con của thư mục. Theo sau các liên kết tượng trưng (symlinks) trên dòng lệnh và bỏ qua symlinks gặp phải trong khi đệ qui xuyên qua thư mục.
-L|	--logical|	trong liên hợp với `-R/--recursive`, đi luận lý theo tất cả symlinks.
-P|	--physical	|trong liên hợp với `-R/--recursive`, đi vật lý theo tất cả symlinks.
|--test||trình bày kết quả của lệnh nhưng không giữ thay đổi.
 



