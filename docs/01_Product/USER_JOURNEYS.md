# User Journeys

[SOURCE OF TRUTH]
Status: Frozen

## Journey 1 — Chuẩn bị và chạy một phiên live đầu tiên (Persona 1)

1. Tạo Scene (bố cục hiển thị, overlay sản phẩm).
2. Thiết lập AI Host (giọng nói, phong cách dẫn).
3. Thiết lập Automation cơ bản (VD: tự động chào khi có người mới
   vào xem).
4. Bắt đầu Live Session — theo dõi Comment Engine xử lý bình luận.
5. Can thiệp thủ công (Human Override) khi có tình huống ngoài kịch bản.
6. Kết thúc phiên — xem lại Analytics.

## Journey 2 — Tái sử dụng cấu hình cho phiên live thứ N (Persona 2)

1. Mở phiên mới, chọn Scene/AI Host/Automation đã tạo trước đó.
2. Chỉnh sửa nhỏ nếu cần (VD: đổi sản phẩm đang giới thiệu).
3. Chạy phiên — không phải dựng lại từ đầu.

## Journey 3 — Giám sát và xử lý sự cố giữa phiên (Persona 3)

1. Theo dõi Health Monitor trong lúc phiên đang chạy.
2. Nhận cảnh báo khi có nguy cơ (mất kết nối nền tảng, tài nguyên
   máy quá tải).
3. Thực hiện Human Override — tạm dừng AI Host hoặc chuyển Scene
   dự phòng.
4. Phiên tiếp tục chạy sau khi xử lý xong.
