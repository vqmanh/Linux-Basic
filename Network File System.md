# NFS (Network File System)

## Nội dung 

[A. Tổng quan về NFS](#A)

- [A1. Một số khái niệm](#A1)

- [A2. Cách hoạt động](#A2)

- [A3. Các phiên bản](#A3)

- [A4. Ưu nhược điểm](#A4)


[B. LAB](#B)

- [B1. Mô hình](#B1)
- [B2. IP Planing](#B2)
- [B3. Triển khai](#B3)

<a name = "A"></a>
## A. Tổng quan về NFS

- NFS là một trong những phương pháp được sử dụng để chia sẻ dữ liệu trên các hệ thống vật lý. Được phát triển bởi SunMicrosystems và năm 1984, cho phép người dùng xem, tùy chọn lưu trữ và cập nhật trên máy tính từ xa.
- NFS là hệ thống cung cấp dịch vụ chia sẻ file phổ biến trong hệ thống mạng Linux và Unix.
- NFS cho phép các máy tính kết nối tới 1 phân vùng đĩa trên 1 máy từ xa giống như là local disk. Cho phép việc truyền file qua mạng được nhanh và trơn tru hơn.
- NFS sử dụng mô hình Client/Server. Trên server có các disk chứa các file hệ thống được chia sẻ và một số dịnh vụ chạy ngầm (daemon) phục vụ cho việc chia sẻ với Client.
- Cung cấp chức năng bảo mật file và quản lý lưu lượng sử dụng (file system quota).

- Khi triển khai hệ thống lớn hoặc chuyên biệt cần áp dụng 3NFS, còn người dùng ngẫu nhiên hoặc nhỏ lẻ thì áp dụng 2NFS, 4NFS.
- Với NFSv4, yêu cầu hệ thống phải có kernel phiên bản từ 2.6 trở lên

- Client từ phiên bản kernel 2.2.18 trở đi đều hỗ trợ NFS trên nền TCP

<a name = "A1"></a>
### A1. Một số khái niệm

- **Virtual filesystem (VFS)** là một kỹ thuật tự động chuyển hướng tất cả các truy xuất đến NFS-mount file một cách thông suốt trên Remote Server. 

- **Stateless Operation** là những chương trình đọc và ghi tập tin trên hệ thống tập tin cục bộ dựa vào hệ thống để theo dõi và ghi nhận vị trí đọc dữ liệu thông qua con trỏ địa chỉ pointer. 
- **Caching** trên NFS Client để lưu lại một số dữ liệu cần thiết vào hệ thống cục bộ. 
- **NFS Background Mounting** chỉ định khoảng thời gian đợi với tham số gb trong trường hợp Remote Server không tồn tại. 
- **Hard and Soft Mounts** có ý nghĩa rằng quá trình mount file luôn được tiến hành và quá trình sử dụng RPC để mount remote file system. 

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