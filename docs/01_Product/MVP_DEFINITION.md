# MVP Definition

[SOURCE OF TRUTH]
Status: Frozen

Tập con tối thiểu của FUNCTIONAL_SCOPE.md cho bản đầu tiên. Một năng lực không nằm trong danh sách dưới đây nghĩa là bị hoãn tới sau MVP, không phải bị loại bỏ khỏi Product Vision.

## Có trong MVP

1. Workspace — tạo, lưu một Workspace chứa Scene/AI Host/Automation/Media.
2. Scene cơ bản — tạo, lưu, dùng lại giữa các phiên.
3. Hỗ trợ ít nhất một AI Host — sinh nội dung nói cơ bản theo kịch bản định trước + phản ứng đơn giản.
4. Automation — rule if-trigger cơ bản (VD: chào khách mới).
5. Comment Engine — nhận diện và trả lời câu hỏi thường gặp.
6. Voice — chuyển văn bản thành giọng nói.
7. Kết nối một nền tảng livestream đầu tiên (ưu tiên nền tảng phổ biến nhất với người dùng mục tiêu — quyết định cụ thể ghi ở ADR khi bắt đầu 06_Integrations).
8. Human Override — tạm dừng/tiếp quản AI Host và Automation.
9. Health Monitor — cảnh báo cơ bản khi mất kết nối nền tảng.

## Chưa có trong MVP (hoãn sau)

- Nhiều AI Host chạy đồng thời trong một phiên.
- Đa nền tảng cùng lúc (nhiều Platform Adapter chạy song song).
- Analytics chi tiết (chỉ có số liệu cơ bản trong MVP).
- Tự động phục hồi hoàn toàn sau crash (MVP chỉ cảnh báo, chưa tự phục hồi — xem 02_Architecture/chapters/07_failure_recovery.md).
