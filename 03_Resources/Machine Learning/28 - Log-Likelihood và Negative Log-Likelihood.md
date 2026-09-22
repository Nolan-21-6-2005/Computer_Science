# Log-Likelihood và Negative Log-Likelihood

## Câu hỏi

Tại sao ML thường dùng log-likelihood thay vì likelihood?

## Likelihood

Nếu các observations độc lập:

$$
P(D|\theta)=\prod_{i=1}^{n}P(x_i|\theta)
$$

Khi $n$ lớn, tích của nhiều số nhỏ có thể rất nhỏ.

## Log

Lấy log:

$$
\log P(D|\theta)
=
\sum_{i=1}^{n}\log P(x_i|\theta)
$$

Việc này:

- tránh nhiều phép nhân nhỏ;
- dễ tính toán hơn;
- biến tích thành tổng;
- giữ nguyên nghiệm tối ưu vì log là hàm đơn điệu tăng.

## Negative Log-Likelihood

Optimization thường là minimization:

$$
NLL=-\log P(D|\theta)
$$

Ví dụ:

```python
loss = -torch.log(prob).mean()
```

Trong thực tế nên dùng các loss/API ổn định số học thay vì tự tính log của probability nếu framework đã cung cấp.

## Liên hệ Cross-Entropy

Classification với one-hot target có:

$$
L=-\sum_c y_c\log p_c
$$

Đây chính là negative log-likelihood trong nhiều thiết lập classification.

## Ghi nhớ

> Log-likelihood biến tích xác suất thành tổng; negative log-likelihood biến bài toán maximize likelihood thành minimize loss.
