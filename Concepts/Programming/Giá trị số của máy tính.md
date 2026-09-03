Cho một chương trình:

```c
#include<stdio.h>
int main() {
	int a = 1;
	printf("%p", &a); 
}
```

![[Computer Memory.excalidraw]]
## Địa chỉ của bộ nhớ

```
Địa chỉ vật lý       Nội dung
0x00000000           1 byte
0x00000001           1 byte
0x00000002           1 byte
...
0xBFFFFFFF           1 byte
```
## Mảng

```c
int a[4] = {10, 20, 30, 40};
```

Nếu địa chỉ `a[0]` là 1000 (địa chỉ nằm ở một ô nhớ nào đó trên bộ nhớ chính), thì vì mỗi int chiếm 4 byte:

```c
a[0] → 1000
a[1] → 1004
a[2] → 1008
a[3] → 1012
```
# Số lượng giá trị trong các bit
Tập hợp các bit khi tồn tại n bit nhớ:
- 1 bit nhớ: {1, 0}
- 2 bit nhớ: {00, 01, 10, 11}
- 3 bit nhớ: 
	{000, 001, 010, 011, 
	100, 101, 110, 111}
Số mũ các bit. 
- 000 => 0. (1 trạng thái) 
- 001 => 1. (2 trạng thái) 
- 010 => 2. (3 trạng thái) 
- 100 => 3. (4 trạng thái)
Tính số byte chiếm dụng:
Một ô nhớ chiếm 4 byte. 
- 4 * 8 = 32 (bit). 

Nếu tính theo cách máy tính thường dùng:
Vậy địa chỉ vật lý có thể hình dung từ:

$$3GB=3*1024^3=\text{3 221 225 472}$$

