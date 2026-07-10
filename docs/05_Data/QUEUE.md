# Queue

[SOURCE OF TRUTH]
Status: Frozen

Cơ chế hàng đợi dùng khi một module cần xử lý tuần tự các mục việc trong phạm vi một phiên — khác Event (tồn tại tức thời), Queue giữ một danh sách đang chờ xử lý.

## Các Queue đã biết trong hệ thống

- Hàng đợi Comment — Comment Engine giữ danh sách bình luận/tin nhắn đang chờ phân loại/xử lý (đã nêu ở comment_engine/MODULE.md — Runtime State).
- Hàng đợi phản hồi AI Host (Pending response) — nếu nhiều yêu cầu sinh nội dung xếp hàng chờ AI Host xử lý tuần tự.

## Nguyên tắc

1. Queue thuộc Runtime Data (chương 02) — mất khi phiên Ended, trừ khi có nhu cầu ghi log riêng cho Analytics.
2. Một Queue chỉ thuộc sở hữu của đúng một module — module khác không được đọc/ghi trực tiếp vào Queue của module khác, chỉ biết qua Event khi có kết quả xử lý.
3. Khi Queue quá tải (vượt khả năng xử lý), module sở hữu Queue phải cảnh báo Health Monitor (đã nêu ở comment_engine/MODULE.md — Failure Modes: quá tải khi lượng comment tăng đột biến), không được âm thầm rớt dữ liệu mà không ghi nhận.

## Điều file này không định nghĩa

Không định nghĩa cơ chế kỹ thuật cụ thể (FIFO, priority queue, giới hạn kích thước bộ nhớ) — chi tiết triển khai.
