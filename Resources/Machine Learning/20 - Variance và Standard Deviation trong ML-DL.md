# Variance và Standard Deviation trong ML-DL

## Câu hỏi

Variance và standard deviation cho biết điều gì?

## Variance

Variance đo mức độ phân tán quanh mean:

$$
Var(X)=E[(X-E[X])^2]
$$

Với dataset:

$$
Var(X)\approx\frac{1}{n}\sum_{i=1}^{n}(x_i-\bar{x})^2
$$

Standard deviation:

$$
\sigma = \sqrt{Var(X)}
$$

## Trong Python

```python
import numpy as np

x = np.array([10, 11, 12, 20, 30])

print(np.mean(x))
print(np.std(x))
```

## Tại sao ML cần nó?

Variance giúp ta biết feature có độ biến thiên lớn hay nhỏ.

Ví dụ:

```text
age       -> 18, 19, 20, 21
salary    -> 5, 20, 100, 200
```

Hai feature có scale và độ phân tán rất khác nhau.

Điều này có thể ảnh hưởng đến quá trình optimization.

## Standardization

Một cách preprocessing phổ biến:

$$
z=\frac{x-\mu}{\sigma}
$$

Trong code:

```python
x_scaled = (x - mean) / std
```

## Ghi nhớ

> Variance đo độ phân tán; standard deviation đưa độ phân tán về cùng đơn vị với dữ liệu.
