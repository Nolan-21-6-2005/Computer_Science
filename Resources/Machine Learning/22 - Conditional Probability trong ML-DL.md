# Conditional Probability trong ML-DL

## Câu hỏi

Tại sao $P(Y|X)$ lại quan trọng trong ML?

## Công thức

Conditional probability:

$$
P(Y|X)
$$

được đọc là:

> Xác suất của $Y$ khi đã biết $X$.

Với các biến phù hợp:

$$
P(Y|X)=\frac{P(X,Y)}{P(X)}
$$

## ML nhìn bài toán như thế nào?

Classification có thể được nhìn dưới dạng:

$$
P(Y|X)
$$

Ví dụ:

```text
X = ảnh chiếc xe

P(car | image) = 0.95
P(bus | image) = 0.03
P(bike | image) = 0.02
```

Model nhận $X$ và ước lượng distribution của $Y$.

## Trong PyTorch

```python
logits = model(x)

probs = torch.softmax(logits, dim=1)
```

`probs` có thể được diễn giải như conditional probabilities theo cách model được thiết kế.

## Ghi nhớ

> Nhiều bài toán prediction trong ML có thể được hiểu là học hoặc xấp xỉ $P(Y|X)$.
