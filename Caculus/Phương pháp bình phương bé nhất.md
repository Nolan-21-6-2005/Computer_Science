Cho mô hình đường thẳng

$$
y=a+bx
$$

với tập dữ liệu

$$
(x_1,y_1),(x_2,y_2),\ldots,(x_n,y_n)
$$

Ta cần tìm hai tham số \(a,b\) sao cho đường thẳng khớp dữ liệu tốt nhất.

---

## Hàm sai số

Sai số tại điểm thứ \(i\):

$$
e_i=y_i-(a+bx_i)
$$

Tổng bình phương sai số:

$$
S(a,b)=\sum_{i=1}^{n}\left[y_i-(a+bx_i)\right]^2
$$

Mục tiêu:

$$
\min S(a,b)
$$

---

### Bước 1. Đạo hàm theo \(a\)

Xét một hạng tử của tổng:

$$
\left[y_i-(a+bx_i)\right]^2
$$

Đặt

$$
u=y_i-(a+bx_i).
$$

Theo quy tắc dây chuyền:

$$
\frac{d(u^2)}{da}
=
2u\frac{du}{da}
$$

Ta có

$$
\frac{du}{da}=-1.
$$
(Đạo hàm theo a)
Suy ra

$$
\frac{\partial}{\partial a}
\left[y_i-(a+bx_i)\right]^2
=
-2\left[y_i-(a+bx_i)\right].
$$

Do đó

$$
\frac{\partial S}{\partial a}
=
\sum_{i=1}^{n}
-2\left[y_i-(a+bx_i)\right].
$$

Cho đạo hàm bằng 0:

$$
-2\sum_{i=1}^{n}
\left[y_i-a-bx_i\right]
=0.
$$

Chia hai vế cho \(-2\):

$$
\sum_{i=1}^{n}
(y_i-a-bx_i)=0.
$$

Khai triển:

$$
\sum y_i
-
\sum a
-
\sum bx_i
=0.
$$

Vì \(a\) và \(b\) là hằng số nên

$$\begin{aligned}
\sum a=na \\
\sum bx_i=b\sum x_i
\end{aligned}
$$

Suy ra

$$
na+b\sum x_i=\sum y_i.
$$

Đây là **phương trình thứ nhất**.

---

### Bước 2. Đạo hàm theo \(b\)

Tiếp tục đặt

$$
u=y_i-(a+bx_i).
$$

Lấy đạo hàm theo \(b\):

$$
\frac{du}{db}=-x_i.
$$

Theo quy tắc dây chuyền:

$$
\frac{\partial}{\partial b}
\left[y_i-(a+bx_i)\right]^2
=
-2x_i
\left[y_i-(a+bx_i)\right].
$$

Suy ra

$$
\frac{\partial S}{\partial b}
=
-2
\sum
x_i
\left[y_i-(a+bx_i)\right].
$$

Cho đạo hàm bằng 0:

$$
\sum
x_i
(y_i-a-bx_i)
=0.
$$

Khai triển:

$$
\sum x_i y_i
-
a\sum x_i
-
b\sum x_i^2
=
0.
$$

Chuyển vế:

$$
a\sum x_i
+
b\sum x_i^2
=
\sum x_i y_i.
$$

Đây là **phương trình thứ hai**.

---
## Hệ phương trình chuẩn

Từ hai phương trình trên, ta thu được

$$
\begin{cases}
na+b\sum x_i=\sum y_i,\\
a\sum x_i+b\sum x_i^2=\sum x_i y_i.
\end{cases}
$$

Hệ này được gọi là **hệ phương trình chuẩn (Normal Equations)** của phương pháp bình phương bé nhất.
