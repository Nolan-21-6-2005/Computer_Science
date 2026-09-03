# Mục tiêu

Chuyển dạng dữ liệu tuyến tính thành dạng dữ liệu phi tuyến.

![[dao_ham.jpg]]
# Đạo hàm bậc nhất
## Đạo hàm của hàm nhiều biến

$$
f(x,y)=x^2+3xy+y^2
$$
### Đạo hàm theo x

Coi y là hằng số.
$$
\frac{\partial f}{\partial x} = 2x+3y
$$
### Đạo hàm theo y

Coi x là hằng số.
$$
\frac{\partial f}{\partial y} = 3x+2y
$$
## Gradient Algorithms

$$a_{n+1}=a_n- \eta\nabla f(a_n)$$
Trong đó:
- $a_{n+1}$: giá trị mới của tham số. 
- $a_n$: giá trị cũ của tham số. 
- $\eta$: tốc độ học. 
- $\nabla f(a_n)$: đạo hàm của hàm số một hoặc nhiều biến. 

# Đạo hàm bậc hai

## Ma trận Hessian

- Loss = độ cao của ngọn đồi.
- Gradient = độ dốc của con dốc.
- Hessian = độ thay đổi của độ dốc.

Cho hai biến: $L(w1​,w2​)$

Hessian là:

$$
H=
\begin{bmatrix}
\frac{\partial^2f}{\partial w_1^2}
&
\frac{\partial^2f}{\partial w_1\partial w_2}
\\
\frac{\partial^2f}{\partial w_2\partial w_1}
&
\frac{\partial^2f}{\partial w_2^2}
\end{bmatrix}
$$

Ý nghĩa từng phần:

$H_{11}$​ cho biết:

> Gradient theo $w_1​$ thay đổi nhanh thế nào khi thay đổi $w_1​$.

---

$H_{22}​$ cho biết:

> Gradient theo w2​ thay đổi nhanh thế nào khi thay đổi w2​.

---

$H_{12}$​ cho biết:

> Thay đổi $w_2​$ sẽ ảnh hưởng thế nào đến gradient theo $w_1$​.

---

$H_{21}​$ cho biết:

> Thay đổi $w_1$​ sẽ ảnh hưởng thế nào đến gradient theo $w_2​$.
