---
tags:
---
##### [[Tìm siêu phẳng phân chia (Hyperplane)]]
Phương trình siêu phẳng:

$$
w^Tx+b=0
$$
Trong đó:
- $w$: vector trọng số.
- $x$: vector đặc trưng.
- $b$: hệ số dịch chuyển.

##### [[Tối đa hóa khoảng cách biên (Margin)]]
Margin là khoảng cách từ siêu phẳng đến các điểm dữ liệu gần nhất của mỗi lớp.

$$
Margin = \frac{2}{||w||}​
$$

Mục tiêu: 

$$
min \frac{1}{2} ||w||^2
$$

với điều kiện:

$$
y_i(w^Tx_i+b)\ge1
$$
##### Xác định Support Vectors
Support Vectors là các điểm dữ liệu nằm gần siêu phẳng nhất.
Đặc điểm:
- Quyết định vị trí của siêu phẳng.
- Nếu bỏ các điểm khác đi, siêu phẳng gần như không thay đổi.
- Nếu thay đổi support vectors, siêu phẳng sẽ thay đổi.
##### Dữ liệu không phân tách tuyến tính

