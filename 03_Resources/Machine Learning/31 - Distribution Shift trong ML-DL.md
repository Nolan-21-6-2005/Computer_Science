# Distribution Shift trong ML-DL

## Câu hỏi

Tại sao model chạy tốt trên test nhưng ra thực tế lại kém?

## Ý tưởng

Ta thường giả định dữ liệu train và dữ liệu thực tế có distribution tương tự.

Một cách viết đơn giản:

$$
P_{train}(X,Y)\approx P_{test}(X,Y)
$$

Nếu distribution thay đổi:

$$
P_{train}(X,Y)\ne P_{real}(X,Y)
$$

thì performance có thể thay đổi.

## Ví dụ

Model nhận diện camera trong lớp học:

```text
Training:
- camera sáng
- góc cố định
- ảnh rõ

Production:
- ánh sáng yếu
- camera rung
- góc thay đổi
```

Đây là một dạng distribution shift.

## Liên hệ code

Không chỉ nhìn:

```python
train_loss
val_loss
```

mà cần kiểm tra dữ liệu thực tế.

Ví dụ:

```python
print(train_df.describe())
print(real_df.describe())
```

Hoặc kiểm tra distribution của từng feature.

## Ghi nhớ

> Generalization không chỉ là "test accuracy cao"; model cần hoạt động tốt trên distribution mà hệ thống thực tế sẽ gặp.
