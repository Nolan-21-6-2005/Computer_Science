# Bias và Variance trong ML-DL

## Câu hỏi

Bias và variance giúp giải thích lỗi model như thế nào?

## Ý tưởng

Một model có thể mắc lỗi vì:

- bias cao: model quá đơn giản hoặc giả định không phù hợp;
- variance cao: model quá nhạy với training data.

Một biểu diễn kinh điển của expected squared error là:

$$
E[(Y-\hat f(X))^2]
=
Bias^2 + Variance + Noise
$$

## Ví dụ trực quan

### Bias cao

```text
Model quá đơn giản
→ không bắt được pattern
→ thường underfit
```

### Variance cao

```text
Model quá nhạy với training data
→ học cả noise
→ thường overfit
```

## Liên hệ code

Regularization có thể giúp kiểm soát complexity:

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=1e-3,
    weight_decay=1e-4
)
```

Dropout cũng là một kỹ thuật liên quan đến regularization.

## Ghi nhớ

> Bias và variance là hai khía cạnh khác nhau của sai số tổng quát hóa; chúng giúp ta suy nghĩ về underfitting và overfitting.
