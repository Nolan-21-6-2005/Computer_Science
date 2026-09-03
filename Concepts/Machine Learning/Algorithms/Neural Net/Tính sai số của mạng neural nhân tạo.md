Coi công thức của một neuron là:
$$n_j​=\sum_i w_{ji}​x_i​+b_j​$$
Thì [[hàm kích hoạt]] của mạng neuron là: $f^n[n^n_i(k)]$.

Tính sai số neuron ngõ ra:
$$\Delta_i(k) = -\frac{E(k)}{n^2_i(k)}=[t_i(k)-a_i(k)]f'^2[n^2_i(k)]$$
Trong đó:
- $t_i(k)$: ngõ ra mong muốn tại $k$.
- $a_i(k)$: giá trị dự đoán sau một batch.
- $f'^2[n^2_i(k)]$: đạo hàm của hàm kích hoạt.

Tính sai số các nơ-ron ẩn:
$$\delta_i(k)=f'^1[n​^1_i(k)]\sum_j ​\Delta_j​(k)w_{ji}​$$

Trong đó:
- $​\Delta_j​(k)w_{ji}$: sai số truyền ngược từ khi thực hiện lan truyền ngược.
- $f'^1[n​^1_i(k)]$:  đạo hàm của hàm kích hoạt.
