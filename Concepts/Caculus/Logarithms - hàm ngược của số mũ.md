## Mục đích sử dụng
Logarithm được dùng vì **nó biến phép nhân thành phép cộng**, đồng thời nén các giá trị rất lớn hoặc rất nhỏ về một thang đo dễ xử lý hơn.

- Nếu $2^5$ thì $\log_2(32) =5$

Biến phép nhân thành phép cộng:
$$\log(ab) = \log(a) + \log(b)$$
### 1. Log của tích

$$
\log(ab)=\log a+\log b
$$

Giúp chuyển:

$$
\prod_{i=1}^{N}(\cdot)
\quad\Longrightarrow\quad
\sum_{i=1}^{N}\log(\cdot)
$$

---

### 2. Log của lũy thừa

$$
\log(a^b)=b\log a
$$

Giúp đưa số mũ ra ngoài dấu log.

## Từ Likelihood đến Log-Likelihood

### 1. Hàm Likelihood

Đối với Logistic Regression, hàm likelihood là:

$$
L(w,b)=\prod_{i=1}^{N}p_i^{y_i}(1-p_i)^{1-y_i}
$$

Trong đó:

- $p_i=P(y_i=1\mid x_i)$ là xác suất mô hình dự đoán mẫu thứ $i$ thuộc lớp 1.
- $y_i\in\{0,1\}$ là nhãn thực.

---

### 2. Lấy log hai vế

Lấy logarithm của likelihood:

$$
\log L(w,b)
=
\log\left(
\prod_{i=1}^{N}
p_i^{y_i}(1-p_i)^{1-y_i}
\right)
$$

---

### 3. Áp dụng tính chất của log

Sử dụng tính chất:

$$
\log(ab)=\log a+\log b
$$

Suy ra:

$$
\log L(w,b)
=
\sum_{i=1}^{N}
\log\left(
p_i^{y_i}(1-p_i)^{1-y_i}
\right)
$$

---

### 4. Tiếp tục tách tích bên trong log

Áp dụng tiếp:

$$
\log(ab)=\log a+\log b
$$

Ta được:

$$
=
\sum_{i=1}^{N}
\left[
\log(p_i^{y_i})
+
\log((1-p_i)^{1-y_i})
\right]
$$

---

### 5. Đưa số mũ ra ngoài log

Sử dụng quy tắc:

$$
\log(a^b)=b\log a
$$

Suy ra:

$$
\boxed{
\log L(w,b)
=
\sum_{i=1}^{N}
\left[
y_i\log p_i
+
(1-y_i)\log(1-p_i)
\right]
}
$$

Đây chính là **hàm Log-Likelihood** của Logistic Regression.

