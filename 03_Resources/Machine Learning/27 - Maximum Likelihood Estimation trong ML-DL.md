# Maximum Likelihood Estimation trong ML-DL

## Câu hỏi

Maximum Likelihood Estimation (MLE) là gì và tại sao nó xuất hiện trong ML?

## Ý tưởng

Ta giả sử dữ liệu được sinh từ một model có parameter $\theta$.

Likelihood:

$$
L(\theta)=P(D|\theta)
$$

MLE chọn parameter làm likelihood của dữ liệu quan sát lớn nhất:

$$
\hat{\theta}
=
\arg\max_\theta P(D|\theta)
$$

## Log-Likelihood

Thay vì tối ưu trực tiếp likelihood, thường tối ưu log-likelihood:

$$
\hat{\theta}
=
\arg\max_\theta \log P(D|\theta)
$$

Vì log biến phép nhân thành phép cộng:

$$
\log\prod_i p_i
=
\sum_i\log p_i
$$

## Liên hệ với optimization

Maximize log-likelihood tương đương minimize negative log-likelihood:

$$
\min_\theta -\log P(D|\theta)
$$

Đây chính là cấu trúc rất quen thuộc trong ML.

## Ghi nhớ

> MLE biến việc "tìm parameter phù hợp với dữ liệu" thành một bài toán optimization.
