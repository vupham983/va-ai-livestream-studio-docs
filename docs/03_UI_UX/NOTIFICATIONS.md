# Notifications

[SOURCE OF TRUTH]
Status: Frozen

## Vai trò
Hiển thị cảnh báo từ Health Monitor và thông báo hệ thống khác (NON_FUNCTIONAL_SCOPE.md — Observability).

## Nguyên tắc
1. Cảnh báo Critical (chương 07) phải hiển thị nổi bật, không tự động biến mất — người dùng phải chủ động xác nhận đã đọc.
2. Cảnh báo Non-critical có thể tự động biến mất sau một khoảng thời gian, không chặn thao tác khác.
3. Notification không thay thế Dock trạng thái Health Monitor — Notification là tức thời, Dock là trạng thái liên tục.
4. Các cảnh báo cùng nguồn và cùng nguyên nhân phải được gộp hoặc cập nhật trạng thái, không tạo vô hạn notification trùng lặp (chống alert fatigue).
5. Trạng thái Acknowledged và Resolved phải phân biệt rõ: người dùng xác nhận đã đọc (Acknowledged) không có nghĩa sự cố đã được khắc phục (Resolved).

## Điều file này không định nghĩa
Không định nghĩa thời gian hiển thị cụ thể, vị trí pixel.
