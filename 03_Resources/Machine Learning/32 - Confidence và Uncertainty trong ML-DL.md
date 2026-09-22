# Confidence và Uncertainty trong ML-DL

## Câu hỏi

Model dự đoán `0.95` có thực sự nghĩa là "95% chắc chắn" không?

## Hai khái niệm cần phân biệt

### Confidence score

Ví dụ:

```python
probs = torch.softmax(logits, dim=1)

confidence, predicted = probs.max(dim=1)
```

`confidence` có thể là xác suất lớn nhất mà model xuất ra.

### Uncertainty

Uncertainty rộng hơn confidence.

Model có thể:

```text
prediction = cat
confidence = 0.95
```

nhưng probability đó chưa chắc đã được calibration tốt.

## Calibration

Nếu một nhóm prediction có confidence khoảng 0.8, một model được calibration tốt sẽ có accuracy thực tế khoảng 80% trên nhóm đó.

Do đó:

> Probability output không tự động đồng nghĩa với xác suất đúng thực tế.

## Tại sao quan trọng?

Đặc biệt quan trọng trong:

- medical AI;
- autonomous systems;
- anomaly detection;
- decision support;
- production monitoring.

## Ghi nhớ

> Confidence là giá trị model xuất ra; uncertainty là khái niệm rộng hơn về mức độ không chắc chắn của prediction.
