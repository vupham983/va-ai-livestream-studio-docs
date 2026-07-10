# Principles

[SOURCE OF TRUTH]
Status: Frozen

Nguyên tắc dùng để phân xử khi hai lựa chọn thiết kế mâu thuẫn
nhau. Khi PRINCIPLES.md không đủ để phân xử, xem ARCHITECTURE_GOVERNANCE.md
mục 6 (Approver quyết định, ghi thành ADR).

## 1. Ổn định thắng tính năng mới

Không hy sinh sự ổn định của Live Session đang chạy để đổi lấy
số lượng tính năng.

## 2. Con người thắng tự động hoá

Khi Automation và Human Override xung đột (VD: người dùng can
thiệp giữa lúc automation đang chạy), quyền của con người luôn
thắng ngay lập tức, không cần xác nhận thêm.

## 3. Đơn giản thắng linh hoạt, trừ khi có bằng chứng cần linh hoạt

Không thiết kế module cho một khả năng "có thể cần trong tương lai"
nếu chưa có use case cụ thể — tránh over-engineering, giữ đúng
tinh thần MVP_DEFINITION.md.

## 4. Reuse thắng Duplicate

Khi một logic đã tồn tại ở module khác, không viết lại — tham
chiếu hoặc trích xuất thành thành phần dùng chung. (Nguyên tắc
này áp dụng cả cho code lẫn chính tài liệu docs/.)

## 5. Rõ ràng thắng ngắn gọn trong tài liệu kiến trúc

Một ADR hay MODULE.md dài nhưng rõ ràng luôn tốt hơn một bản ngắn
nhưng để lại vùng xám cho người đọc tự suy đoán.

## 6. Không có quyết định kiến trúc "tạm thời"

Nếu một quyết định được ghi vào ADR, nó có hiệu lực cho tới khi
bị Superseded bởi ADR khác — không có khái niệm "làm tạm rồi sửa
sau" mà không ghi nhận lại.
