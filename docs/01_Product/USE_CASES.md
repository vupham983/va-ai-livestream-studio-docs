# Use Cases

[SOURCE OF TRUTH]
Status: Frozen

Tình huống cụ thể, tham chiếu Journeys ở trên.

## UC1 — Tự động chào khách mới (liên quan Journey 1, bước 3-4)

Khi có người xem mới vào phiên, Automation trigger AI Host phát
lời chào tự động, không cần người vận hành thao tác.

## UC2 — Trả lời câu hỏi lặp lại về sản phẩm (liên quan Journey 1, bước 4)

Comment Engine nhận diện câu hỏi thường gặp (giá, size, cách đặt
hàng), AI Host trả lời tự động; câu hỏi phức tạp được đẩy lên cho
người vận hành xử lý thủ công.

## UC3 — Chuyển Scene khẩn cấp khi mất kết nối một nền tảng (liên
quan Journey 3, bước 3)

Khi Health Monitor phát hiện một Platform Adapter mất kết nối, hệ
thống cảnh báo ngay và cho phép người vận hành chuyển sang Scene
dự phòng hoặc tạm dừng nền tảng đó mà không ảnh hưởng các nền
tảng khác đang live song song.

## UC4 — Tái sử dụng AI Host persona giữa nhiều phiên (liên quan
Journey 2)

Người dùng đã cấu hình một AI Host persona (giọng, phong cách)
cho sản phẩm A, chọn lại đúng persona đó khi live sản phẩm B mà
không cần cấu hình lại.
