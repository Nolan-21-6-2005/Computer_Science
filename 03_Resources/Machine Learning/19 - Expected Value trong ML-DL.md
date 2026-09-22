# Expected Value trong ML-DL

## Câu hỏi

Expected value (kỳ vọng) có ý nghĩa gì trong ML?

## Công thức

Với biến rời rạc:

$$
E[X] = \sum_x xP(X=x)
$$

Với biến liên tục:

$$
E[X] = \int x f(x)\,dx
$$

Hiểu đơn giản:

> Expected value là giá trị trung bình có trọng số theo xác suất.

## Ví dụ

```python
import numpy as np

x = np.array([1, 2, 3, 4, 5])

mean = np.mean(x)
print(mean)
```

Trong thực tế, `np.mean()` trên dataset là một dạng ước lượng empirical của kỳ vọng.

## Liên hệ với ML

Expected value xuất hiện rất nhiều trong lý thuyết ML:

$$
E_{(X,Y)\sim P_{data}}[L(f(X),Y)]
$$

Đây là expected loss: loss trung bình mà model có thể tạo ra trên distribution dữ liệu.

Ta không biết toàn bộ distribution $P_{data}$ nên dùng dataset để xấp xỉ:

$$
\frac{1}{n}\sum_{i=1}^{n}L(f(x_i),y_i)
$$

Đó chính là empirical mean loss.

## Ghi nhớ

> Training loss trung bình có thể được xem như một cách ước lượng expected loss trên distribution dữ liệu.
