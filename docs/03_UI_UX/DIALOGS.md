# Dialogs

[SOURCE OF TRUTH]
Status: Frozen

## Vai trò
Hộp thoại xác nhận hành động không thể hoàn tác (destructive configuration action) hoặc yêu cầu nhập liệu quan trọng.

## Nguyên tắc
1. Dialog không bao giờ được dùng cho Emergency Override (INTERACTION_PATTERNS.md).
2. Dialog có thể chặn luồng thao tác đang thực hiện, nhưng không được vô hiệu hoá global Human Override controls hoặc phím tắt khẩn cấp. Khi Live Session đang chạy, các điều khiển an toàn toàn cục luôn có ưu tiên cao hơn modal state của Dialog.

## Điều file này không định nghĩa
Không định nghĩa layout Dialog cụ thể.
