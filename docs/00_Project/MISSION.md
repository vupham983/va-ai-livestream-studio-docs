# Mission

[SOURCE OF TRUTH]
Status: Frozen

## Mission 1 — Xây dựng hệ thống sản xuất livestream chuyên nghiệp

Số hoá quy trình vận hành một buổi livestream thành các đơn vị
quản lý được: Scene, Session, Comment, Voice — thay vì coi
livestream là một khối hành động không thể tách rời.

## Mission 2 — Tích hợp AI như một thành phần sản xuất, không phải người cầm lái

AI Host, Automation là công cụ được Live Session điều phối, không
phải thành phần tự quyết định vận hành. Con người luôn giữ quyền
tiếp quản.

## Mission 3 — Đảm bảo độ tin cậy ở mức thương mại thật

Một buổi live chạy nhiều giờ liên tục không được crash, không mất
trạng thái phiên. Mọi trạng thái quan trọng của Live Session (scene
đang chạy, hàng đợi comment, trạng thái automation...) phải được
hệ thống quản lý và có khả năng phục hồi khi lỗi xảy ra giữa chừng.

## Mission 4 — Giữ kiến trúc có thể mở rộng

Thêm một nền tảng livestream mới không được đòi hỏi viết lại lõi
hệ thống — chỉ cần thêm một Platform Adapter mới.

## Mission 5 — Giữ phần mềm dùng được bởi người không biết lập trình

Người vận hành livestream, không phải lập trình viên, là người
dùng chính.

## Mission 6 — Xây một lần, dùng lại mọi nơi

Một Scene, một AI Host, một Automation, một Prompt, một Media Asset
— khi đã tạo, phải tái sử dụng được qua nhiều Session, không bắt
người dùng tạo lại từ đầu mỗi lần live. Nếu không, hệ thống sẽ
thoái hoá thành thao tác copy-paste thủ công, mất đi lý do tồn
tại của chính nó.

## Điều Mission không bao gồm

- Không có mục tiêu trở thành nền tảng cho lập trình viên xây
  ứng dụng khác.
- Không có mục tiêu tối ưu chi phí AI bằng mọi giá nếu đánh đổi
  bằng độ tin cậy của phiên live đang chạy.
