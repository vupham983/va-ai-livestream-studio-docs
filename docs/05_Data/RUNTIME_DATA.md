# Runtime Data

[SOURCE OF TRUTH]
Status: Approved

Dữ liệu runtime tổng quát — tồn tại trong phạm vi một Live Session đang chạy, thuộc Aggregate Boundary (chương 02, ADR-0007). File này liệt kê từng loại Runtime Data theo module sở hữu; STATE.md tổng hợp toàn bộ Runtime Data này thành một bức tranh runtime duy nhất của phiên (xem STATE.md), QUEUE.md mô tả riêng cơ chế hàng đợi khi cần xử lý tuần tự.

## Các loại Runtime Data theo module

- Scene instance đang active — Scene.
- AI Host: speaking state, conversation context, pending response, active persona instance — AI Host.
- Automation: rule nào đang theo dõi, số lần trigger — Automation.
- Comment Engine: hàng đợi comment đang chờ xử lý (chi tiết ở QUEUE.md).
- Voice: trạng thái đang phát/xử lý audio — Voice.
- Health Monitor: trạng thái Health hiện tại theo từng sub-domain — Health Monitor.
- Platform Adapter: trạng thái kết nối hiện tại — Platform Adapter.
- Analytics: số liệu đang tích luỹ trong phiên — Analytics.
- Live Session: trạng thái state machine hiện tại, danh sách tham chiếu các Runtime Data trên (chương 02).

## Nguyên tắc

1. Mọi Runtime Data bị huỷ hoặc chuyển vào Snapshot khi Live Session chuyển sang Ended — không có Runtime Data nào "sống sót" ngoài phạm vi một phiên trừ khi được lưu vào Snapshot (xem SNAPSHOT.md).
2. Runtime Data không được đọc/ghi trực tiếp bởi module không sở hữu nó — module khác chỉ biết qua sự kiện phát ra Event Bus (chương 04), không truy vấn trực tiếp bộ nhớ của module khác.

## Điều file này không định nghĩa

Không định nghĩa cấu trúc bộ nhớ cụ thể (in-memory object, struct...) — chi tiết triển khai.
