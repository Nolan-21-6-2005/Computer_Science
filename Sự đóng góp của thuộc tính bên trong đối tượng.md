Ta có: $3+3 = 6$ và $3*2 = 6$

Xét 3 ta thấy $\{1,2\} \in [0, 3]$.

Xét 2 ta thấy ${1} \in [0, 2]$.

Nếu áp dụng [[tích vô hướng]] ta sẽ thấy:
$$3*2 = 3*1 + 3*1 = 1*2 + 1*2 + 1*2 = 6$$
Điều đó có nghĩa là phép nhân ngoài việc là một phương pháp khác để tính tổng. Nó còn cho thấy tổng các thuộc tính của hai đối tượng. 

Đối với lập trình. Nếu một hàm muốn truy cập một thuộc tính bên trong biến được khai báo bởi kiểu dữ liệu cấu trúc. 

```
struct bien {
      thuoc_tinh1;
      thuoc_tinh2;
      thuoc_tinh3;
};


function(bien) {
      bien.thuoc_tinh = get(name);
}

```

Coi: 

- Các thuộc tính chưa được hàm truy cập là $0*1$.
- Các Thuộc tính được truy cập là $1*1$.
- sự kiện hàm truy cập một thuộc tính: 
$$\text{function(bien)}.\text{struct bien} = 1*1 + 0*1 + 0*1 = 1$$.