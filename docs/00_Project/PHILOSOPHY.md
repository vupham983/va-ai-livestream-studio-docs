# Philosophy

[SOURCE OF TRUTH]
Status: Frozen

## 1. Live Session là trung tâm, không phải AI

Mọi module tồn tại để phục vụ một phiên livestream đang diễn ra.
Khi một quyết định thiết kế phải chọn giữa "AI làm được nhiều hơn"
và "Live Session chạy ổn định hơn", Live Session thắng.

## 2. Con người luôn giữ quyền tiếp quản (Human Override)

AI vận hành trong giới hạn do con người thiết lập. Ở bất kỳ thời
điểm nào trong một Live Session, con người phải có khả năng can
thiệp trực tiếp — dừng AI Host, sửa lại một hành động Automation,
tắt một Scene — mà không cần dừng toàn bộ phiên live.

## 3. Build Once, Reuse Everywhere

Một đơn vị đã tạo (Scene, AI Host persona, Automation rule, Prompt,
Media asset) phải tái sử dụng được qua nhiều Session khác nhau.
Hệ thống không được buộc người dùng tạo lại từ đầu mỗi lần chuẩn
bị một buổi live.

## 4. Ổn định quan trọng hơn tính năng

Một tính năng mới không được đánh đổi bằng rủi ro sập phiên live
đang chạy. Khi phải chọn, chọn ổn định.

## 5. Kiến trúc phải sống được 10 năm, không phải 10 tháng

Mọi quyết định kỹ thuật được đánh giá bằng câu hỏi: "quyết định
này còn hợp lý khi hệ thống có 50 module, không phải 10 module,
hay không?" — không tối ưu cho tốc độ ra bản đầu tiên bằng mọi giá.

## 6. Tài liệu là một phần của kiến trúc, không phải phụ lục

Một quyết định không được ghi lại (ADR) coi như chưa từng được
quyết định — sẽ bị đặt lại câu hỏi từ đầu bởi người tiếp theo,
kể cả nếu người đó là chính tác giả quyết định ban đầu.
