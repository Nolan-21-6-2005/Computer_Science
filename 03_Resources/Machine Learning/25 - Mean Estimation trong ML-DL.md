# Mean Estimation trong ML-DL

## Câu hỏi

Tại sao `mean()` lại quan trọng trong ML?

## Ý tưởng

Ta thường muốn biết expected value thật:

$$
\mu=E[X]
$$

Nhưng không biết distribution thật.

Ta dùng sample mean:

$$
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
$$

để ước lượng $\mu$.

## Python

```python
mean = np.mean(x)
```

## ML

Training loss thường được tính bằng mean:

$$
\hat{L}=
\frac{1}{n}
\sum_{i=1}^{n}L_i
$$

Ví dụ:

```python
loss = loss_fn(pred, target)

# Nếu loss_fn trả về loss của từng sample:
mean_loss = loss.mean()
```

PyTorch thường thực hiện reduction theo mean trong nhiều loss function mặc định.

## Tại sao không lấy một sample?

Một sample có thể chứa noise.

Khi tăng số lượng observations, estimate thường ổn định hơn nếu các giả định sampling phù hợp.

## Ghi nhớ

> Mean trong ML thường là một estimator dùng để xấp xỉ expected value từ dữ liệu quan sát.
