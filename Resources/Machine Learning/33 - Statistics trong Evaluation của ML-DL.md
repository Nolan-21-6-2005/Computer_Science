# Statistics trong Evaluation của ML-DL

## Câu hỏi

Tại sao evaluation cũng cần xác suất thống kê?

## Không chỉ có accuracy

Một model có thể được đánh giá bằng:

- accuracy;
- precision;
- recall;
- F1-score;
- ROC-AUC;
- MAE;
- MSE.

Mỗi metric trả lời một câu hỏi khác nhau.

## Ví dụ classification

```python
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score
)
```

Các metric được tính từ các observations của test set.

## Mean metric

Ví dụ MSE:

$$
MSE=
\frac{1}{n}
\sum_{i=1}^{n}(y_i-\hat y_i)^2
$$

Đây là một statistic được tính từ sample.

## Quan trọng

Một metric trên một test set không phải là toàn bộ sự thật về performance trên mọi dữ liệu tương lai.

Kết quả phụ thuộc vào:

- cách sampling;
- kích thước test set;
- distribution;
- noise;
- metric được chọn.

## Ghi nhớ

> Evaluation là quá trình dùng các thống kê trên dữ liệu quan sát để ước lượng cách model hoạt động trên dữ liệu chưa thấy.
