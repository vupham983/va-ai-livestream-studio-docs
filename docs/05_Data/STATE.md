# State

[SOURCE OF TRUTH]
Status: Frozen

Trạng thái hiện tại của một Live Session đang chạy — sống trong Engine Process, được UI Process nhận qua IPC dưới dạng bản phản chiếu (mirror), không giữ bản sao độc lập (chương 05 — Runtime Model).

## Thành phần chính của State

- Trạng thái state machine của Live Session (Idle/Preparing/Live/Paused/Error/Ended) — chương 02.
- Tổng hợp Runtime Data hiện tại của toàn bộ module đang hoạt động trong phiên (xem RUNTIME_DATA.md).

## Nguyên tắc đồng bộ UI–Engine

State là nguồn sự thật duy nhất, luôn sống ở Engine Process. UI Process hiển thị đúng bản mirror nhận qua IPC — nếu mất kết nối IPC tạm thời, UI hiển thị State cũ nhất đã biết kèm nhãn "có thể không mới nhất", không tự suy đoán trạng thái mới (nhất quán với health_monitor/MODULE.md — Recovery Strategy khi thiếu dữ liệu giám sát).

## Quan hệ với Cache

State không phải Cache — State là nguồn sự thật (Source of Truth của runtime), trong khi Cache (xem CACHE.md) chỉ là bản lưu tạm để tối ưu, luôn có thể xây dựng lại từ State.

## Điều file này không định nghĩa

Không định nghĩa giao thức IPC cụ thể (đã nêu ở chương 05 là chưa chốt, chờ ADR triển khai).
