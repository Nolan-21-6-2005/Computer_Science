## Câu hỏi

**Trong code, Weight và Bias thực sự là gì?**

## Weight

Weight là các tham số mà model học trong quá trình training.

Với một layer:

```python
import torch.nn as nn

layer = nn.Linear(3, 2)
```

Có thể xem layer có:

```text
3 input features
2 output neurons
```

Kiểm tra:

```python
print(layer.weight.shape)
# torch.Size([2, 3])

print(layer.bias.shape)
# torch.Size([2])
```

Weight có shape:

(2,3)(2,3)

Bias có shape:

(2,)(2,)

## Tại sao weight là `(2, 3)`?

Vì PyTorch lưu weight theo:

(out_features, in_features)(out\_features,\ in\_features)

Tức:

```text
2 neurons
3 weights cho mỗi neuron
```

Có thể hình dung:

```text
Neuron 1 → w11 w12 w13
Neuron 2 → w21 w22 w23
```

tạo thành:

W=[w11w12w13w21w22w23]W = \begin{bmatrix} w_{11}&w_{12}&w_{13}\\ w_{21}&w_{22}&w_{23} \end{bmatrix}

## Bias

Mỗi output neuron thường có một bias riêng:

b=[b1b2]b = \begin{bmatrix} b_1\\ b_2 \end{bmatrix}

Bias cho phép layer dịch chuyển kết quả sau phép biến đổi tuyến tính.

## Xem parameters

```python
for name, parameter in layer.named_parameters():
    print(name)
    print(parameter.shape)
```

Có thể nhận được:

```text
weight
torch.Size([2, 3])

bias
torch.Size([2])
```

## Điều quan trọng khi lập trình

**Weight và bias là những giá trị model cần học.**

Chúng không phải dữ liệu đầu vào.

Có thể phân biệt:

```text
Input
  ↓
X

Model parameters
  ↓
W, b

Output
  ↓
Z
```

Trong quá trình training:

W,b→gradient→optimizer→W,b mớiW,b \rightarrow \text{gradient} \rightarrow \text{optimizer} \rightarrow W,b\ \text{mới}

## Kiểm tra model

Một thói quen hữu ích:

```python
for name, parameter in model.named_parameters():
    print(name, parameter.shape)
```

Điều này giúp hiểu model đang có những tham số nào và kích thước của chúng ra sao.