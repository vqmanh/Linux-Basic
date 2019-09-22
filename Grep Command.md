# Tìm hiểu về lệnh GREP trong linux

## Lệnh grep là gì?
`Grep` là từ viết tắt của G lobal R egular E xpression P rint.

- Grep là một công cụ dòng lệnh Linux/Unix được sử dụng để tìm kiếm một chuỗi ký tự trong một tệp được chỉ định. Mẫu tìm kiếm văn bản được gọi là biểu thức chính quy. Khi tìm thấy kết quả khớp, nó sẽ in dòng kết quả. Lệnh grep rất tiện lợi khi tìm kiếm thông qua các tệp nhật ký lớn.

## Cách sử dụng

**Cú pháp** `grep [tuỳ chọn] <file>`

### Ví dụ

**Mình có 2 file voivang.txt và song.txt**

<img src=https://imgur.com/uLe69bH.jpg>

### 1: Tìm một chuỗi trong một file

Tìm từ "tat nang"

<img src=https://imgur.com/yoGaXLp.jpg>

### 2:  Tìm kiếm chuỗi trên nhiều file

Tìm từ "gio"



<img src=https://imgur.com/iuNldDJ.jpg>

### 3: Tìm kiếm không phân biệt hoa thường, sử dụng tùy chọn `-i`

<img src=https://imgur.com/xxiTUQI.jpg>

Nếu không có tùy chọn `-i` thì khi tìm tiếm từ "toi" sẽ không hiển thị được vì chúng ta tìm kiếm là chữ thường.

### 4: Tìm kiếm ngược, sử dụng tùy chọn `-v`


<img src=https://imgur.com/aAcBVXe.jpg>

Tùy chọn này sẽ hiển thị những dòng không chứa từ "nang"

### 5: Hiển thị số dòng, số lượng, giới hạn số dòng đầu ra

Tùy chọn ứng với các tùy chọn sau:

- `-n`: Hiện thị số dòng của dòng cần tìm
- `-c`: Đếm số dòng khớp với kí tự cần tìm
- `-m[chỉ số cần giới hạn]`: Giới hạn số lượng dòng khớp

<img src=https://imgur.com/0rLtBtw.jpg>

### 6: Tìm kiếm nhiều chuỗi

Có 3 cú pháp tìm:

- C1: `grep -e "word1" -e "word2" <file>`
- C2: `grep "word1\|word2" <file>` # `\` để phân biệt word1 với word2
- C3: `egrep "word1|word2" <file>`

<img src=https://imgur.com/ukelNG6.jpg>

### 7: Tìm kiếm tên tệp

Các tùy chọn:

- `-l`: để có được các tập tin phù hợp với tìm kiếm
- `-L`: để có được các tập tin không phù hợp với tìm kiếm
- `-h`: hiện thị không với tên file (khi chỉ định nhiều file)
- `-H`: hiển thị cùng với tên file


<img src=https://imgur.com/zF3MOQt.jpg>

<img src=https://imgur.com/Om426NL.jpg>

### 8: Tìm chính xác với `-w`

<img src=https://imgur.com/enwwuM4.jpg>

Với `grep` thông thường thì khi tìm "vqmanh" sẽ hiển thị tất cả các dòng chứa "vqmanh" kể cả "vqmanh99". Khi sử dụng tùy chọn `-w' thì sẽ tìm chính xác dòng chỉ chứa "vqmanh" thôi.