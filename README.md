# E-Museum Ticket — Website Đặt Vé Bảo Tàng Trực Tuyến

Bản refactor của source code gốc. **Toàn bộ chức năng được giữ nguyên** —
đây là một hệ thống nhiều trang (đăng ký/đăng nhập, tìm địa điểm, đặt vé,
thanh toán, quản lý vé, hồ sơ cá nhân), không phải một trang đơn giản.

## Cấu trúc thư mục

```
/
├── index.html              Trang chủ
├── locations.html          Danh sách địa điểm / bảo tàng
├── museum-detail.html      Chi tiết một bảo tàng
├── booking.html            Đặt vé tham quan
├── payment.html            Thanh toán
├── ticket.html             Xác nhận vé sau khi đặt
├── ticket-detail.html      Chi tiết một vé (mã QR)
├── my-tickets.html         Danh sách vé của người dùng
├── login.html / register.html   Đăng nhập / Đăng ký
├── profile.html            Hồ sơ cá nhân
│
├── css/
│   ├── variables.css       Design tokens (màu, font, bo góc, đổ bóng...) — load đầu tiên
│   ├── style.css           Reset + typography + layout dùng chung
│   ├── components.css      Nút, thẻ, badge, modal, form... dùng chung
│   ├── responsive.css      Media query dùng chung (390 / 768 / 1200px)
│   └── <page>.css          CSS riêng cho từng trang (được tách ra từ
│                           khối <style> nội tuyến trong mỗi file .html)
│
├── js/
│   ├── main.js             Navbar, menu di động, trạng thái đăng nhập — dùng chung
│   ├── storage.js          Lớp trung gian thao tác localStorage
│   ├── api.js              Mock API: đọc/ghi dữ liệu từ data/db.json
│   ├── auth.js             Đăng ký / đăng nhập / đăng xuất
│   ├── utils.js             Định dạng tiền tệ, ngày giờ, tạo ID...
│   └── <page>.js            Logic riêng cho từng trang tương ứng
│
├── data/db.json             "Cơ sở dữ liệu" giả lập (museum, tour, user, ticket)
├── static/                  Ảnh bảo tàng
└── vercel.json               Cấu hình deploy Vercel
```

## Những gì đã refactor trong lần này

1. **Tách CSS nội tuyến ra khỏi HTML** — 9/9 trang đều có khối `<style>`
   nhúng thẳng trong `<head>` (267 dòng ở `index.html`, có trang tới 310
   dòng). Toàn bộ đã được tách sang `css/<tên-trang>.css` và link bằng
   `<link rel="stylesheet">`. Giao diện không đổi.
2. **Sửa lỗi tải CSS trùng** — `style.css` dùng `@import url('variables.css')`
   trong khi mọi trang đã `<link>` trực tiếp `variables.css` trước đó ⇒
   biến CSS bị tải và parse 2 lần trên **mọi** trang. Đã bỏ `@import` trùng.
3. **Thêm comment mô tả đầu file** cho toàn bộ 14 file JS và 4 file CSS gốc
   (trước đó không có file nào có comment mô tả mục đích).
4. Semantic HTML (`header/nav/main/section/article/aside/footer`) và thuộc
   tính ảnh (`alt`, `loading="lazy"`) — kiểm tra và xác nhận **đã đạt chuẩn**
   từ source gốc, không cần sửa.

## Cách chạy local

Vì các trang dùng `fetch()` để đọc `data/db.json`, cần chạy qua một web
server (không mở trực tiếp file `.html` bằng `file://`):

```bash
# Cách 1: dùng Python
python3 -m http.server 8080

# Cách 2: dùng Node (npx serve)
npx serve .
```

Sau đó mở `http://localhost:8080`.

## Việc còn có thể làm tiếp (chưa nằm trong lần refactor này)

Do quy mô dự án lớn (~8.700 dòng HTML/CSS/JS trên 23 file), lần refactor
này tập trung vào cấu trúc tổng thể. Nếu cần đi sâu hơn theo từng file
(đổi tên biến, chia nhỏ hàm dài, thêm `width`/`height` cho từng ảnh cụ thể,
audit Lighthouse chi tiết...), có thể tiếp tục theo từng file JS/CSS cụ thể.
