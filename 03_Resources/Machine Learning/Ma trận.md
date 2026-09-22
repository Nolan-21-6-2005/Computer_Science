# Ma trận trong ML/DL — Dùng để làm gì?

## Câu hỏi

**Tại sao khi lập trình Machine Learning và Deep Learning lại thường xuyên gặp ma trận?**

## Ý chính

Ma trận là một cách để tổ chức nhiều giá trị số thành cấu trúc có **hàng** và **cột**.

Trong ML/DL, ma trận thường xuất hiện ở hai nơi:

1. **Biểu diễn dữ liệu**
    
2. **Biểu diễn trọng số của mô hình**
    

Ví dụ dataset có 4 mẫu và mỗi mẫu có 3 features:

X∈R4×3X \in \mathbb{R}^{4\times3}

Có thể biểu diễn bằng NumPy:

```python
import numpy as np

X = np.array([
    [20, 8, 1],
    [35, 20, 8],
    [28, 15, 4],
    [40, 30, 12]
])

print(X.shape)
# (4, 3)
```

Ở đây:

```text
4 → số mẫu
3 → số features
```

Ta có thể truy cập:

```python
X[0]      # mẫu đầu tiên
X[:, 0]   # feature đầu tiên
X[0, 1]   # giá trị tại hàng 0, cột 1
```

## Ma trận trong Neural Network

Một layer có thể có ma trận trọng số:

```python
W = np.array([
    [0.2, 0.5],
    [0.1, 0.3],
    [0.7, 0.8]
])

print(W.shape)
# (3, 2)
```

Nếu input có 3 features và layer có 2 neurons:

X∈RN×3X \in \mathbb{R}^{N\times3} W∈R3×2W \in \mathbb{R}^{3\times2}

thì:

XW∈RN×2XW \in \mathbb{R}^{N\times2}

## Điều cần nhớ khi code

Khi gặp một ma trận, hãy hỏi:

```text
Shape của nó là gì?
Mỗi chiều đại diện cho cái gì?
```

Ví dụ:

```python
print(X.shape)
print(W.shape)
```

Đây là một trong những thói quen quan trọng nhất khi lập trình ML/DL.

## Liên kết

[[Shape của dữ liệu trong ML-DL]]

[[Phép nhân ma trận trong Neural Network]]

[[Weights và Bias]]

[[Tensor trong Deep Learning]]