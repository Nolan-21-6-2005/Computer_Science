---
project: EduWatch
status: Planning
tags:
- EduWatch
- Streamlit
- UI
- Dashboard
- Multi-page
- Architecture
title: EduWatch --- UI, Dashboard & Multi-page Architecture
updated: 2026-09-21
---

# EduWatch --- UI, Dashboard & Multi-page Architecture

> \[!abstract\] Mục tiêu Xây dựng giao diện EduWatch theo hướng
> **dashboard giám sát tập trung**, dễ đọc, dễ mở rộng và phù hợp với
> vai trò người dùng.
>
> Hệ thống tập trung vào việc **ghi nhận và phân tích các hành vi có thể
> quan sát được** trong học tập và thi cử, thay vì suy diễn trực tiếp về
> "thái độ" của một cá nhân.

------------------------------------------------------------------------

# 1. Nguyên tắc giao diện

## 1.1. Font chữ

Toàn hệ thống sử dụng font chữ có độ đậm rõ ràng.

### Header

-   Chữ màu đen.
-   Font-weight đậm.
-   Tiêu đề phải dễ nhận biết ở cấp độ đầu tiên.

### Icon

-   Icon sử dụng màu xanh chủ đạo của EduWatch.
-   Icon và text phải có độ tương phản rõ.

### Caption / mô tả

-   Màu xám.
-   Vẫn phải **in đậm**, không sử dụng nét quá mảnh.
-   Kích thước nhỏ hơn tiêu đề nhưng phải đủ lớn để đọc được.

------------------------------------------------------------------------

# 2. Style chung cho Streamlit

## 2.1. Tất cả control

Các control mặc định của Streamlit cần có cùng một ngôn ngữ thiết kế:

-   `st.text_input`
-   `st.button`
-   `st.selectbox`
-   Các control tương tự

### Quy tắc

-   Bo góc sâu ở cả bốn góc.
-   Ưu tiên dạng pill đối với control có chiều cao thấp.
-   Border nhẹ.
-   Shadow nhẹ khi cần.
-   Không sử dụng quá nhiều màu.
-   Màu xanh EduWatch dùng cho trạng thái active / action chính.

------------------------------------------------------------------------

# 3. Những thành phần nên dùng HTML + CSS + JavaScript

Các thành phần có layout hoặc tương tác phức tạp không nên cố ép vào
component mặc định của Streamlit.

Khi cần HTML + JavaScript:

``` text
Streamlit
   │
   └── st.iframe()
           │
           └── HTML + CSS + JavaScript
```

Không sử dụng `st.components.v1.html()` cho các component HTML/JS mới
của giao diện này.

------------------------------------------------------------------------

# 4. Dashboard --- Thống kê báo cáo

## 4.1. Mục tiêu

Trang này trả lời câu hỏi:

> **Tình hình giám sát và mức độ vi phạm trong toàn hệ thống đang như
> thế nào?**

Không nên biến dashboard thành danh sách chi tiết từng vi phạm.

## 4.2. Metric cards

Đề xuất sử dụng khoảng **5--6 metric card chính**.

### Card 1 --- Tổng số vi phạm

Hiển thị: - Tổng số vi phạm trong khoảng thời gian đang chọn. - Mức
tăng/giảm so với kỳ trước.

Không cần tách "tăng" và "giảm" thành hai card riêng.

### Card 2 --- Vi phạm / giờ giám sát

Mục đích: - Chuẩn hóa số vi phạm theo thời gian hệ thống thực sự giám
sát. - Tránh đánh giá một phòng chỉ dựa vào tổng số vi phạm.

``` text
Mật độ vi phạm =
Tổng số vi phạm / Tổng số giờ giám sát
```

### Card 3 --- Tỷ lệ xác nhận đúng

Cho biết trong số các cảnh báo đã được xử lý, bao nhiêu cảnh báo được
người giám sát xác nhận là vi phạm.

``` text
Tỷ lệ xác nhận đúng =
Số cảnh báo được xác nhận đúng
────────────────────────────── × 100
Tổng số cảnh báo đã xử lý
```

### Card 4 --- Tỷ lệ báo sai AI

Cho biết số cảnh báo được người giám sát xác định là không phải vi phạm.

### Card 5 --- Tỷ lệ cảnh báo đã xử lý

Theo dõi khối lượng công việc của người giám sát.

``` text
Tỷ lệ xử lý =
Số cảnh báo đã xử lý
──────────────────── × 100
Tổng số cảnh báo
```

### Card 6 --- Tổng giờ giám sát

Tổng thời gian camera / hệ thống đã thực sự hoạt động trong khoảng thời
gian được chọn.

------------------------------------------------------------------------

# 5. Biểu đồ của trang Thống kê báo cáo

## 5.1. Line chart --- Xu hướng vi phạm

-   X = thời gian.
-   Y = số vi phạm.
-   Có thể chọn ngày, tuần, tháng hoặc khoảng thời gian tùy chọn.

## 5.2. Bar chart --- 8 loại vi phạm phổ biến

So sánh 8 loại vi phạm mà hệ thống đang nhận diện.

Chart chỉ thể hiện **tần suất sự kiện được hệ thống ghi nhận**, không
dùng để kết luận phẩm chất của cá nhân.

------------------------------------------------------------------------

# 6. Bảng lịch sử sử dụng phòng

Cần bổ sung một bảng dữ liệu trong database để theo dõi việc sử dụng
EduWatch tại các phòng.

## Đề xuất bảng

``` text
RoomUsage
──────────────
id
room_id
start_time
end_time
duration
session_type
camera_count
detector_enabled
```

Có thể tính:

``` text
Phòng
Số ca giám sát
Tổng giờ giám sát
Số camera sử dụng
Thời gian detector hoạt động
Tổng số vi phạm
Vi phạm / giờ
```

Mục tiêu là phân biệt **tổng số vi phạm** với **mật độ vi phạm theo thời
gian giám sát**.

------------------------------------------------------------------------

# 7. Trang Giám sát trực tiếp

## 7.1. Camera grid

``` text
┌──────────────────┬──────────────────┐
│    Camera 1      │    Camera 2      │
├──────────────────┼──────────────────┤
│    Camera 3      │    Camera 4      │
└──────────────────┴──────────────────┘
```

Các khung camera: - Bo góc lớn. - Có label camera. - Có trạng thái Live
/ Offline. - Click để phóng to. - Camera được chọn chiếm vùng chính; ba
camera còn lại nằm bên phải. - Click lại để trở về grid 2×2.

------------------------------------------------------------------------

# 8. Thanh điều khiển camera

Thanh điều khiển phải nằm **ngay dưới camera grid**, không để khoảng
trắng lớn.

``` text
┌──────────────────────────────────────────────────────────┐
│  📷 Chụp ảnh    🔴 Ghi hình    │   TRÍ TUỆ NHÂN TẠO     │
│                                │   Đang hoạt động    ◉   │
└──────────────────────────────────────────────────────────┘
```

## Thành phần

### Chụp ảnh

-   Button dạng pill.
-   Nền trung tính.
-   Icon camera.

### Ghi hình

-   Khi chưa ghi: `Ghi hình`.
-   Khi đang ghi: `Dừng ghi`.
-   Dùng màu đỏ/hồng nhạt để biểu thị recording.

### Detector

Không sử dụng toggle mặc định của Streamlit nếu cần giao diện giống
thiết kế mẫu.

``` text
TRÍ TUỆ NHÂN TẠO
Đang hoạt động

┌───────────────────┐
│              ●    │
└───────────────────┘
```

-   ON → xanh lá + nút tròn bên phải.
-   OFF → xám + nút tròn bên trái.

------------------------------------------------------------------------

# 9. Trang Nhật ký vi phạm

## 9.1. Mục tiêu

Trang này trả lời:

> **Hệ thống đã phát hiện những sự kiện nào và người giám sát đã xử lý
> chúng ra sao?**

## 9.2. Metric cards

Đề xuất 5 card:

1.  **Tổng số vi phạm**
2.  **Được xác nhận**
3.  **Báo sai AI**
4.  **Chờ xử lý**
5.  **Tỷ lệ xử lý**

Trong đó:

``` text
Tỷ lệ xử lý =
Đã xử lý / Tổng cảnh báo × 100
```

------------------------------------------------------------------------

# 10. Quản lý thông tin vi phạm

Dùng dạng **listview**.

Mỗi violation item có:

``` text
┌─────────────────────────────────────────────┐
│ [ẢNH]                                       │
│ Sử dụng tài liệu trái phép                  │
│ Camera: Góc cửa chính                       │
│ Phòng: P101                                 │
│ Thời gian: 14:30:05                         │
│ Độ tin cậy: 98.5%                           │
│                                             │
│ [Xác nhận]             [Báo sai AI]         │
└─────────────────────────────────────────────┘
```

Dữ liệu phải lấy từ database, không hard-code.

------------------------------------------------------------------------

# 11. Nhật ký vi phạm realtime ở trang Giám sát

Panel bên phải:

``` text
┌──────────────────────────────┐
│ NHẬT KÝ VI PHẠM MỚI NHẤT  • │
│                              │
│ ┌──────────────────────────┐ │
│ │ Cam: Góc cửa chính       │ │
│ │ 14:30:05                 │ │
│ │ Sử dụng tài liệu         │ │
│ │ [      ẢNH BẰNG CHỨNG ]  │ │
│ │ [ Xác nhận ] [ Báo sai ] │ │
│ └──────────────────────────┘ │
└──────────────────────────────┘
```

Kiến trúc:

``` text
SQLite
   ↓
FastAPI
   ↓
/violations/panel
   ↓
st.iframe()
   ↓
HTML + CSS + JavaScript
```

Panel phải: - sát phía phải; - nằm ngay dưới header; - có scroll
riêng; - không phá layout camera.

------------------------------------------------------------------------

# 12. Trang Quản lý người dùng

Dùng dạng **listview**.

Mỗi user item có: - Avatar. - Mã người dùng. - Tên / vai trò. - Trạng
thái. - Chỉnh sửa. - Khóa / mở khóa.

Có thể thêm: - Tìm kiếm. - Lọc theo vai trò. - Lọc theo trạng thái. -
Thêm người dùng.

------------------------------------------------------------------------

# 13. Multi-page architecture

## 13.1. Mục tiêu mới

Chỉ sử dụng:

``` python
st.navigation()
```

làm router chính.

Hydralit / custom topbar chỉ chịu trách nhiệm giao diện topbar, **không
chịu trách nhiệm routing page**.

------------------------------------------------------------------------

# 14. Phân quyền page theo role

Các page đã được khai báo rõ trong `view/navigation.py`. Không tạo lại
page ở nhiều nơi.

## Role 0 --- Quản trị

``` text
Quản trị
├── Thống kê báo cáo
├── Giám sát trực tiếp
├── Nhật ký vi phạm
├── Danh sách tòa nhà
└── Quản lý người dùng
```

## Role 1 --- Giám sát

``` text
Giám sát
├── Giám sát trực tiếp
├── Nhật ký vi phạm
└── Xuất biên bản
```

## Role 2 --- An ninh

``` text
An ninh
├── Giám sát an ninh
├── Trạng thái thiết bị
└── Báo cáo sự cố
```

------------------------------------------------------------------------

# 15. Cách tổ chức `navigation.py`

`navigation.py` là nơi duy nhất khai báo page.

``` python
SIGN_IN_PAGE = st.Page(...)
SIGN_UP_PAGE = st.Page(...)

ADMIN_PAGES = {
    "Quản trị": [
        st.Page(...),
        st.Page(...),
        st.Page(...),
    ]
}

SUPERVISOR_PAGES = {
    "Giám sát": [
        st.Page(...),
        st.Page(...),
        st.Page(...),
    ]
}

SECURITY_PAGES = {
    "An ninh": [
        st.Page(...),
        st.Page(...),
        st.Page(...),
    ]
}


def get_pages_for_role(role):
    if role == 0:
        return ADMIN_PAGES
    if role == 1:
        return SUPERVISOR_PAGES
    if role == 2:
        return SECURITY_PAGES
    return {}
```

Điểm quan trọng:

> **Không cần viết lại toàn bộ page. Chỉ gom các `st.Page(...)` đã có
> vào nhóm role tương ứng.**

------------------------------------------------------------------------

# 16. `frontend_app.py` mới

Luồng chính:

``` text
frontend_app.py
      │
      ▼
kiểm tra session_state
      │
 ┌────┴─────┐
 │          │
Chưa login  Đã login
 │          │
 ▼          ▼
Auth       lấy role
pages         │
ẩn sidebar    ▼
         get_pages_for_role()
               │
               ▼
         st.navigation()
               │
               ▼
          Sidebar pages
```

### Chưa đăng nhập

``` python
pg = st.navigation(
    [SIGN_IN_PAGE, SIGN_UP_PAGE],
    position="hidden",
)
```

### Đã đăng nhập

``` python
role = st.session_state["role"]

pages = get_pages_for_role(role)

pg = st.navigation(
    pages,
    position="sidebar",
)

pg.run()
```

------------------------------------------------------------------------

# 17. Tránh nhiều hệ thống routing

Không dùng đồng thời:

``` text
session_state page switch
        +
st.navigation
        +
Hydralit routing
```

Kiến trúc cần giữ:

``` text
st.navigation
    = Routing

Sidebar
    = Page navigation

Topbar
    = UI / shortcut / global actions
```

Topbar không trở thành router thứ hai.

------------------------------------------------------------------------

# 18. Database là nguồn dữ liệu chính

Không hard-code: - số vi phạm; - loại vi phạm; - thời gian; - camera; -
phòng; - độ tin cậy; - trạng thái xử lý.

Nguồn dữ liệu:

``` text
SQLite
```

hoặc:

``` text
FastAPI
```

khi component cần realtime / HTML + JavaScript.

------------------------------------------------------------------------

# 19. Phân biệt dữ liệu quan sát và kết luận

## Có thể quan sát

-   Sử dụng tài liệu.
-   Trao đổi.
-   Sử dụng thiết bị.
-   Rời vị trí.
-   Sự kiện camera ghi nhận.
-   Cảnh báo AI.
-   Cảnh báo được xác nhận.
-   Cảnh báo bị báo sai.

## Không nên tự động kết luận

-   "Sinh viên có thái độ xấu."
-   "Sinh viên không chăm học."
-   "Sinh viên thiếu nghiêm túc."
-   "Sinh viên có ý thức kém."

Dashboard nên mô tả **mức độ / tần suất hành vi được hệ thống ghi
nhận**.

------------------------------------------------------------------------

# 20. Thứ tự triển khai

## Phase 1 --- Refactor navigation

-   [ ] Gom toàn bộ `st.Page()` vào `view/navigation.py`.
-   [ ] Phân nhóm theo role.
-   [ ] `frontend_app.py` chỉ gọi `get_pages_for_role()`.
-   [ ] Chưa đăng nhập → Sign in / Sign up.
-   [ ] Đăng nhập → sidebar theo role.
-   [ ] Không tạo routing thứ hai.

## Phase 2 --- Chuẩn hóa UI

-   [ ] Font-weight toàn hệ thống.
-   [ ] Bo góc text input.
-   [ ] Bo góc button.
-   [ ] Bo góc selectbox.
-   [ ] Chuẩn hóa màu.
-   [ ] Chuẩn hóa spacing.

## Phase 3 --- Trang Giám sát

-   [ ] Camera grid 2×2.
-   [ ] Click camera → camera chính.
-   [ ] Ba camera phụ bên phải.
-   [ ] Control bar sát camera.
-   [ ] Custom AI toggle.
-   [ ] Violation panel bằng `st.iframe()`.

## Phase 4 --- Nhật ký vi phạm

-   [ ] 5 metric cards.
-   [ ] Listview.
-   [ ] Ảnh bằng chứng.
-   [ ] Filter.
-   [ ] Xác nhận.
-   [ ] Báo sai AI.

## Phase 5 --- Dashboard

-   [ ] 5--6 metric cards.
-   [ ] Line chart.
-   [ ] Bar chart 8 loại vi phạm.
-   [ ] Room usage table.
-   [ ] Filter thời gian.
-   [ ] Filter tòa nhà / phòng.

## Phase 6 --- Database

-   [ ] Bảng `RoomUsage`.
-   [ ] Foreign key.
-   [ ] Dữ liệu lịch sử sử dụng phòng.
-   [ ] Dữ liệu violation đầy đủ.
-   [ ] Trạng thái xử lý.
-   [ ] Người xác nhận.

------------------------------------------------------------------------

# 21. Kiến trúc tổng thể

``` text
                         EduWatch
                            │
             ┌──────────────┴──────────────┐
             │                             │
          Frontend                       Backend
             │                             │
        Streamlit                        FastAPI
             │                             │
      ┌──────┴──────┐                      │
      │             │                      │
 st.navigation   Topbar                    │
      │        (UI only)                   │
      ▼                                    ▼
 Role-based pages                    Database / API
      │                                    │
      ├── Admin                            │
      ├── Giám sát                         │
      └── An ninh                          │
                                           │
                                      SQLite DB
```

## Nguyên tắc cốt lõi

``` text
st.navigation
    = Routing

Sidebar
    = Page navigation

Topbar
    = UI / shortcut / global actions

Streamlit
    = Application shell

st.iframe()
    = Complex HTML + JS components

FastAPI
    = API / realtime / database access

SQLite
    = Persistent data
```

------------------------------------------------------------------------

# 22. Decision log

## Đã quyết định

-   [x] Sử dụng Streamlit native `st.navigation()` cho multi-page.
-   [x] Page hiển thị theo role.
-   [x] Sidebar là navigation chính.
-   [x] Topbar không thay thế router.
-   [x] HTML + JS phức tạp sử dụng `st.iframe()`.
-   [x] Violation data lấy từ database.
-   [x] Camera layout là 2×2.
-   [x] Control bar nằm ngay dưới camera.
-   [x] Các control Streamlit được bo góc.
-   [x] Metric dashboard tập trung vào dữ liệu quan sát và hoạt động
    giám sát.
-   [x] Không dùng metric để tự động đánh giá phẩm chất hoặc thái độ cá
    nhân.

## Cần triển khai

-   [ ] Refactor `navigation.py`.
-   [ ] Refactor `frontend_app.py`.
-   [ ] Tạo bảng `RoomUsage`.
-   [ ] Hoàn thiện dashboard metric cards.
-   [ ] Hoàn thiện violation listview.
-   [ ] Hoàn thiện custom AI toggle.
-   [ ] Chuẩn hóa CSS.
