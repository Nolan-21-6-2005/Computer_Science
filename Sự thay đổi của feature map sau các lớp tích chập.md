# Mỗi lần đi qua một phép tích chập, số kênh và feature map thay đổi như thế nào ?

## Ảnh đầu vào

Giả sử ảnh đầu vào:

$$Input = 224 \times 224 \times 3$$

Gồm ba ma trận:

- R (Red)
- G (Green)
- B (Blue)
# Filter

Mỗi Filter có kích thước:
$$3 \times 3 \times 3$$Tức là mỗi Filter có:

- Một Kernel $3\times 3$ xử lý kênh R.
- Một Kernel $3 \times 3$ xử lý kênh G.
- Một Kernel $3 \times 3$ xử lý kênh B.

![[yibNN2CoGF9zPvHlIvWq56S6t9psBfP_qRBQCxoQC7of6eSFyJVKH_VvFlW8kK65UZZHa_n9bQltioHC6ZcRuJsWpZcX0qqr_eNy74IEQSHHwda0ABeNsMBGIs1f6cClN6z0tGKerfhHFIfUT7ufl6BTPIMDwt74Wq0dRapGAHlbcmex_5DfJXF3TZ3jTWZK.jpg]]

Với Filter thứ nhất:

$$Output_1 = (R * W_R)+(G * W_G)+(B * W_B)$$

Kết quả là một Feature Map.