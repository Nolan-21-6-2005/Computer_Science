Input: Một vector số được embedding từ một văn bản.
Output: các đặc trưng của từ vựng trong văn bản.

Mục tiêu: học cách trích xuất đặc trưng thông qua trọng số.

![[transformer_encoder.webp]]

1. Nhận đầu vào đã được mã hóa vị trí.
2. Xác định mức độ liên quan giữa các token dựa trên các trong số khởi tạo.
3. Đánh giá độ chính xác của kết quả.
4. Cập nhật lại trọng số.

[[Positional Encoding]]: Mã hóa vị trí.
[[Multi-head Attention]]: Học cách trích xuất đặc trưng.
[[Feed-Forward]]: Học các đặc trưng.