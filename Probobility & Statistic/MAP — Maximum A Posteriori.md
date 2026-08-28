MAP tìm giả thuyết có xác suất hậu nghiệm lớn nhất.

$$ h_{MAP}

\arg\max_h P(h|D) $$

Sử dụng định lý Bayes:

$$ P(h|D)

\frac{P(D|h)P(h)}{P(D)} $$
Vì $P(D)$ giống nhau đối với mọi giả thuyết $h$, khi so sánh các giả thuyết ta có thể bỏ qua $P(D)$:

$$ h_{MAP}

\arg\max_h P(D|h)P(h) $$

### Ý nghĩa

MAP lựa chọn giả thuyết dựa trên **hai yếu tố**:

$$ \boxed{ Likelihood \times Prior } $$

Tức là:

- dữ liệu có phù hợp với giả thuyết không?
- bản thân giả thuyết đó có khả năng xảy ra từ trước không?

---


$$ h_{MLE}

\arg\max_h P(D|h) $$

### So sánh MLE và MAP

MLE:

$$ \boxed{ \arg\max_h P(D|h) } $$

MAP:

$$ \boxed{ \arg\max_h P(D|h)P(h) } $$

Điểm khác biệt:

- **MLE** chỉ quan tâm đến Likelihood.
- **MAP** quan tâm đến Likelihood và Prior.

Có thể hiểu:

$$ MAP = MLE + Prior $$

theo nghĩa MAP đưa thêm thông tin tiên nghiệm vào quá trình lựa chọn giả thuyết.