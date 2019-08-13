# FILE PERMISSION


### **Sẽ có 3 nhóm quyền chính tương ứng với 3 nhóm đối tượng:**
+ User – Owner
+ Group
+ Other (anonymous)

### **Nhóm quyền:**
+ Read (r) - 4
+ Write (w) - 2
+ Execute (x) – 1

**Ta có thể biểu thị quyền này theo 2 dạng**
-	Biểu thị qua kí tự: r w x
-	Biểu thị qua số hệ cơ số 8 (bát phân): 4 2 1

**Lệnh gán quyền hạn cho đối tượng:**
 
 `chmod quyền_hạn tên_file/folder`

 VD: chmod 740 /home/vqmanh

 <img src=https://imgur.com/fZ7T4Hn.jpg>

**Thay đổi người sở hữu:**

 `chown tên_user tên_file/folder`

**Thay đổi nhóm sở hữu:**

 `chgrp tên_group tên_file/folder`

 **Thêm quyền thực thi:** 
 
 `chmod +x tên_file`

 | Quyền | File	| Folder |
 ------|--------|---------|
 Read (r-4)|	Đọc file	|Liệt kê file và thư mục con tức là dùng lệnh ls
Write (w-2)	|Chỉnh sửa, xóa file	|Tạo file, tạo thư mục con, xóa file, xóa thư mục con
Execute (x-1)	|Thực thi file	|Có quyền chuyển vào thư mục, tức lệnh cd

### Phân quyền mở rộng (ACL)

`getfacl` -> để kiểm tra thuộc tính của file/folder

`setfacl` -> để thiết lập quyền hạn

VD: Thiết lập permission cho user: 

User vqmanh: đọc và thực thi

User pak: chỉ đọc

`setfacl -m user:vqmanh:rx file/folder`

`setfacl -m user:pak:r file/folder`

VD: Thiết lập permisson cho group

Group thuctap: chỉ đọc

`setfacl -m group:thuctap:r file/folder`

VD: Thiết lập permission cho other

`setfacl -m other:r file/folder`


	 



