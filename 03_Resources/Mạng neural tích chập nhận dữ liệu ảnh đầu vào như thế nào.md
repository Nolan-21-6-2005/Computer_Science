# Mạng neural tích chập nhận dữ liệu ảnh đầu vào như thế nào ?

$$
N_{\text{Output Channels}} = N_{\text{Filters}} = N_{\text{Feature Map}} = N_{\text{Convolution Neural in 1 layer}}
$$

Mỗi một neuron sẽ nhận vào một feature map có kích thước được xác định bởi người thiết kế.
## Làm thế nào để biết được số lượng kênh đầu vào mà các lớp tích chập sẽ phải nhận ?

Lớp đầu tiên (dựa trên loại ảnh):

| Loại dữ liệu  | Input Channels |
| ------------- | -------------- |
| Ảnh Grayscale | 1              |
| Ảnh RGB       | 3              |
| Ảnh RGBA      | 4              |
Ví dụ:
 $$Input = 224 \times 224 \times 3$$
Vậy lớp đầu tiên nhận 3 kênh.
# Link
- [[Sự thay đổi của feature map sau các lớp tích chập]]
- 


