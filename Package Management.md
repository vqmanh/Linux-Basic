# Package Management

|Công cụ cấp thấp|Công cụ cấp cao|	Family|
------|--------------|------------|
apt-get	|dpkg|	Debian
zypper	|rpm|	SUSE
yum	|rpm	|Red Hat

- Cả hai hệ thống quản lý gói đều cung cấp hai cấp công cụ: công cụ cấp thấp (như dpkg hoặc rpm): giải nén các gói riêng lẻ, chạy tập lệnh, cài đặt phần mềm chính xác. Trong khi công cụ cấp cao (như apt-get, yum. hoặc zypper) hoạt động với các nhóm gói, tải gói từ nhà cung cấp và tìm ra các gói phụ thuộc. 
- Hầu hết thời gian người dùng chỉ cần làm việc với công cụ cấp cao, công việc này sẽ đảm nhiệm việc gọi công cụ cấp thấp khi cần thiết. 
- Dependency tracking là một tính năng đặc biệt quan trọng của công cụ cấp cao, vì nó xử lý các chi tiết tìm kiếm và cài đặt từng gói cần thiết cho bạn. 
- Tuy nhiên, cài đặt một gói duy nhất có thể dẫn đến hàng chục hoặc thậm chí hàng trăm gói phụ thuộc được cài đặt.

|Operation	|RPM|	Debian|
--------|---------|------
Cài đặt một gói|rpm –i foo.rpm|	dpkg --install foo.deb
Cài đặt một gói với các phụ thuộc từ kho lưu trữ|yum install foo	|apt-get install foo
Hủy bỏ một gói|rpm –e foo.rpm|	dpkg --remove foo.deb
Xóa gói và phụ thuộc bằng kho lưu trữ|yum remove foo|	apt-get remove foo
Cập nhật gói lên phiên bản mới hơn|rpm –U foo.rpm|	dpkg --install foo.deb
Cập nhật gói sử dụng kho lưu trữ và giải quyết các gói phụ thuộc|yum update foo|	apt-get upgrade foo
Cập nhật toàn bộ hệ thống|yum update	|apt-get dist-upgrade
Hiển thị tất cả các gói đã cài đặt|yum list installed|	dpkg --list
Nhận thông tin về một gói được cài đặt bao gồm các tệp|rpm –qil foo|	dpkg --listfiles foo	
Hiển thị gói có sẵn với "foo" trong tên|yum list foo|	apt-cache search foo
Hiển thị tất cả các gói có sẵn|yum list	|apt-cache dumpavail
Hiển thị các gói một tập tin thuộc về|rpm –qf file	|dpkg --search file