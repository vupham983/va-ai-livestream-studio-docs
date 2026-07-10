# Stress Testing

[SOURCE OF TRUTH]
Status: Frozen

## Phạm vi
Kiểm thử hành vi khi tải vượt mức bình thường — đặc biệt các Failure Mode dạng "quá tải" đã khai ở MODULE.md (VD: comment_engine — quá tải khi lượng comment tăng đột biến).

## Nguyên tắc
1. Test phải xác nhận khi quá tải, hệ thống cảnh báo Health Monitor đúng như Recovery Strategy mô tả — không được âm thầm mất dữ liệu mà không ghi nhận (05_Data/QUEUE.md — nguyên tắc #3).
2. Test phải xác nhận một module quá tải không kéo sập các module khác (Availability, NON_FUNCTIONAL_SCOPE.md).

## Điều file này không định nghĩa
Không định nghĩa ngưỡng tải cụ thể (bao nhiêu comment/giây) — kết quả đo được từ chính quá trình test.
