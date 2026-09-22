# Probability Density Function trong ML-DL

## Câu hỏi

PDF (Probability Density Function) dùng để làm gì?

## Ý chính

Với biến ngẫu nhiên liên tục, probability density function (PDF) mô tả mật độ xác suất.

Ký hiệu:

$$
f(x)
$$

Xác suất để $X$ nằm trong khoảng $[a,b]$ được tính bằng:

$$
P(a \le X \le b) = \int_a^b f(x)\,dx
$$

Điểm quan trọng:

> Với biến liên tục, $f(x)$ không phải trực tiếp là xác suất tại đúng một điểm.

## Ví dụ

Phân phối Gaussian:

$$
X \sim \mathcal{N}(\mu,\sigma^2)
$$

PDF:

$$
f(x)=
\frac{1}{\sqrt{2\pi\sigma^2}}
e^{-\frac{(x-\mu)^2}{2\sigma^2}}
$$

## Liên hệ ML

PDF xuất hiện trong các bài toán:

- density estimation;
- Gaussian models;
- anomaly detection;
- probabilistic modeling;
- generative models.

Ví dụ trực quan:

```python
import numpy as np

x = np.random.normal(
    loc=0,
    scale=1,
    size=1000
)
```

Các sample được tạo từ Gaussian distribution.

## Ghi nhớ

> PDF mô tả mật độ của một biến liên tục; diện tích dưới đường PDF trên một khoảng mới tương ứng với xác suất.
