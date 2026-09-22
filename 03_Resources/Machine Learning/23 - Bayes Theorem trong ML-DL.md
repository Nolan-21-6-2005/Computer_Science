# Bayes Theorem trong ML-DL

## Câu hỏi

Bayes theorem liên quan gì đến ML?

## Công thức

$$
P(A|B)=
\frac{P(B|A)P(A)}{P(B)}
$$

Trong ML, một dạng rất quan trọng là:

$$
P(Y|X)=
\frac{P(X|Y)P(Y)}{P(X)}
$$

## Ý nghĩa

Ta có:

- $P(Y)$: prior;
- $P(X|Y)$: likelihood;
- $P(X)$: evidence;
- $P(Y|X)$: posterior.

Bayes cho phép cập nhật niềm tin về $Y$ sau khi quan sát $X$.

## Ví dụ

Trong bài toán phân loại email:

```text
Y = spam / not spam
X = các đặc điểm của email
```

Ta muốn:

$$
P(spam|X)
$$

Bayes cho ta một cách liên hệ nó với $P(X|spam)$ và prior $P(spam)$.

## Trong ML hiện đại

Không phải mọi neural network đều được xây dựng trực tiếp bằng Bayes theorem.

Tuy nhiên, tư duy Bayes là nền tảng của:

- Bayesian inference;
- probabilistic models;
- uncertainty estimation;
- Naive Bayes;
- Bayesian neural networks.

## Ghi nhớ

> Bayes theorem mô tả cách cập nhật xác suất của một giả thuyết khi có thêm bằng chứng.
