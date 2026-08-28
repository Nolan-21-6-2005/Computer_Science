Naïve Bayes là phương pháp **phân lớp dựa trên [[Bayesian|định lý Bayes]]**.

Mục tiêu là xác định lớp $C$ có xác suất lớn nhất khi quan sát một mẫu dữ liệu $X$.
$$C_{MAP}

\arg\max_C P(C|X) $$

Theo định lý Bayes:
$$ P(C|X)

\frac{P(X|C)P(C)}{P(X)} $$
Vì $P(X)$ giống nhau đối với các lớp khi so sánh, ta có:

$$ C_{MAP}

\arg\max_C P(X|C)P(C) $$
# Giả định "Naïve"

Giả sử mẫu $X$ có nhiều thuộc tính:

$$ X=(x_1,x_2,\ldots,x_n) $$

Naïve Bayes giả định các thuộc tính $x_i$ **độc lập có điều kiện khi biết lớp $C$**.

được phân rã thành:

$$ P(X|C)

\prod_{i=1}^{n}P(x_i|C) $$

Suy ra:
$$ \boxed{ C_{MAP}

\arg\max_C P(C) \prod_{i=1}^{n}P(x_i|C) } $$

Đây là công thức quan trọng nhất cần nhớ của Naïve Bayes.

---
# Quy trình phân lớp bằng Naïve Bayes

Với một mẫu mới:

$$ X=(x_1,x_2,\ldots,x_n) $$

Ta thực hiện:

1. Xét từng lớp $C$ có thể có.
2. Tính xác suất tiên nghiệm:

$$ P(C) $$

3. Tính xác suất của từng thuộc tính khi biết lớp:

$$ P(x_i|C) $$

4. Nhân các xác suất:

$$ P(C) \prod_{i=1}^{n}P(x_i|C) $$

5. Chọn lớp có giá trị lớn nhất:

$$ C^*

\arg\max_C P(C) \prod_{i=1}^{n}P(x_i|C) $$

