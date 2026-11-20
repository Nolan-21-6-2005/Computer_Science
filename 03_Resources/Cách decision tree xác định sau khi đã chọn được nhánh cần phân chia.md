# Cách decision tree xác định ngưỡng sau khi đã chọn được nhánh cần phân chia

Giả sử có dataset

| Người | Age | Mua máy tính |
| ----- | --- | ------------ |
| A     | 20  | Không        |
| B     | 22  | Không        |
| C     | 25  | Có           |
| D     | 30  | Có           |
| E     | 35  | Có           |
| F     | 40  | Có           |
| G     | 45  | Không        |
| H     | 50  | Có           |
## Ngưỡng phân chia ứng viên
$$Threshold = \frac{Age_i+Age_{i+1}}{2}$$

| Hai giá trị tuổi | Threshold |
| ---------------- | --------- |
| 20 và 22         | 21        |
| 22 và 25         | 23.5      |
| 25 và 30         | 27.5      |
| 30 và 35         | 32.5      |
| 35 và 40         | 37.5      |
| 40 và 45         | 42.5      |
| 45 và 50         | 47.5      |
## Đánh giá từng ngưỡng bằng Gini Index
Công thức:
$$Gini(D)=1-\sum_{i=1}^{k}p_i^2$$
Trong đó:

- $p_i$: Tỷ lệ mẫu thuộc lớp thứ $i$.
- $k$: Số lớp phân loại.

Gini càng thấp thì nhóm dữ liệu càng thuần nhất ([[Cách tính Gini Index cho từng ngưỡng]]).
## So sánh các ngưỡng

|Threshold|Gini sau phân chia|
|---|---|
|21|0.429|
|23.5|0.375|
|27.5|0.367|
|32.5|0.417|
|37.5|0.417|
|42.5|0.429|
|47.5|0.375|

Threshold = 27.5 vì có Gini sau phân chia thấp nhất.