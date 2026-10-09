# [Thực hành] Tạo Menu Dọc Trong CSS (Vertical Dropdown Menu)

## 📌 Giới Thiệu & Mục Tiêu

Bài thực hành hướng dẫn kỹ thuật xây dựng thanh điều hướng dọc (**Vertical Flyout/Dropdown Menu**) đa cấp thuần **CSS** (không cần JavaScript), vận dụng linh hoạt kỹ thuật định vị phần tử (**CSS Positioning**), mô hình hộp (**Box Model**) và bộ chọn trạng thái (**`:hover`**).

### 🎯 Mục tiêu bài học:
- **Làm chủ kỹ thuật định vị (CSS Positioning)**: Sử dụng cặp thuộc tính `position: relative` (neo tọa độ ở thẻ cha) và `position: absolute` (đặt tọa độ chính xác cho menu con).
- **Bộ chọn CSS lồng nhau (Nested Selectors)**: Thấu hiểu cách viết bộ chọn chuỗi `#menu ul li:hover ul` để kích hoạt menu con tương ứng.
- **Ẩn/Hiện phần tử (Visibility Control)**: Sử dụng `visibility: hidden` và `visibility: visible` để quản lý trạng thái hiển thị.
- **Tối ưu trải nghiệm người dùng (UX)**: Đảm bảo vùng bấm liên kết (`display: block`), chuyển đổi con trỏ chuột (`cursor: pointer`), và xử lý không bị tràn dòng chữ dài.

---

## 📂 Cấu Trúc Dự Án

```text
thuc_hanh_tao_menu_doc/
├── index.html           # Bản chuẩn theo đúng cấu trúc đề bài CodeGym
├── styles.css           # CSS chuẩn CodeGym (kèm tối ưu chống vỡ dòng chữ dài)
├── modern_menu.html     # Phiên bản nâng cấp giao diện UI/UX cao cấp
├── modern_styles.css    # CSS giao diện Dark/Indigo hiện đại & hiệu ứng chuyển cảnh
└── README.md            # Tài liệu phân tích kỹ thuật chi tiết
```

---

## 📐 Phân Tích Kỹ Thuật: Cơ Chế Tọa Độ & Box-Model

### 1. Tại sao menu con lại có tọa độ `left: 140px`?

Trong tệp `styles.css`:
```css
#menu li {
    position: relative;
    width: 100px;
    height: 30px;
    padding: 0 20px;
    background-color: black;
}

#menu ul ul {
    position: absolute;
    top: 0;
    left: 140px;
    visibility: hidden;
}
```

> [!NOTE]
> **Công thức tính điểm tiếp giáp:**
> - Thẻ cha `li` có bề rộng nội dung: $\text{Width} = 100\text{px}$.
> - Đệm trong: $\text{Padding-left} = 20\text{px}$, $\text{Padding-right} = 20\text{px}$.
> - **Tổng chiều rộng chiếm dụng của thẻ cha:**
>   $$\text{Total Width} = 100\text{px} + 20\text{px} + 20\text{px} = 140\text{px}$$
> 
> Khi menu con `ul` được gán `left: 140px`, cạnh trái của menu con sẽ nằm khít ngay sát mép ngoài bên phải của mục cha, tạo cảm giác menu con "bay ra" (flyout) liền mạch ngay bên cạnh.

---

### 2. Sơ đồ cây DOM và cơ chế kích hoạt Hover

```text
#menu (div)
 └── ul (Cấp 1: Danh sách chính)
      ├── li (Sem 1) [position: relative]
      │    ├── a ("Sem 1")
      │    └── ul (Cấp 2) [position: absolute; top: 0; left: 140px; visibility: hidden]
      │         ├── li ("Computing Fundamental")
      │         ├── li ("C Programming")
      │         ├── li ("Building Dynamic Website")
      │         ├── li ("MS Transact-SQL")
      │         └── li ("Project")
      ├── li (Sem 2) [position: relative]
      │    └── ul (Cấp 2) [...]
      ├── li (Sem 3)
      └── li (Sem 4)
```

Khi người dùng rê chuột vào `li`:
```css
/* Khi li của danh sách ngoài được hover, ul con nằm bên trong nó sẽ xuất hiện */
#menu ul li:hover ul {
    visibility: visible;
}
```

---

## 📊 Bảng Thuộc Tính CSS Cốt Lõi

| Bộ chọn | Thuộc tính CSS | Ý nghĩa & Tác dụng kỹ thuật |
| :--- | :--- | :--- |
| `#menu, #menu ul, #menu li` | `padding: 0; margin: 0;` | Reset các khoảng cách mặc định của trình duyệt |
| `#menu li` | `list-style: none;` | Bỏ các dấu chấm tròn (bullet markers) mặc định của thẻ `li` |
| `#menu li` | `position: relative;` | Thiết lập điểm mốc tọa độ gốc `(0, 0)` cho các phần tử con tuyệt đối |
| `#menu li` | `width: 100px; height: 30px;` | Cố định kích thước khung chữ nhật của từng mục menu |
| `#menu li` | `line-height: 30px;` | Căn giữa dòng văn bản theo phương thẳng đứng |
| `#menu li` | `cursor: pointer;` | Chuyển con trỏ chuột sang hình bàn tay báo hiệu tương tác |
| `#menu li a` | `text-decoration: none;` | Bỏ gạch chân mặc định của liên kết |
| `#menu li:hover` | `background-color: red;` | Phản hồi thị giác tức thì khi di chuột qua |
| `#menu ul ul` | `position: absolute;` | Tách menu con khỏi luồng tài liệu thông thường |
| `#menu ul ul` | `top: 0; left: 140px;` | Đặt menu con thẳng hàng với đỉnh mục cha và nằm ngay sát mép phải |
| `#menu ul ul` | `visibility: hidden;` | Ẩn hoàn toàn menu con khi chưa có tương tác chuột |
| `#menu ul li:hover ul` | `visibility: visible;` | Kích hoạt hiển thị menu con khi mục cha được hover |

---

## ✨ Điểm Nâng Cấp Ở Bản Hiện Đại (`modern_menu.html`)

Ngoài bản chuẩn 100% của CodeGym, dự án trang bị thêm phiên bản hiện đại:
1. **Thẩm mỹ Dark Mode & Glassmorphism**: Nền gradient tối sang trọng, bo góc `12px`, viền kính mờ `rgba(255, 255, 255, 0.08)`.
2. **Hiệu ứng chuyển động (Transitions)**: Menu con trượt nhẹ ra với `opacity`, `transform: translateX()`, tạo cảm giác mượt mà thay vì bật giật cục.
3. **Mũi tên chỉ thị (`›`)**: Tự động hiển thị và xoay nhẹ báo hiệu cho người dùng biết mục nào có chứa thực đơn con.
4. **Typography cao cấp**: Sử dụng font chữ hiện đại Google Fonts *Plus Jakarta Sans*.

---

## 🚀 Hướng Dẫn Trải Nghiệm & Kiểm Thử

Bạn có thể mở trực tiếp các tệp HTML bằng trình duyệt web:
1. **Bản chuẩn CodeGym**: Mở tệp [**`index.html`**](file:///C:/Users/dathao/Downloads/thuc_hanh_tao_menu_doc/index.html).
2. **Bản nâng cấp UI/UX**: Mở tệp [**`modern_menu.html`**](file:///C:/Users/dathao/Downloads/thuc_hanh_tao_menu_doc/modern_menu.html).
