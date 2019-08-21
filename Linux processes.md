# Linux processes

- Trong Linux Hệ điều hành xem mỗi đơn thể mã lệnh mà nó điều khiển là một tiến trình(process). Một chương trình có thể bao gồm nhiều tiến trình kết hợp với nhau.Các tiến trình hoạt động chia sẻ tốc độ xử lý của CPU,cùng dùng chung vùng nhớ và các tài nguyên hệ thống khác. Các tiến trình được điều phối xoay vòng bởi hệ điều hành. Một chương trình nếu mở rộng dần ra, sẽ có lúc cần phải tách ra thành nhiều tiến trình để xử lý những công việc độc lập với nhau. Thực tế trong Linux các lệnh là độc lập với nhau có khả năng kết hợp và truyền dự liệu cho nhau. Tiến trình của chúng ta hoạt động phải luôn ở trạng thái tôn trọng và sẵn sàng nhường quyền xử lý CPU cho các tiến trình khác khi hệ thống có yêu cầu. Nếu một tiến trình xây dựng không tốt sẽ gây ra lỗi sẽ làm treo các tiến trình khác hay thậm chí là phá vỡ hệ thống. 
- Tiến trình là một thực thể điều khiển đoạn mã lệnh có riêng một không gian địa chỉ, có ngăn xếp stack riêng rẽ, có bảng chứa thông số mô tả file được mở cùng tiến trình và đặc biệt có một định danh PID duy nhất trên hệ thống vào thời điểm tiến trình đang chạy. 
- Trong Linux, tiến trình được cấp không gian địa chỉ bộ nhớ phẳng là 4GB. Dữ liệu cảu tiến trình này không thể đọc và truy xuất được bởi các tiến trình khác. Hai tiến trình khác nhau không thể xâm phạm biến của nhau. Tuy nhiên nếu chúng ta muốn chia sẻ dữ liệu giữa 2 tiến trình, Linux cung cấp cho ta một vùng không gian địa chỉ chung để làm điều này.

Type	|Description
----------|---------
Interactive|Cần phải được bắt đầu bởi người dùng, tại một dòng lệnh hoặc thông qua giao diện đồ họa như biểu tượng hoặc lựa chọn menu.
Batch|Các quy trình tự động được lên lịch từ đó và sau đó ngắt kết nối khỏi thiết bị đầu cuối. Các tác vụ này được xếp hàng và hoạt động trên cơ sở FIFO (First In, First Out).
Daemons|Máy chủ xử lý chạy liên tục. Nhiều người được khởi chạy trong quá trình khởi động hệ thống và sau đó chờ người dùng hoặc yêu cầu hệ thống chỉ ra rằng dịch vụ của họ là bắt buộc.
Threads|Các quy trình nhẹ. Đây là các tác vụ chạy dưới quy trình chính, chia sẻ bộ nhớ và các tài nguyên khác, nhưng được hệ thống lên lịch và chạy trên cơ sở cá nhân.
Kernel Threads|Các tác vụ hạt nhân mà người dùng không bắt đầu cũng không chấm dứt và có ít quyền kiểm soát. Chúng có thể thực hiện các hành động như di chuyển một luồng từ CPU này sang CPU khác hoặc đảm bảo các hoạt động đầu vào / đầu ra vào đĩa được hoàn thành.

## Cách hoạt động của tiến trình

**Khi một tiến trình đang chạy từ dòng lệnh, chúng ta có thể nhấn phím Ctrl+z để đưa nó vào hoạt động phía hậu trường. Tiến trình cảu Linux có các trạng thái:**

- Đang chạy(running): đây là lúc tiến trình chiếm quyền xử lý CPU dùng tính toán hay thực hiện các công việc của mình.
- Chờ(waiting): tiến trình bị Hệ Điều Hành tước quyền xử lý CPU, và chờ đến lượt cấp phát khác.
- Tạm dừng(suspend): Hệ điều hành tạm dừng tiến trình. Tiến trình được đưa vào trạng thái sleep. Khi cần thiết và có nhu cầu hệ điều hành sẽ đánh thức hay nạp lại mã lệnh của tiến trình vào bộ nhớ. Cấp phát tài nguyên CPU để tiến trình tiếp tục hoạt động. Thực tế khi chúng ta đăng nhập vào hệ thống và tương tác trên dòng lệnh cùng là lúc chúng ta đang ở trong tiến trình shell của bash. Khi gọi một lệnh có nghĩa là chúng ta yêu cầu bash tạo thêm một tiến trình con thực thi khác. Lệnh `ps` và các option hệ thống sẽ liệt kê cho chúng ta thông tin về các tiến trình mà hệ điều hành đang kiểm soát.

## Chạy các tiến trình

- Lệnh `ps` cung cấp thông tin về tiến trình đang chạy. Nếu bạn muốn cập nhật lặp đi lặp lại trạng thái này, bạn có thể sử dụng lệnh top hoặc các biến thể thường được cài đặt như `htop` hoặc `atop` từ dòng lệnh. Các lệnh `ps` có nhiều lựa chọn để xác định chính xác những nhiệm vụ để kiểm tra, những thông tin nào để hiển thị về họ, và chính xác những gì định dạng đầu ra nên được sử dụng.
    

- Lệnh `pstree` hiển thị các tiến trình đang chạy trên hệ thống dưới dạng một sơ đồ cây thể hiện mối quan hệ giữa một quá trình và quá trình cha mẹ và toàn bộ các quá trình khác mà nó tạo ra. Các mục lặp lại của một quá trình không được hiển thị và các luồng được hiển thị trong dấu ngoặc nhọn. 

Cài đặt`yum install -y psmisc`

```
[root@vqmanh ~]# pstree
systemd─┬─NetworkManager───2*[{NetworkManager}]
        ├─VGAuthService
        ├─agetty
        ├─auditd───{auditd}
        ├─crond
        ├─dbus-daemon───{dbus-daemon}
        ├─firewalld───{firewalld}
        ├─irqbalance
        ├─lvmetad
        ├─master─┬─pickup
        │        └─qmgr
        ├─polkitd───6*[{polkitd}]
        ├─rsyslogd───2*[{rsyslogd}]
        ├─sshd─┬─sshd───bash───pstree
        │      └─sshd───sftp-server
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-udevd
        ├─tuned───4*[{tuned}]
        └─vmtoolsd───{vmtoolsd}
```

## Bảng thông tin tiến trình

- Hệ điều hành lưu giữ một cấu trúc danh sách bên trong hệ thống gọi là bảng tiến trình. Bảng tiến trình quản lý tất cả PID của hệ thống cùng với thông tin chi tiết về các tiến trình đang chạy. Ví dụ khi ta gọi lệnh ps, Linux thường đọc thông tin trong bảng tiến trình này và hiển thị những lệnh hay tên tiến trình được gọi: thời gian chiếm giữ CPU của tiến trình, tên người sử dụng tiến trình.

    - Bạn có thể sử dụng `ps -u` để hiển thị thông tin của các quy trình cho tên người dùng được chỉ định. 
    - Lệnh `ps -ef` hiển thị tất cả các quy trình trong hệ thống một cách chi tiết. 
    - Lệnh `ps -eLf` tiến thêm một bước và hiển thị một dòng thông tin cho mỗi luồng (một quá trình có thể chứa nhiều luồng).
    - Hiển thị process theo `PID` và `PPID`. Ta có thể sử dụng option `-p` để hiển thị prosess theo `PID` mà ta cần tìm kiếm. Dùng lệnh `ps -fp PID_của_process` ta có thể tìm kiếm một lúc nhiều `PID` bằng các nhập vào một lúc nhiều `PID` và ngăn các nhau bởi dấu cách.
    - Ta cũng có thể để hiển thị các process có một `PPID` nào đó ta dùng tùy chọn `--ppid`. Câu lệnh `ps -f --ppid PPID_cần_hiển_thị`
    - Lệnh `ps` cũng cho phép ta hiển thị ra các tiến trình có câu lệnh được chỉ ra. Ta sử dụng option `-C`. Dùng lệnh `ps -fC tên_lệnh`
    - Lệnh `ps` này cũng cho phép ta chỉ định những kết quả trả về màn hình với option `-o`: ` ps -o uid,pid,etime,cmd`



<img src=https://imgur.com/KVwfkmH.jpg>

- `UID` là tên của người dùng đã goi tiến trình
- `PID` là định danh mà hệ thống cấp cho tiến trình
- `PPID` là định danh của tiến trình cha
- `C` là số tiến trình con của một tiến trình
- `STIME` là thời gian tiến trình cha được sử dụng
- `TIME` là thời gian chiếm CPU của tiến trình
- `CMD` toàn bộ dòng lệnh khi tiến trình được triệu gọi
- `TTY` là màn hình terminal ảo nơi thực hiện tiến trình (Như chúng ta đã biết người dùng có thể đăng nhập vào hệ thống Linux từ rất nhiều terminal khác nhau để gọi tiến trình).

## Tắt tiến trình đang chạy với lệnh `kill`

- Trong bất kỳ hệ điều hành nào có đôi lúc chúng ta gặp phải tình trạng một ứng dụng không hoạt đọng và chúng ta ko thể đóng nó lại bằng cách bình thường. Trong Linux có lệnh cho phép ta chấm dứt hoạt động của chương trình một cách mạnh mẽ.
- Để chấm dứt một quá trình bạn có thể gõ `kill PID`. Tuy nhiên, lưu ý, bạn chỉ có thể giết các quy trình của riêng mình, những quy trình thuộc về người dùng khác sẽ vượt quá giới hạn trừ khi bạn đã root.

### Sử dụng `top` để có được các cập nhật theo thời gian thực liên tục (cứ sau hai giây theo mặc định).

```
[root@vqmanh ~]# top
top - 17:29:38 up 56 min,  1 user,  load average: 0.00, 0.01, 0.04
Tasks: 103 total,   1 running, 101 sleeping,   1 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1863236 total,  1562364 free,   126524 used,   174348 buff/cache
KiB Swap:  2097148 total,  2097148 free,        0 used.  1551444 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
   686 root      20   0  300824   6324   4972 S   0.3  0.3   0:06.97 vmtoolsd
```

- Dòng đầu tiên của `top` hiển thị một bản tóm tắt nhanh chóng về những gì đang xảy ra trong hệ thống bao gồm:

    - Hệ thống đã hoạt động được bao lâu rồi
    - Có bao nhiêu người dùng đang đăng nhập
    - Trung bình tải là gì?

*Trung bình tải xác định mức độ bận rộn của hệ thống. Trung bình tải 1,00 trên mỗi CPU cho biết hệ thống được đăng ký đầy đủ, nhưng không bị quá tải. Nếu trung bình tải vượt quá giá trị này, nó chỉ ra rằng các quy trình đang cạnh tranh về thời gian của CPU. Nếu trung bình tải rất cao, nó có thể chỉ ra rằng hệ thống đang gặp sự cố.*

- Dòng thứ hai của `top` hiển thị tổng số quá trình, số lượng quá trình chạy, ngủ, dừng và zombie. So sánh số lượng các quy trình đang chạy với trung bình tải giúp xác định xem hệ thống đã đạt đến công suất hay có lẽ một người dùng cụ thể đang chạy quá nhiều quy trình. Các quy trình dừng nên được kiểm tra để xem mọi thứ có chạy đúng không.

- Dòng thứ ba của `top` cho biết thời gian CPU được phân chia giữa người dùng và `kernel` bằng cách hiển thị phần trăm thời gian CPU được sử dụng cho mỗi lần. Tỷ lệ phần trăm công việc người dùng đang chạy ở mức ưu tiên thấp hơn sau đó được liệt kê. Chế độ không tải ( id ) phải ở mức thấp nếu trung bình tải cao và ngược lại. Tỷ lệ phần trăm công việc đang chờ cho I / O được liệt kê. Ngắt bao gồm tỷ lệ phần cứng so với ngắt phần mềm. Steal time ( st ) thường được sử dụng với các máy ảo, có một số thời gian CPU nhàn rỗi của nó dành cho các mục đích sử dụng khác.

- Dòng thứ tư và thứ năm của topđầu ra biểu thị mức sử dụng bộ nhớ, được chia thành hai loại:

    - Bộ nhớ vật lý (RAM) - hiển thị trên dòng 4.
    - Hoán đổi không gian - hiển thị trên dòng 5.
    - Cả hai loại đều hiển thị tổng bộ nhớ, bộ nhớ đã sử dụng và không gian trống.

Mỗi dòng trong danh sách quy trình của `top` sẽ hiển thị thông tin về một quy trình. Theo mặc định, các quy trình được sắp xếp theo mức sử dụng CPU cao nhất. Thông tin sau đây về mỗi quy trình được hiển thị:

- Số nhận dạng tiến trình (PID)
- Chủ sở hữu tiến trình (USER)
- Ưu tiên (PR) và giá trị tốt đẹp (NI)
- Ảo (VIRT), vật lý (RES) và bộ nhớ dùng chung (SHR)
- Tình trạng (S)
- Tỷ lệ phần trăm của CPU (% CPU) và bộ nhớ (% MEM) được sử dụng
- Thời gian thực hiện (TIME +)
- Lệnh (CMD)

## Scheduling processes - Quy trình lập kế hoạch

### `at` là tiện ích được sử dụng để thực hiện bất kỳ lệnh không tương tác tại một thời điểm xác định. Công việc của `at` được chọn bởi các dịch vụ `atd`.

Cài đặt:

`yum install -y at`

 `systemctl start atd`

`systemctl enable atd`

**Cách sử dụng lệnh `at`**

Như bạn đọc có thể thấy, sau khi chỉ định thời gian được lên lịch, một dấu nhắc lệnh giống như hình ảnh sau sẽ xuất hiện:

```
[root@vqmanh ~]# at now + 10 minutes
at> echo OK
at> <EOT>
job 2 at Wed Aug 21 17:56:00 2019
```
Tại đây, người dùng chỉ cần nhập các lệnh muốn chạy. Chúng sẽ được thực thi dưới tên `user` hiện tại. Nhập lệnh muốn chạy tại một thời điểm được chỉ định và nhấn `Enter`. Nếu muốn chạy một lệnh tiếp theo, lặp lại quy trình tương tự. Khi thực hiện xong, nhấn Ctrl + D. `<EOT>` sẽ được hiển thị khi nhấn các phím đó, theo sau là thời gian các lệnh sẽ được thực thi.

Nếu muốn chạy các lệnh yêu cầu quyền root, đừng sử dụng `sudo`. Hãy nhớ rằng, lệnh sẽ chạy mà không cần giám sát, vì vậy `sudo` sẽ không hoạt động bởi vì không có ai nhập mật khẩu. Thay vào đó, trước tiên hãy đăng nhập với tư cách user root:
`sudo -i`

Và sau đó sử dụng lệnh `at` như bình thường. Bây giờ, tất cả các lệnh sẽ được thực thi với quyền root, thay vì user thông thường.

Sau khi lên lịch lệnh, hãy nhập:

`exit`


**Cách chỉ định ngày và thời gian để lên lịch cho các lệnh at
Người dùng có thể sử dụng một trong các hình thức sau đây.**

1. Chạy lệnh sau số phút, giờ, ngày hoặc tuần được chỉ định.

```
at now + 10 minutes
at now + 10 hours
at now + 10 days
at now + 10 weeks
```
2. Chạy vào một thời điểm chính xác:

`at 23:10`

Nếu bây giờ là 12:00, và bạn chạy:

`at 11:00`

Thì lệnh sẽ chạy vào ngày mai, tại thời điểm được chỉ định.

3. Chạy vào thời gian và ngày được chỉ định chính xác:

`at 12:00 December 31`

### `crontab` là nơi lưu trữ tất cả các dòng lệnh đã lập lịch sẵn. Nó sẽ tự động chạy các lệnh đó khi đến thời gian đã định trước hoặc theo đúng chu khì đã định.

- Một `cron` schedule đơn giản là một text file. Mỗi người dùng có một cron schedule riêng, file này thường nằm ở `/var/spool/cron`. Crontab files không cho phép bạn tạo hoặc chỉnh sửa trực tiếp với bất kỳ trình text editor nào, trừ phi bạn dùng lệnh crontab.

**Cài đặt crontab**
```
yum install cronie
service crond start
chkconfig crond on
```
Một số lệnh thường dùng:
```
crontab -e: tạo hoặc chỉnh sửa file crontab 
crontab -l: hiển thị file crontab 
crontab -r: xóa file crontab
```

**Cấu trúc của crontab**

<img src=https://imgur.com/PPKvXYW.jpg>

Nếu một cột được gán ký tự * điều đó có nghĩa là câu lệnh sẽ chạy ở mọi giá trị ứng với cột đó.

Ví dụ:
- Chạy script 30 phút 1 lần:

`0,30 * * * * command`
- Chạy script 15 phút 1 lần:

`0,15,30,45 * * * * command`
- Chạy script vào 3 giờ sáng mỗi ngày:

`0 3 * * * command`

- Chạy một câu lệnh 20 phút một lần
`0,20,40 * * * * command` hoặc `*/20 * * * * command` với cách thứ nhất nó sẽ chạy vào đúng phút thứ 0 20 và 40 còn với cách thứ 2 khi hoàn thành lệnh nó sẽ bắt đầu chạy với 20 phút một lần.

- Chạy một câu lệnh vào đúng 5h chiều các ngày từ thứ 2 đến thứ 6
`0 17 * * 1,2,3,4,5 command` 

*Chú ý khi thêm câu lệnh @reboot command thì câu lệnh command sẽ được thực hiện ngay khi hệ thống khởi động.*