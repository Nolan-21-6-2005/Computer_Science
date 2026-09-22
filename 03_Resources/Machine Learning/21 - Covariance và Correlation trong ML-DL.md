# Covariance và Correlation trong ML-DL

## Câu hỏi

Covariance và correlation dùng để kiểm tra quan hệ giữa các feature như thế nào?

## Covariance

Covariance đo xu hướng hai biến thay đổi cùng nhau:

$$
Cov(X,Y)=E[(X-E[X])(Y-E[Y])]
$$

- dương: thường tăng cùng nhau;
- âm: một biến tăng thì biến kia thường giảm;
- gần 0: ít có quan hệ tuyến tính.

## Correlation

Correlation chuẩn hóa covariance:

$$
\rho_{X,Y}=
\frac{Cov(X,Y)}{\sigma_X\sigma_Y}
$$

Giá trị thường nằm trong:

$$
-1\le\rho\le1
$$

## Python

```python
import pandas as pd

corr = df.corr(numeric_only=True)

print(corr)
```

## Dùng trong ML

Correlation có thể hỗ trợ:

- EDA;
- phát hiện feature có quan hệ mạnh;
- phát hiện feature redundancy;
- hiểu dataset trước khi train.

Nhưng:

> Correlation không chứng minh causation.

Feature có correlation cao cũng không tự động có nghĩa feature đó gây ra target.

## Ghi nhớ

> Covariance đo mức thay đổi cùng nhau; correlation chuẩn hóa mối quan hệ tuyến tính về một thang dễ so sánh.
