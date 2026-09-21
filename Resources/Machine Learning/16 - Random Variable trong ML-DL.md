# Random Variable trong ML-DL

## Câu hỏi

Trong ML/DL, "random variable" thực sự đại diện cho cái gì?

## Ý chính

Random variable (biến ngẫu nhiên) là một biến có giá trị phụ thuộc vào một quá trình ngẫu nhiên.

Trong ML, ta thường xem dữ liệu như những giá trị được sinh ra từ một quá trình ngẫu nhiên:

$$
X \sim P(X)
$$

Ví dụ:

- $X$: ảnh đầu vào.
- $Y$: nhãn.
- $X$ và $Y$: các biến ngẫu nhiên.

Mục tiêu của model thường liên quan đến việc học quan hệ:

$$
P(Y|X)
$$

## Liên hệ với code

Một dataset không chỉ là một danh sách số cố định. Ta có thể xem mỗi sample là một lần quan sát của biến ngẫu nhiên.

```python
x = dataset[0]

# x là một observation (một lần quan sát)
# của biến ngẫu nhiên X.
```

Nếu dataset có nhiều sample:

```python
for x, y in dataset:
    # Mỗi (x, y) là một observation
    # được lấy từ quá trình sinh dữ liệu.
    pass
```

## Tại sao cần hiểu?

Điều này giúp hiểu:

- dataset là các observations;
- train/test là các mẫu được lấy từ dữ liệu;
- model cố gắng học quy luật từ các observations;
- prediction có thể được hiểu dưới góc nhìn xác suất.

## Ghi nhớ

> Random variable là cách toán học hóa những đại lượng chưa biết trước giá trị và được quan sát thông qua dữ liệu.
