# Probability Distribution trong ML-DL

## Câu hỏi

Distribution dùng để mô tả điều gì trong ML/DL?

## Ý chính

Probability distribution mô tả cách xác suất được phân bố trên các giá trị có thể xảy ra của một biến ngẫu nhiên.

Ví dụ:

$$
X \sim P(X)
$$

có nghĩa là $X$ tuân theo distribution $P(X)$.

Trong ML, distribution có thể mô tả:

- dữ liệu đầu vào;
- nhãn;
- prediction;
- noise;
- error;
- uncertainty.

## Ví dụ

Giả sử dữ liệu chiều cao:

```python
import numpy as np

heights = np.array([165, 170, 172, 168, 180, 175])
```

Ta đang quan sát một số giá trị được sinh ra từ một distribution nào đó.

Ta thường không biết distribution thật sự là gì, nhưng có thể ước lượng nó từ dữ liệu.

## Trong classification

Model có thể tạo ra distribution trên các class:

```python
probs = torch.softmax(logits, dim=1)
```

Ví dụ:

```text
[0.05, 0.90, 0.05]
```

Đây có thể được hiểu là phân phối xác suất dự đoán trên 3 class.

## Ghi nhớ

> Distribution cho biết xác suất được phân bố như thế nào trên các giá trị hoặc sự kiện có thể xảy ra.
