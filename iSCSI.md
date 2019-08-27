# iSCSI

- iSCSI- Internet Small Computer System Interface được xem là một giao thức để truyền tải các SCSI qua mạng IP bằng giao thức TCP/IP. Từ đó iSCSI cho phép truy cập các khối dữ liệu (phân vùng, disk, hoặc thiết bị lưu trữ) trên server qua các lệnh SCSI và truyền tải dữ liệu trên hệ thống mạng(LAN/WAN).

- Nó hoạt động trên mô hình Client-Server. Ta có thể tưởng tượng thay vì ta đem 1 disk cắm trực tiếp vào 1 máy để chạy và khi cần di chuyển thì ta tháo ra và mang đến máy khác cắm vào. Làm như thế rất bất tiện. Với iSCSI thì ta không cần mất công như vậy mà vẫn có thể đáp ứng được điều đó. Ta vẫn có thể làm việc với disk trên server như các disk đó đang thực sự đang nằm trên máy của mình.
- Một thiết bị từ xa trên mạng được gọi là iSCSI Target, một máy khách kết nối với iSCSI Target được gọi là iSCSI Initiator.

### Một số thuật ngữ
* `Initiator`: Là máy client. Nó gửi yêu cầu truy cập đến server để truy cập vào khối dữ liệu.
* `Target`: đóng vai trò server, nơi lưu trữ dữ liệu. Mỗi máy target phải có một tên duy nhất để các `initiator` truy cập vào. Một `target` có một hoặc nhiều hơn 1 khối các thiết bị lưu trữ.
* `ALC` (Access Control List) danh sách điều khiển truy nhập là một hạn chế truy nhập bằng cách sử dụng nút IQN để xác thực quyền truy nhập cho Client.
* `Discovery` nó liên quan đến việc truy vấn server để tìm kiếm mục tiêu được cấu hình.
* `iqn`: là một quy định trong cách đăt tên cho cả `target` và `initiator`. Định dạng đặt tên như sau:
`iqn.năm-tháng.tên_miền:tên_phân_biệt`
Trong đó:
    * `iqn` biểu thị rằng tên này sẽ sử dụng tên miền làm mã định danh cho nó.  
    * `năm-tháng` là tháng đầu tiên mà tên miền được tạo.
    * `tên miền` là tên của server. 
    * `tên phân biệt` tên này được thêm vào để ta phân biệt với target khác trên máy này.
* `Login` điều này xác thực target hoặc LUN dể bắt đầu sử dụng để login vào từ máy client.
* `LUN`- Logical Unit Number. Gồm các thiết bị (disk, partition hoặc logical volume) được gắn kết với nhau và thông qua target. Một target có thể gồm nhiều LUN nhưng thông thường một target cung cấp một LUN.
* `node` là một iSCSI initiator hoặc iSCSI target được xác định bởi IQN của nó.
* `portal` là địa chỉ IP và port trên target và initiator để thiết lập kết nối.
### Một số loại bộ nhớ sao lưu (backstores).
* `Block` một khối thiết bị lưu trữ được xác định trên server. Nó có thể là disk, partition, logical volume, hoặc bất kỳ files thiết bị nào được chỉ ra trên server.
* `fileio` nó tạo ra một file có kích thước xác định trong hệ thống file của server.

### iSCSI Target Setup

**Cài đặt và cài đặt cấu hình khởi động cùng hệ thống**

```
yum -y install targetcli
systemctl enable target
systemctl start target
```

**Sau đó dùng lệnh `targetcli` để hiện dấu nhắc tương tác với iSCSI, dùng `ls` kiểm tra bố cục**
```
[root@vqmanh ~]# targetcli
targetcli shell version 2.1.fb46
Copyright 2011-2013 by Datera, Inc and others.
For help on commands, type 'help'.

/> ls
o- / ......................................................................................................................... [...]
  o- backstores .............................................................................................................. [...]
  | o- block .................................................................................................. [Storage Objects: 0]
  | o- fileio ................................................................................................. [Storage Objects: 0]
  | o- pscsi .................................................................................................. [Storage Objects: 0]
  | o- ramdisk ................................................................................................ [Storage Objects: 0]
  o- iscsi ............................................................................................................ [Targets: 0]
  o- loopback ......................................................................................................... [Targets: 0]
/>

```

**Tạo một kho lưu trữ**

- *`Backstores` cho phép hỗ trợ cho các phương thức lưu trữ đối tượng khác nhau trên máy cục bộ. Tạo một đối tượng lưu trữ xác định các tài nguyên mà kho lưu trữ sẽ sử dụng. Các `backstores` được hỗ trợ là: `block devices`, `files`, `pscsi` và `ramdisks`.*

- *Tạo một block(có thể là file nếu sử dụng kiểu fileio) và chỉ định cho nó dùng thiết bị lưu trữ nào (có thể là disk, partition hoặc là logical volume). Cú pháp `create tên_block thiêt_bị`* 

```
/> /backstores/block create name=block_storage dev=/dev/sdb
Created block storage object block_storage using /dev/sdb.
```

**Tạo iSCSI Target**

- *Tiếp theo tạo target - định dạng đặt tên như sau: `iqn.năm-tháng.tên_miền:tên_phân_biệt`. Tên miền ở đây là vqmanh.vn nhưng trong quy định về đặt tên thì tên miền phải được viết đảo ngược lại.*

```
/> iscsi/ create iqn.2019-08.vn.vqmanh:disk1
Created target iqn.2019-08.vn.vqmanh:disk1.
Created TPG 1.
Global pref auto_add_default_portal=true
Created default portal listening on all IPs (0.0.0.0), port 3260.
```

- *Theo mặc định, một cổng thông tin được tạo khi iSCSI Target được tạo nghe trên tất cả các địa chỉ IP (0.0.0.0) và cổng iSCSI mặc định 3260. Đảm bảo rằng 3260 không được sử dụng bởi một ứng dụng khác, nếu không thì chỉ định một cổng khác.*

**Cấu hình LUN**

```
/> /iscsi/iqn.2019-08.vn.vqmanh:disk1/tpg1/luns create /backstores/block/block_storage
Created LUN 0.
/> ls
o- / ......................................................................................................................... [...]
  o- backstores .............................................................................................................. [...]
  | o- block .................................................................................................. [Storage Objects: 1]
  | | o- block_storage ................................................................... [/dev/sdb (10.0GiB) write-thru activated]
  | |   o- alua ................................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ....................................................................... [ALUA state: Active/optimized]
  | o- fileio ................................................................................................. [Storage Objects: 0]
  | o- pscsi .................................................................................................. [Storage Objects: 0]
  | o- ramdisk ................................................................................................ [Storage Objects: 0]
  o- iscsi ............................................................................................................ [Targets: 1]
  | o- iqn.2019-08.vn.vqmanh:disk1 ....................................................................................... [TPGs: 1]
  |   o- tpg1 ............................................................................................... [no-gen-acls, no-auth]
  |     o- acls .......................................................................................................... [ACLs: 0]
  |     o- luns .......................................................................................................... [LUNs: 1]
  |     | o- lun0 .............................................................. [block/block_storage (/dev/sdb) (default_tg_pt_gp)]
  |     o- portals .................................................................................................... [Portals: 1]
  |       o- 0.0.0.0:3260 ..................................................................................................... [OK]
  o- loopback ......................................................................................................... [Targets: 0]

```

- *Tạo ACL để khi client trình bày đúng cái tên này thì nó mới được target chấp nhận kết nối. Khi tạo ACL này thì có 1 phần ko đổi là phần trước dấu hai chấm đây là phần tên máy nó cố định như ta đặt từ trước. Phần sau dấu 2 chấm là một mật khẩu riêng những client nào ta muốn cho truy cập vào thì ta mới cho biết.*

