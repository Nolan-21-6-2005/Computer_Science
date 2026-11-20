# Cách tính Gini Index 
Công thức:
$$Gini(D)=1-\sum_{i=1}^{k}p_i^2$$
Trong đó:

- $p_i$: Tỷ lệ mẫu thuộc lớp thứ $i$.
- $k$: Số lớp phân loại.

Giả sử có tập dữ liệu:

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
Các ngưỡng tuổi:

| Hai giá trị tuổi | Threshold |
| ---------------- | --------- |
| 20 và 22         | 21        |
| 22 và 25         | 23.5      |
| 25 và 30         | 27.5      |
| 30 và 35         | 32.5      |
| 35 và 40         | 37.5      |
| 40 và 45         | 42.5      |
| 45 và 50         | 47.5      |
Chọn ngưỡng Age < 27.5

## **Nhánh trái: Age dưới 27.5**

| Nhãn  | Số lượng |
| ----- | -------- |
| Không | 2        |
| Có    | 1        |
$$Gini_L=1-(2/3)^2-(1/3)^2$$
$$Gini_L=0.444$$
## **Nhánh phải: Age từ 27.5 trở lên**

| Nhãn  | Số lượng |
| ----- | -------- |
| Không | 1        |
| Có    | 4        |
$$Gini_R=1-(1/5)^2-(4/5)^2$$
$$Gini_R=0.32$$

Gini sau khi phân chia được tính bằng trung bình có trọng số:

$$Gini_{split}= \frac{3}{8}Gini_L+ \frac{5}{8}Gini_R$$
$$Gini_{split}=0.367$$
