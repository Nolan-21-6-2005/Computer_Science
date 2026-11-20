# Làm thế nào neural network biết được nó cần nhận biết các đặc trưng gì của ảnh


> [!question] Câu hỏi
> Làm sao để giúp AI biết nó phải học những gì, làm những gì ?

Cần tổ chức dữ liệu thành dạng bảng cùng với nhãn tương ứng với các ảnh để máy tính học:
- Đối với bài toán phân loại:

| Ảnh đầu vào | Nhãn (Label) |
| ----------- | ------------ |
| Ảnh 1       | Cat          |
| Ảnh 2       | Dog          |
| Ảnh 3       | Bird         |
- Đối với bài toán nhận diện vật thể thì có thêm bounding box:

| Ảnh đầu vào | Nhãn (Label) | Bounding box |
| ----------- | ------------ | ------------ |
| Ảnh 1       | Cat          | (x, y, w, h) |
| Ảnh 2       | Dog          | (x, y, w, h) |
| Ảnh 3       | Bird         | (x, y, w, h) |
