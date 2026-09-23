
## Câu hỏi

**Tại sao Neural Network sử dụng phép nhân ma trận?**

## Ý chính

Một layer cơ bản thực hiện:

$Z=XW+b$

Trong đó:

- $X$: input
    
- $W$: weight
    
- $b$: bias
    
- $Z$: output trước activation
    

## Ví dụ

Có 2 samples, mỗi sample có 3 features:

```python
import numpy as np

X = np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```

Shape:

```python
X.shape
# (2, 3)
```

Layer có 2 neurons:

```python
W = np.array([
    [0.1, 0.2],
    [0.3, 0.4],
    [0.5, 0.6]
])
```

Shape:

```python
W.shape
# (3, 2)
```

Thực hiện:

```python
Z = X @ W

print(Z)
print(Z.shape)
```

Shape:

```text
(2, 3) @ (3, 2)
       ↓
     (2, 2)
```

Kết quả có 2 samples và 2 outputs.

## Tại sao mỗi neuron nhận được nhiều feature?

Phép nhân ma trận thực chất thực hiện nhiều phép tính tuyến tính cùng lúc.

Ví dụ neuron thứ nhất:

z1=x1w1+x2w2+x3w3z_1 = x_1w_1+x_2w_2+x_3w_3

Neuron thứ hai:

z2=x1w1+x2w2+x3w3z_2 = x_1w_1+x_2w_2+x_3w_3

Thay vì tự viết từng phép tính, ma trận cho phép thực hiện tất cả cùng lúc:

```python
Z = X @ W
```

## Thêm bias

Bias thường có shape:

```text
(number_of_neurons,)
```

Ví dụ:

```python
b = np.array([0.1, 0.2])
```

Sau đó:

```python
Z = X @ W + b
```

NumPy sẽ thực hiện broadcasting.

## Trong PyTorch

```python
import torch

X = torch.tensor([
    [1., 2., 3.],
    [4., 5., 6.]
])

W = torch.tensor([
    [0.1, 0.2],
    [0.3, 0.4],
    [0.5, 0.6]
])

b = torch.tensor([0.1, 0.2])

Z = X @ W + b

print(Z)
print(Z.shape)
```

## Trong thực tế

Khi sử dụng PyTorch, ta thường không tự tạo `W` và `b`.

Thay vào đó:

```python
import torch.nn as nn

layer = nn.Linear(3, 2)
```

PyTorch sẽ tạo:

```text
weight
bias
```

và thực hiện phép biến đổi tương đương:

Z=XWT+bZ = XW^T+b

Cách PyTorch lưu `weight` khiến shape của weight thường là:

```text
(out_features, in_features)
```

Đây là lý do cần phân biệt **công thức toán học** và **cách framework lưu tensor**.

## Liên kết

[[Shape của dữ liệu trong ML-DL]]

[[Weights và Bias]]

[[Forward Propagation]]