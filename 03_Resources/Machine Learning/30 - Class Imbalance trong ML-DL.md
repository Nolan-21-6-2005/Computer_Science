# Class Imbalance trong ML-DL

## Câu hỏi

Tại sao accuracy cao nhưng model vẫn có thể hoạt động kém?

## Ví dụ

Dataset:

```text
Class A: 950 samples
Class B: 50 samples
```

Nếu model luôn đoán A:

$$
Accuracy=95\%
$$

Nhưng model không nhận diện được B.

## Vấn đề thống kê

Phân phối class trong dataset bị lệch:

$$
P(Y=A)\gg P(Y=B)
$$

Nếu chỉ nhìn accuracy, ta có thể bỏ qua class thiểu số.

## Cách kiểm tra

```python
print(df["label"].value_counts())
print(df["label"].value_counts(normalize=True))
```

## Một số hướng xử lý

Tùy bài toán:

- class weights;
- oversampling;
- undersampling;
- balanced batch;
- đánh giá bằng precision/recall/F1;
- confusion matrix.

Ví dụ class weight:

```python
loss_fn = torch.nn.CrossEntropyLoss(
    weight=class_weights
)
```

## Ghi nhớ

> Khi distribution của class bị lệch, metric tổng thể như accuracy có thể không phản ánh đầy đủ hiệu năng trên từng class.
