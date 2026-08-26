---
tags:
---
Input: một văn bản được embedding thành một vector số nhiều chiều.
Output: các hidden state.

Mục tiêu: Học cách trích xuất đặc trưng thông qua trọng số.

![[rnn_architecture.png]]

1. Nhận vector đầu vào.
2. Khởi tạo trọng số sao cho các điểm dữ nằm ở vị trí hợp lý trên đồ thị (tức là có thể biểu diễn ngữ cảnh một cách hợp lý)
3. Đánh giá đầu ra. 
4. Cập nhật lại sao cho sai số nhỏ nhất.

Ưu điểm:

- Yêu cầu phần cứng không quá cao.
- Biết thứ tự của từ tố trong câu văn.

Nhược điểm: 

- Chậm do phải thực hiện trích xuất tuần tự.
