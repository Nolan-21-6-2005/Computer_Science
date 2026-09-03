---
tags:
---

Input: input dữ liệu tuyến tính.
Output: dữ liệu phi tuyến.

Mục tiêu: Học quy luật của dữ liệu thông qua trọng số.

![[Neural net.mp4]]

Bước 1: Nhận dữ liệu.
Bước 2: Khởi tạo trọng số ngẫu nhiên.
Bước 3: Đánh giá độ chính xác.
Bước 4: Cập nhật lại trọng số sao cho sai số nhỏ nhất có thể.
# Convolution Neural Network

Input: một bức ảnh.
Output: Vector đặc trưng.
## Các thông tin cơ bản

### Kernel

Là bộ lọc thứ sẽ lấy pattern trên các điểm ảnh và đưa vào từng neural. Kích thước của kernel là kích thước đầu vào của một neural.

VD về kernel:

![[Convolution_arithmetic_-_Padding_strides.gif|]]

Như trong ảnh thì: 

- Kernel 3x3.
- Strike = 1.
- Padding là các ô vuông có nét đứt viền ngoài khung ảnh gốc.
- Kích thước đầu vào của một neuron 3x3 = 9.
- Strike: là tốc độ trượt qua các điểm ảnh.
- Padding: Là bộ đệm của bộ lọc.

### Feature Map

Là ma trận đầu ra thu được sau khi một **kernel (filter)** quét qua ảnh hoặc feature map ở lớp trước.
- Activation Function: chuyển đổi giá trị tuyến tính thành giá trị phi tuyến.
- Pooling: Là phép toán giúp giảm kích thước Feature Map và giữ các đặc trưng quan trọng (các điểm ảnh cho thấy hình dạng của vật thể).
### Flatten
Là phép toán biến Feature Map thành vector 1 chiều.

# Recurrent Neural Network

Input: một văn bản được embedding thành một vector số nhiều chiều.
Output: các hidden state (đặc trưng của từ tố).

Mục tiêu: Học cách trích xuất đặc trưng thông qua trọng số.

![[rnn_architecture.png]]

1. Nhận vector đầu vào.
2. Khởi tạo trọng số sao cho các điểm dữ nằm ở vị trí hợp lý trên đồ thị (tức là có thể biểu diễn ngữ cảnh một cách hợp lý).
3. Đánh giá đầu ra. 
4. Cập nhật lại sao cho sai số nhỏ nhất.

Ưu điểm:

- Yêu cầu phần cứng không quá cao.
- Biết thứ tự của từ tố trong câu văn.

Nhược điểm: 

- Chậm do phải thực hiện trích xuất tuần tự.

# BERT

Input: Một vector số được embedding từ một văn bản.
Output: các đặc trưng của từ vựng trong văn bản.

Mục tiêu: học cách trích xuất đặc trưng thông qua trọng số.

![[transformer_encoder.webp|700]]

![[images.png|228]]
1. Nhận đầu vào đã được mã hóa vị trí.
2. Xác định mức độ liên quan giữa các token dựa trên các trong số khởi tạo.
3. Đánh giá độ chính xác của kết quả.
4. Cập nhật lại trọng số.