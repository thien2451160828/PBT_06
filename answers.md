### Câu A1 (10đ) — Grid System

**Bảng Layout Grid System:**

| Kích thước | < 768px | 768px - 991px | ≥ 992px |
| :--- | :--- | :--- | :--- |
| **Số cột** | 1 | 2 | 4 |
| **Box layout** | Xếp chồng dọc (4 hàng, mỗi hàng 1 box) | Lưới 2x2 (2 hàng, mỗi hàng 2 box) | Nằm ngang (1 hàng 4 box) |

**Trả lời câu hỏi thêm:**
* **`col-md-6` nghĩa là gì?** Khi màn hình đạt kích thước từ trung bình trở lên (medium `md` - từ `768px`), phần tử này sẽ chiếm 6 phần trên tổng số 12 phần của hệ thống lưới (chiếm 50% chiều rộng container).
* **Tại sao không cần viết `col-sm-12`?** Bootstrap sử dụng cơ chế **Mobile-First**. Class `col-12` được khai báo ban đầu sẽ áp dụng cho mọi màn hình từ nhỏ nhất (xs) và tự động kéo dài tác dụng lên các màn hình lớn hơn (như sm). Nó chỉ dừng lại khi gặp một breakpoint khác đè lên (ở đây là `col-md-6`). Việc viết thêm `col-sm-12` là thừa vì `col-12` đã lo liệu việc đó rồi.

### Câu A2 (10đ) — Utilities & Components

**1. Giải thích class `d-none d-md-block`:**
* **Hiển thị khi nào:** Hiển thị ở dạng khối (`display: block`) trên các màn hình từ Tablet trở lên (kích thước `md` ≥ 768px).
* **Ẩn khi nào:** Ẩn hoàn toàn (`display: none`) trên các màn hình nhỏ như Mobile (kích thước < 768px).
* *Cơ chế:* Bootstap dùng Mobile-First, nên `d-none` sẽ có tác dụng từ màn hình nhỏ nhất, sau đó lên đến điểm neo `md` thì bị `d-md-block` ghi đè.

**2. Liệt kê và giải thích 5 spacing utilities (margin/padding):**
* `mt-3`: `margin-top` mức 3 (thường là `1rem` - khoảng 16px).
* `px-4`: `padding-left` và `padding-right` mức 4 (thường là `1.5rem` - khoảng 24px).
* `mb-auto`: `margin-bottom` được thiết lập là `auto` (thường dùng trong Flexbox để đẩy các phần tử khác lên trên).
* `py-2`: `padding-top` và `padding-bottom` mức 2 (thường là `0.5rem` - khoảng 8px).
* `mx-auto`: `margin-left` và `margin-right` là `auto` (kỹ thuật kinh điển để căn giữa một khối (block) theo chiều ngang).

**3. Sự khác nhau giữa `.container`, `.container-fluid`, và `.container-md`:**
* `.container`: Cố định chiều rộng tối đa (max-width) tại mỗi điểm neo màn hình (sm, md, lg...). Khi màn hình to ra, nội dung ở giữa và để lại 2 khoảng lề trống hai bên.
* `.container-fluid`: Luôn luôn chiếm 100% chiều rộng màn hình ở mọi kích thước, không có lề dư 2 bên.
* `.container-md`: Sẽ chiếm 100% chiều rộng ở màn hình nhỏ (giống fluid), nhưng khi đạt đến mốc `md` (≥ 768px) trở lên thì nó mới bắt đầu hoạt động giống như `.container` bình thường (có max-width và lề 2 bên).

### Câu C1 (10đ) — Tùy biến Bootstrap

**1. Quy trình đổi màu `$primary` từ xanh mặc định sang `#E63946`:**
Để đổi màu gốc của hệ thống, chúng ta không can thiệp vào file CSS có sẵn mà phải làm việc với mã nguồn SASS của Bootstrap.
* **Công cụ cần thiết:** * NodeJS / npm (để tải mã nguồn Bootstrap gốc về dự án).
    * Trình biên dịch SASS (ví dụ: extension *Live Sass Compiler* trong VS Code hoặc công cụ dòng lệnh Dart Sass).
* **Quy trình thực hiện (Modify file nào?):**
    1.  Tạo một file SCSS hoàn toàn mới trong dự án của bạn (ví dụ: `custom.scss`).
    2.  Tiến hành khai báo đè giá trị biến lên **trước** khi nhúng Bootstrap:
        ```scss
       
        $primary: #E63946;
        
       
        @import "../node_modules/bootstrap/scss/bootstrap";
        ```
    3.  Sử dụng trình biên dịch SASS để dịch file `custom.scss` này thành một file CSS thông thường (ví dụ `custom.css`).
    4.  Vào file HTML, xóa link CDN mặc định đi và thay bằng đường dẫn tới file `custom.css` vừa được tạo ra.

**2. Tại sao KHÔNG nên override trực tiếp `.btn-primary { background: red; }` mà nên dùng SASS variables?**
* **Tính đồng bộ toàn hệ thống (Consistency):** Trong Bootstrap, biến `$primary` không chỉ tạo ra màu cho mỗi cái nút. Nó là gen gốc để tạo ra hàng chục thành phần khác như chữ (`.text-primary`), nền (`.bg-primary`), viền (`.border-primary`), huy hiệu (`.badge`), v.v. Nếu bạn dùng code CSS thuần đè lên `.btn-primary`, các thành phần khác sẽ vẫn giữ màu xanh cũ (rất thiếu đồng bộ). Khi dùng biến SASS, bạn chỉ cần sửa 1 dòng, toàn bộ trang web sẽ tự động chuyển sang màu đỏ `#E63946`.
* **Mất trạng thái tương tác tự động (Hover / Focus):** Mã nguồn SASS của Bootstrap sử dụng các hàm tính toán màu tự động (như tự động làm tối màu đi 10% khi hover chuột, hoặc làm sáng màu lên ở viền lúc focus). Nếu bạn đè CSS trực tiếp `background: red;`, cái nút của bạn sẽ trở nên "bất động", di chuột vào không có hiệu ứng gì cả, bắt buộc bạn phải viết thêm code cho cả trạng thái `:hover` và `:active` rất mất công.

### Câu C2 (10đ) — So sánh CSS thuần và Bootstrap

Dựa trên kinh nghiệm xây dựng Navbar responsive và Product Card bằng CSS thuần (ở PBT trước) so với việc sử dụng Bootstrap ở bài này, dưới đây là bảng phân tích chi tiết:

| Tiêu chí | CSS Thuần (Custom CSS) | Bootstrap 5 |
| :--- | :--- | :--- |
| **Số dòng CSS cần viết** | Rất nhiều (có thể lên tới 100 - 200 dòng chỉ để căn chỉnh flexbox, padding, hover và viết Media Queries cho Responsive). | **0 dòng CSS**. Hoàn toàn không cần viết CSS, chỉ cần ghép các class có sẵn (`navbar`, `card`, `d-md-flex`) trực tiếp vào thẻ HTML. |
| **Thời gian phát triển** | Lâu. Phải tự tính toán kích thước, debug các lỗi hiển thị vỡ khung trên thiết bị di động. | **Cực kỳ nhanh**. Chỉ mất vài phút để ráp xong một layout chuẩn chỉnh vì các Component đã được thiết kế sẵn. |
| **Khả năng tùy biến** | **Tuyệt đối (100%)**. Có thể tự do sáng tạo mọi hình dáng, bóng đổ, hiệu ứng animation độc lạ không đụng hàng. | **Bị hạn chế**. Nếu chỉ dùng class mặc định, trang web trông sẽ rất "đại trà" (Bootstrap-look). Để tùy biến sâu đòi hỏi phải biết can thiệp vào SCSS phức tạp. |

**Kết luận thực tiễn:**

**1. Khi nào NÊN dùng Bootstrap?**
* Khi cần chạy đua với thời gian (Rapid Prototyping) để ra mắt sản phẩm nhanh nhất.
* Khi làm các hệ thống quản trị nội bộ (Admin Dashboard), trang thống kê (như Bài B2 vừa làm), nơi chú trọng tính năng, sự gọn gàng hơn là thiết kế màu mè.
* Khi đội ngũ phát triển mạnh về Backend nhưng yếu về thiết kế giao diện (UI/UX).

**2. Khi nào KHÔNG NÊN dùng Bootstrap?**
* Khi dự án yêu cầu một giao diện mang tính nhận diện thương hiệu cực kỳ cao, độc bản và sáng tạo (ví dụ: landing page cho một chiến dịch marketing lớn, web nghệ thuật, portfolio cá nhân).
* Khi dự án cần tối ưu hiệu năng tuyệt đối (việc tải toàn bộ thư viện Bootstrap cồng kềnh cho một trang web quá đơn giản là dư thừa và làm chậm tốc độ tải trang).