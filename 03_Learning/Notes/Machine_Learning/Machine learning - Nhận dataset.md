# Cách mô hình nhận tập dữ liệu
Giả sử y là nhãn và x_i là đặc trưng
1. Nhận một tập dữ liệu như sau

| Đối tượng | $x_1$ | $x_2$ | $x_3$ | y   |
| --------- | ----- | ----- | ----- | --- |
| 1         | 1     | 2     | 2     | 4   |
| 2         | 2     | 3     | 1     | 3   |
2. Đọc lần lượt từ `a[đối tượng_n][x_n]`:
	- Đọc hết dòng cho đến khi gặp ô cuối dòng.	
		- Nếu như gặp ô cuối dòng (tức là ô sau ô hiện tại là ô của dòng dưới) thì ô cuối dòng đó là nhãn.
		- Tất cả các ô còn lại là đặc trưng của từng đối tượng.
