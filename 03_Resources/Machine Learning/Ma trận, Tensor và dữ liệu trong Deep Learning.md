## Câu hỏi

**Nếu Deep Learning không chỉ sử dụng ma trận thì tensor khác ma trận như thế nào?**

## Scalar

Một giá trị đơn:

```python
x = 5
```

Có thể xem là tensor 0 chiều.

x∈Rx \in \mathbb{R}

## Vector

Một dãy giá trị:

```python
x = [1, 2, 3]
```

Shape:

```text
(3,)
```

Đây là tensor 1 chiều.

## Matrix

Hai chiều:

```python
X = [
    [1, 2],
    [3, 4]
]
```

Shape:

```text
(2, 2)
```

## Tensor

Tensor tổng quát hóa các cấu trúc nhiều chiều.

Ví dụ một batch ảnh RGB:

```python
images.shape
# (32, 3, 224, 224)
```

Có 4 chiều:

```text
32 → batch
3  → channels
224 → height
224 → width
```

## Có thể hiểu như sau

Scalar→Vector→Matrix→Tensor\text{Scalar} \rightarrow \text{Vector} \rightarrow \text{Matrix} \rightarrow \text{Tensor}

Không phải tensor là một loại hoàn toàn khác ma trận.

**Ma trận là một trường hợp đặc biệt của tensor có 2 chiều.**

## Khi lập trình

Trong PyTorch:

```python
import torch

x = torch.tensor(5)
print(x.ndim)
# 0

x = torch.tensor([1, 2, 3])
print(x.ndim)
# 1

x = torch.tensor([
    [1, 2],
    [3, 4]
])
print(x.ndim)
# 2

x = torch.randn(32, 3, 224, 224)
print(x.ndim)
# 4
```

Có thể kiểm tra:

```python
print(x.shape)
print(x.ndim)
```

## Điều cần nhớ

Khi lập trình Deep Learning, đừng chỉ hỏi:

> "Đây có phải là ma trận không?"

Hãy hỏi:

> **"Tensor này có bao nhiêu chiều và mỗi chiều đại diện cho cái gì?"**

Đây là cách tư duy hữu ích hơn khi làm việc với PyTorch, TensorFlow hoặc NumPy.