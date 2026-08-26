# Cách vận hành của thuật toán ML

Khi học lần đầu mô hình sẽ thử nghiệm trước. 

VD 1: 
Bài toán Linear Regression:
$$
y = wx+b
$$
- Trong đó: $w$ và $b$ là tham số.

Khi thiết kế mô hình ta sẽ chọn tham số sao cho mô hình cho ra kết quả hợp lý nhất.

Giả sử: 

| $x_1$ | $x_2$ | $y$ |
| ----- | ----- | --- |
| 1     | 2     | 4   |
| 2     | 3     | 6   |
| 2     | 2     | 5   |
Dựa trên tập dữ liệu ta có mô hình

$$
y = w_1x_1 + w_2x_2 + b 
$$

- Khởi tạo thử giá trị siêu tham số: $w_1 = 1$, $w_2=2$, $b = 0$.
- Mô hình sau khi khởi tạo siêu tham số:

$$
y = x_1 + 2x_2
$$

Dữ liệu sau khi dự đoán:

| $x_1$ | $x_2$ | $y$ |
| ----- | ----- | --- |
| 1     | 2     | 4   |
| 2     | 3     | 8   |
| 2     | 2     | 6   |
Các sai số lần lượt là: $y_1 = 0$, $y_2 = 2$, $y_3 = 1$.

$$
\text{MSE} = \frac{0^2 + 2^2 + 1^2}​{3} = \frac{5}{3}​≈1.67
$$

## Hyperparameter

Đây là các biến số có thể kiểm soát được khi thiết kế mô hình. Nó được sử dụng để điều khiến cách học của mô hình. 

Nếu dùng Gradient Descent cho bài toán trên thì siêu tham số là: 

```
learning_rate = 0.1
epoch = 50
```

Thuật toán này sẽ được lặp lại liên tục cho đến khi có được kết quả tối ưu nhất.