# Debug lỗi Shape trong ML/DL

## Câu hỏi

**Khi code ML/DL bị lỗi shape thì nên kiểm tra như thế nào?**

## Nguyên nhân phổ biến

Một trong những lỗi thường gặp:

```text
mat1 and mat2 shapes cannot be multiplied
```

Ví dụ:

```python
X.shape
# (32, 10)

W.shape
# (5, 2)
```

Ta muốn:

```python
X @ W
```

Nhưng:

(32×10)(5×2)(32\times10)(5\times2)

không hợp lệ vì:

10≠510 \neq 5

## Cách kiểm tra

Trước phép toán:

```python
print("X:", X.shape)
print("W:", W.shape)

Z = X @ W
```

Hãy kiểm tra quy tắc:

(m×n)(n×p)→(m×p)(m\times n)(n\times p) \rightarrow (m\times p)

## Ví dụ đúng

```python
X = torch.randn(32, 10)
W = torch.randn(10, 5)

Z = X @ W

print(Z.shape)
# torch.Size([32, 5])
```

Ta có:

(32×10)(10×5)→(32×5)(32\times10)(10\times5) \rightarrow (32\times5)

## Với Neural Network

Giả sử:

```python
model = nn.Sequential(
    nn.Linear(10, 20),
    nn.ReLU(),
    nn.Linear(20, 2)
)
```

Input phải có dạng:

```text
(batch_size, 10)
```

Ví dụ:

```python
x = torch.randn(32, 10)

output = model(x)

print(output.shape)
# (32, 2)
```

Có thể đọc model như:

```text
32 samples
   ↓
10 features
   ↓
Linear(10 → 20)
   ↓
20 features
   ↓
Linear(20 → 2)
   ↓
2 outputs
```

## Quy trình debug

Khi gặp lỗi shape:

```python
print("input:", x.shape)

for layer in model:
    x = layer(x)
    print(layer.__class__.__name__, x.shape)
```

Cách này giúp xác định **layer nào làm shape sai**.

## Quy tắc thực hành

Khi code Deep Learning, hãy tập thói quen:

```python
print(x.shape)
```

ở những điểm quan trọng.

Đặc biệt trước:

```python
Linear
Conv2d
MatMul
Attention
Loss
```

## Ý quan trọng

**Shape không chỉ là thông tin kỹ thuật. Shape cho biết dữ liệu đang được mô hình hiểu như thế nào.**

Ví dụ:

```text
(32, 10)
```

có thể hiểu là:

```text
32 samples × 10 features
```

Trong khi:

```text
(32, 3, 224, 224)
```

có thể hiểu là:

```text
32 images × 3 channels × 224 height × 224 width
```

Vì vậy, hiểu shape là một phần quan trọng để hiểu cách model hoạt động.