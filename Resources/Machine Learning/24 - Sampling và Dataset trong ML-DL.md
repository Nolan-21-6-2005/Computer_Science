# Sampling và Dataset trong ML-DL

## Câu hỏi

Dataset liên quan gì đến sampling?

## Ý chính

Ta thường không có toàn bộ population mà chỉ quan sát một sample.

```text
Population
    ↓ sampling
Dataset
    ↓ train/validation/test
Model
```

Ví dụ:

```python
train_loader = DataLoader(
    dataset,
    batch_size=32,
    shuffle=True
)
```

Mỗi batch có thể được xem là một nhóm observations được lấy ra từ dataset.

## Tại sao cần sampling?

Vì dữ liệu thật có thể rất lớn.

Ta muốn dùng một tập dữ liệu hữu hạn để ước lượng các tính chất của distribution thật.

Ví dụ:

$$
\mu = E[X]
$$

được ước lượng bằng sample mean:

$$
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
$$

## Train / Validation / Test

Việc chia dataset giúp kiểm tra model trên những observations không được dùng để fit parameters.

Một cách đơn giản:

```python
train, val, test = ...
```

## Cảnh báo

Nếu dataset không đại diện cho population mục tiêu, model có thể học một distribution bị lệch.

## Ghi nhớ

> Dataset là tập observations hữu hạn được dùng để suy ra quy luật từ một distribution mà ta thường không quan sát trực tiếp.
