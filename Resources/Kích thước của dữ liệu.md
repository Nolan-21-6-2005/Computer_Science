# Shape của dữ liệu trong ML/DL

## Câu hỏi

**Shape của một ma trận/tensor trong ML/DL có ý nghĩa gì?**

## Ý chính

`shape` cho biết kích thước của từng chiều trong dữ liệu.

Ví dụ:

```python
X.shape
# (100, 5)
```

có thể hiểu là:

```text
100 samples
5 features
```

Tức là:

X∈R100×5X \in \mathbb{R}^{100\times5}

## Dataset dạng bảng

Thông thường:

```text
(samples, features)
```

Ví dụ:

```python
X.shape
# (1000, 20)
```

nghĩa là:

```text
1000 mẫu
20 đặc trưng / mẫu
```

## Ảnh

Một ảnh grayscale:

```text
(height, width)
```

Ví dụ:

```python
image.shape
# (28, 28)
```

Ảnh RGB:

```text
(height, width, channels)
```

Ví dụ:

```python
image.shape
# (224, 224, 3)
```

## Batch ảnh

Khi đưa nhiều ảnh vào model:

```text
(batch, height, width, channels)
```

Ví dụ:

```python
images.shape
# (32, 224, 224, 3)
```

nghĩa là:

```text
32 ảnh
224 pixel chiều cao
224 pixel chiều rộng
3 kênh màu
```

## Trong PyTorch

Có thể kiểm tra:

```python
import torch

x = torch.randn(32, 3, 224, 224)

print(x.shape)
# torch.Size([32, 3, 224, 224])
```

Ở đây PyTorch thường sử dụng thứ tự:

```text
(batch, channels, height, width)
```

khác với nhiều thư viện xử lý ảnh sử dụng:

```text
(height, width, channels)
```

## Tại sao shape quan trọng?

Rất nhiều lỗi khi lập trình Neural Network thực chất là **shape mismatch**.

Ví dụ:

```python
X.shape
# (100, 3)

W.shape
# (4, 2)
```

Không thể thực hiện:

```python
X @ W
```

vì:

(100×3)(4×2)(100\times3)(4\times2)

không hợp lệ.

Chiều bên trong phải bằng nhau:

(100×3)(3×2)(100\times3)(3\times2)

mới hợp lệ.

## Quy tắc cần nhớ

Với:

Am×nBn×pA_{m\times n}B_{n\times p}

kết quả có shape:

Cm×pC_{m\times p}

Có thể nhớ:

```text
(m × n) @ (n × p)
        ↓
      (m × p)
```

## Khi debug model

Hãy thường xuyên kiểm tra:

```python
print(x.shape)
print(weight.shape)
print(bias.shape)
print(output.shape)
```

Đừng chỉ nhìn dữ liệu; hãy nhìn **shape của dữ liệu**.