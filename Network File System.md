# NFS (Network File System)

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

<a name = "A"></a>
## A. Tổng quan về NFS

- NFS là một trong những phương pháp được sử dụng để chia sẻ dữ liệu trên các hệ thống vật lý. Được phát triển bởi SunMicrosystems và năm 1984, cho phép người dùng xem, tùy chọn lưu trữ và cập nhật trên máy tính từ xa.
- NFS là hệ thống cung cấp dịch vụ chia sẻ file phổ biến trong hệ thống mạng Linux và Unix.
- Dịch vụ NFS cho phép các NFS client mount một phân vùng của NFS server như phân vùng cục bộ của nó.
- Dịch vụ NFS không được security nhiều, vì vậy cần thiết phải tin tưởng các client được permit mount các phân vùng của NFS server.

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

<a name = "A"></a>
### Các trường hợp dùng NFS

- Ứng dụng hỗ trợ: VDI, Oracle, VMware ESXi, SAS Grid, SAP HANA, TIBCO, OpenStack, Docker, etc
- Các khách hàng lớn.
- Đơn giản, dễ quản lý.
- Không cần client OS file system.
- Dễ dàng mở rộng, thu hồi.
- Dễ dàng di chuyển các storage.
- Chạy trên Ethernet.
- Hiệu suất lớn, độ trễ thấp. Hiệu suất tốt hơn iSCSl trong vài trường hợp.

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
root_squash|	ngăn remote root users
no_root_squash	|cho phép remote root users
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
### B2. IP Planing

Tên máy ảo|	Hệ điều hành|	IP address|	Subnet mask|	Default gateway
------|-----|----------|--------|-----
NFS Server| CentOS7|66.0.0.199|/24|66.0.0.1
Client|CentOS7|66.0.0.200|/24|66.0.0.1

<a name = "B3"></a>
### B3. Triển khai 