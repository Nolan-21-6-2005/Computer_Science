## Xác định dung lượng bộ nhớ khả dụng
Để ước lượng dung lượng RAM cần thiết cho một thuật toán, ta sẽ xét trường hợp tệ nhất của vấn đề.
Đặt vấn đề:
- Một phương án gồm 10 số nguyên.
- Mỗi số nguyên chiếm 4 byte.
- Vậy một phương án chiếm:
$$10 * 4 = 40 \ \text{byte}$$
Nếu thuật toán lưu đồng thời N phương án thì:
$$RAM=N*4$$
Trong đó số phương án bằng cách sử dụng lý thuyết tổ hợp (tổ hợp, chỉnh hợp,... ).

Nếu có:
- 1.000 phương án → khoảng 40 KB.
- 1.000.000 phương án → khoảng 40 MB.
- 1.000.000.000 phương án → khoảng 40 GB.

