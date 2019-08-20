# Users and Groups

Linux là một hệ điều hành nhiều người dùng, nơi có nhiều hơn một người dùng có thể đăng nhập cùng một lúc. Để xác định người dùng hiện tại, sử dụng `who -a`.

<img src=https://imgur.com/D2XhEBc.jpg>

- Linux sử dụng các nhóm để tổ chức người dùng. Nhóm là tập hợp các tài khoản với các quyền được chia sẻ nhất định. Kiểm soát thành viên nhóm được quản lý thông qua tệp `/etc/group`, trong đó hiển thị danh sách các nhóm và thành viên của họ. Khi người dùng đăng nhập, tất cả các thành viên được hưởng cùng mức truy cập và đặc quyền. Quyền trên các tập tin và thư mục khác nhau có thể được sửa đổi ở cấp độ nhóm.

- Tất cả người dùng Linux được gán một `ID` người dùng duy nhất, `uid`, chỉ là một số nguyên, cũng như một hoặc nhiều ID nhóm, `gid`, bao gồm một ID mặc định giống với ID người dùng. Trong lịch sử, các bản phân phối dựa trên RedHat bắt đầu uid ở mức 500. Các bản phân phối khác bắt đầu từ 1000. Những con số này được liên kết với tên thông qua các tệp `/etc/passwd` và `/etc/group`. Các nhóm được sử dụng để thiết lập một nhóm người dùng có lợi ích chung cho các mục đích về quyền truy cập, đặc quyền và cân nhắc bảo mật. Quyền truy cập vào tệp và thiết bị được cấp trên cơ sở người dùng và nhóm mà họ thuộc về.

- Chỉ người dùng root mới có thể thêm và xóa người dùng và nhóm. Thêm một người dùng mới được thực hiện bằng `useradd` và loại bỏ một người dùng hiện có được thực hiện bằng `userdel`.

- Sử dụng `passwd tên_user` để thay đổi mật khẩu cho người dùng mới  

VD: tạo user - `useradd tên_user`

<img src=https://imgur.com/DpSkjQu.jpg>

```
[root@vqmanh ~]#  cat /etc/passwd | grep vqmanh
vqmanh:x:1000:1000::/home/vqmanh:/bin/bash
[root@vqmanh ~]# ls -lrta /home/vqmanh/
total 12
-rw-r--r--. 1 vqmanh vqmanh 231 Oct 31  2018 .bashrc
-rw-r--r--. 1 vqmanh vqmanh 193 Oct 31  2018 .bash_profile
-rw-r--r--. 1 vqmanh vqmanh  18 Oct 31  2018 .bash_logout
drwx------. 2 vqmanh vqmanh  62 Aug 21 03:08 .
drwxr-xr-x. 4 root   root    31 Aug 21 03:09 ..
```

VD: xóa user - `userdel tên_user`

*Tuy nhiên, thư mục người dùng vẫn sẽ còn trên /home. Điều này có thể hữu ích nếu nó là xóa tạm thời. Để xóa thư mục chính trong khi xóa tài khoản, ta cần sử dụng `userdel -r tên_user`.*

## THAM KHẢO
```
Lệnh tạo user: useradd + tên
Lệnh tạo pass cho user: passwd [tên user]
Mỗi user sẽ có 1 ID (gọi là UDI)                                          
User root có ID = 0
Tất cả user được tạo ra đều được coi là Regular User (user thường)
Thông thường khi user được tạo sẽ có id= 1000
Kiểm tra user đã được tạo chưa: cat /etc/passwd
Lệnh đăng nhập vào user: su [tên user]
Xóa user: userdel  –r  [tên user]
Tạo Group: groupadd [tên group]
Mỗi group sẽ có 1 ID ( gọi là GID). Mặc định nhóm root có ID=0
Kiểm tra group đã dc tạo chưa: cat /etc/group
Gán user và nhóm: usermod –aG [tên nhóm] [tên user]
Kiểm tra user có trong group chưa: lid –g [tên nhóm]
Xóa group: groupdel [tên group]
Ta có thể khóa tài khoản người dùng: passwd –l [tên user] 
Mở khóa user: passwd –u [tên user]	
Lệnh kiểm tra hạn của user: chage –l [tên user]
Cú pháp kiểm tra 90 ngày nữa là ngày bn: date –d “+90 days”
Gia hạn cho user: chage –E [năm-tháng-ngày] [tên user]
Thay đổi số ngày thông báo thay đổi pass User: Chage –W [số ngày] [tên user]
```

**Lệnh `id` cung cấp thông tin về người dùng hiện tại.**

```
[root@vqmanh ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[root@vqmanh ~]# id vqmanh
uid=1000(vqmanh) gid=1000(vqmanh) groups=1000(vqmanh)
```

**Thêm một nhóm mới được thực hiện với `groupadd` và xoá bằng `groupdel`.**

```
[root@vqmanh ~]# groupadd thuctap
[root@vqmanh ~]# cat /etc/group | grep thuctap
thuctap:x:1001:
[root@vqmanh ~]# groupdel thuctap
```

**Thêm người dùng vào một nhóm đã tồn tại được thực hiện bằng `usermod`**

```
[root@vqmanh ~]# usermod -G thuctap vqmanh
[root@vqmanh ~]# groups vqmanh
vqmanh : vqmanh thuctap
[root@vqmanh ~]# usermod -g thuctap vqmanh
[root@vqmanh ~]# groups vqmanh
vqmanh : thuctap
```
**Tất cả các lệnh này cập nhật /etc/group khi cần thiết. Các lệnh `groupmod`có thể được sử dụng để thay đổi ID hoặc tên nhóm**

```
[root@vqmanh ~]#  groupmod thuctap -n nhanhoa
[root@vqmanh ~]# groups vqmanh
vqmanh : nhanhoa
```

## Người dùng `root`
- Tài khoản `root` có quyền truy cập đầy đủ vào hệ thống. Các hệ điều hành khác thường gọi đây là tài khoản quản trị viên. Trong Linux, nó thường được gọi là tài khoản `superuser` . Bạn phải cực kỳ thận trọng trước khi cấp quyền truy cập `root` đầy đủ cho người dùng. Các cuộc tấn công bên ngoài thường bao gồm các thủ thuật được sử dụng để nâng lên tài khoản `root`. Tuy nhiên, bạn có thể sử dụng tính năng `sudo` để gán các đặc quyền hạn chế hơn cho các tài khoản người dùng chuẩn:

    - Chỉ trên cơ sở tạm thời.
    - Chỉ cho một tập hợp con cụ thể của lệnh.

- Khi gán đặc quyền nâng cao, bạn có thể sử dụng lệnh `su tên_user`(chuyển người dùng) để khởi chạy shell mới chạy với tư cách người dùng khác (bạn phải nhập mật khẩu của người dùng mà bạn đang trở thành). Thông thường người dùng khác này là `root` và `shell` mới cho phép sử dụng các đặc quyền nâng cao cho đến khi thoát. 

## Khởi động tập tin - Startup Files
- Trong Linux, chương trình `shell`, nói chung `bash` sử dụng một hoặc nhiều tệp khởi động để cấu hình môi trường. Các tệp trong thư mục `/etc` xác định cài đặt chung cho tất cả người dùng trong khi các tệp khởi tạo trong thư mục chính của người dùng có thể bao gồm và ghi đè cài đặt chung. Các tệp khởi động có thể làm bất cứ điều gì người dùng muốn làm trong mọi lệnh `shell`, chẳng hạn như:

    - Tùy chỉnh lời nhắc của người dùng
    - Xác định các phím tắt và bí danh dòng lệnh
    - Đặt trình soạn thảo văn bản mặc định
    - Đặt đường dẫn cho nơi tìm chương trình thực thi

- Khi bạn đăng nhập lần đầu vào Linux, tệp `/etc/profile` sẽ được đọc và đánh giá, sau đó các tệp sau được tìm kiếm theo thứ tự được liệt kê:

    - ~/.bash_profile
    - ~/.bash_login
    - ~/.profile

- `Shell` đăng nhập Linux đánh giá bất kỳ tệp khởi động nào mà nó xuất hiện đầu tiên và bỏ qua phần còn lại. Điều này có nghĩa là nếu nó tìm thấy `~/.bash_profile`, nó bỏ qua phần còn lại. Các bản phân phối khác nhau có thể sử dụng các tệp khởi động khác nhau. Tuy nhiên, mỗi khi bạn tạo một `shell` mới hoặc cửa sổ terminal, v.v., bạn không thực hiện đăng nhập toàn hệ thống; chỉ có tập tin `~/.bashrc`  được đọc và đánh giá. Mặc dù tệp này không được đọc và đánh giá cùng với vỏ đăng nhập, hầu hết các bản phân phối và / hoặc người dùng bao gồm tệp `~/.bashrc` từ một trong ba tệp khởi động do người dùng sở hữu. Trong các bản phân phối Ubuntu, openSuse và CentOS, người dùng phải thực hiện các thay đổi phù hợp trong tệp `~/.bash_profile` để bao gồm `~/.bashrc`. Các `~/.bash_profile` sẽ có một số dòng thêm, do đó sẽ thu thập các thông số tùy yêu cầu từ `~/.bashrc`.

## Biến môi trường - Environment variables
- Các biến môi trường được đặt tên đơn giản là các đại lượng có giá trị cụ thể và được hiểu bởi `shell`, chẳng hạn như `bash`. Một số trong số này được hệ thống cài đặt sẵn và một số khác được người dùng đặt ở dòng lệnh hoặc trong khi khởi động và các tập lệnh khác. Một biến môi trường thực sự không nhiều hơn một chuỗi ký tự chứa thông tin được sử dụng bởi một hoặc nhiều ứng dụng. Có một số cách để xem các giá trị của các biến môi trường hiện được đặt. Tất cả các `set`, `env` hoặc `export` hiển thị các biến môi trường.

- Theo mặc định, các biến được tạo trong một tập lệnh chỉ có sẵn cho trình `shell` hiện tại. Tất cả các tiến trình con (shell phụ) sẽ không có quyền truy cập vào các giá trị đã được đặt hoặc sửa đổi. Cho phép các tiến trình con để xem các giá trị, yêu cầu sử dụng lệnh `export`.

Task|	Command
-----|-------
Hiển thị giá trị của một biến cụ thể|`echo $SHELL	`
Xuất một giá trị biến mới	|	`export VAR=value`
Thêm một biến vĩnh viễn	|Thêm dòng `export VAR=value to ~/.bashrc`

- Các `HOME` là một biến môi trường đại diện cho `home` hoặc đăng nhập thư mục của người dùng. Các lệnh `cd` không có đối số sẽ thay đổi thư mục làm việc hiện tại với giá trị của HOME. Lưu ý ký tự dấu ngã (~) thường được sử dụng làm chữ viết tắt cho `$HOME`.

- Các `PATH` biến môi trường là một danh sách có thứ tự các thư mục đó sẽ được quét khi một lệnh được đưa ra để tìm các chương trình hay kịch bản thích hợp để chạy. Mỗi thư mục trong đường dẫn được phân tách bằng dấu hai chấm. Tên thư mục trống cho biết thư mục hiện tại tại bất kỳ thời điểm nào.

```
[root@vqmanh ~]# export PATH=$HOME/bin:$PATH
[root@vqmanh ~]# echo $PATH
/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
```

- Các `PS` biến môi trường (Prompt Statement) được sử dụng để tùy chỉnh chuỗi dấu nhắc của bạn trong cửa sổ terminal của bạn để hiển thị các thông tin mà bạn muốn. `PS1` là biến dấu nhắc chính điều khiển dấu nhắc dòng lệnh của bạn trông như thế nào. Các ký tự đặc biệt sau đây có thể được bao gồm trong `PS1`:

Character|	Usage
-------|---------
\u|	Tên người dùng
\h|	Tên máy chủ
\w|	Thư mục làm việc hiện tại
!	|	Số lịch sử của lệnh này
\d	|Ngày

**Chúng phải được bao quanh trong dấu ngoặc đơn khi chúng được sử dụng**
```
[root@vqmanh ~]# export PS1='\u@\h:\w$ '
root@vqmanh:~$ export PS1='\d-\u@\h:\w$ '
Wed Aug 21-root@vqmanh:~$
```

**Các SHELL môi trường điểm biến `shell` lệnh mặc định của người dùng (các chương trình được xử lý bất cứ điều gì bạn gõ vào một cửa sổ lệnh, thường bash) và chứa các đường dẫn đầy đủ đến `shell`**

```
[root@vqmanh ~]# echo $SHELL
/bin/bash
```

## Lịch sử lệnh - Command history

- `Bash` theo dõi các lệnh và câu lệnh đã nhập trước đó trong bộ đệm lịch sử; bạn có thể nhớ lại các lệnh đã sử dụng trước đó chỉ bằng cách sử dụng các phím con trỏ Lên và Xuống. Để xem danh sách các lệnh đã thực hiện trước đó, bạn có thể sử dụng `history` tại dòng lệnh. Danh sách các lệnh được hiển thị với lệnh gần đây nhất xuất hiện cuối cùng trong danh sách. Thông tin này được lưu trữ trong tập tin `~/.bash_history`. Một số biến môi trường liên quan có thể được sử dụng để lấy thông tin về tệp lịch sử.

Variable	|Usage
--------|------------
$HISTFILE|	lưu trữ vị trí của tập tin lịch sử
$HISTFILESIZE|	lưu trữ số lượng dòng tối đa trong tệp lịch sử
$HISTSIZE	|lưu trữ số lượng dòng tối đa trong tệp lịch sử cho phiên hiện tại

**Bảng dưới đây cho thấy cú pháp được sử dụng để thực hiện các lệnh được sử dụng trước đó**

Syntax|	Usage
--|---
!!	|Thực hiện lệnh trước
!	|Bắt đầu thay thế lịch sử
!$	|Tham khảo đối số cuối cùng trong một dòng
!n	|Tham khảo dòng lệnh thứ n
!string	|Tham khảo lệnh gần đây nhất bắt đầu bằng chuỗi

## Tạo bí danh - Creating Aliases

- Các lệnh tùy chỉnh có thể được tạo để sửa đổi hành vi của những cái đã có bằng cách tạo bí danh. Thông thường các bí danh này được đặt trong `~/.bashrc` của bạn để chúng có sẵn cho bất kỳ shell lệnh nào bạn tạo. Các `alias` với không có đối số sẽ liệt kê các bí danh quy định hiện hành.

```
[root@vqmanh ~]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
```