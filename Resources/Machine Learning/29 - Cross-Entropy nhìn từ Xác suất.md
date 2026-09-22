# Cross-Entropy nhìn từ Xác suất

## Câu hỏi

Cross-Entropy Loss thực chất đang phạt điều gì?

## Classification

Giả sử model dự đoán:

```text
cat = 0.1
dog = 0.8
bird = 0.1
```

Nếu label thật là `cat`, model đã gán xác suất thấp cho class đúng.

Cross-entropy:

$$
L=-\sum_c y_c\log p_c
$$

Với one-hot target, chỉ còn:

$$
L=-\log p_{true}
$$

## Ví dụ

Nếu:

$$
p_{true}=0.9
$$

thì:

$$
L=-\log(0.9)
$$

Nếu:

$$
p_{true}=0.1
$$

thì loss lớn hơn rất nhiều.

## PyTorch

```python
loss_fn = torch.nn.CrossEntropyLoss()

loss = loss_fn(logits, target)
```

Điểm quan trọng:

> `CrossEntropyLoss` trong PyTorch nhận logits, không cần gọi `softmax()` trước.

```python
loss = loss_fn(logits, target)
```

Nếu cần probability để xem kết quả:

```python
probs = torch.softmax(logits, dim=1)
```

## Ghi nhớ

> Cross-entropy khuyến khích model gán xác suất cao cho class đúng và phạt mạnh những dự đoán tự tin nhưng sai.
