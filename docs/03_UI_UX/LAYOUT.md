# Layout

[SOURCE OF TRUTH]
Status: Frozen

## Cấu trúc vận hành chính
UI hỗ trợ hai chế độ vận hành chính, không phải ba vùng cố định tách rời:
- Workspace Mode — ưu tiên chuẩn bị và chỉnh sửa Configuration.
- Live Operations Mode — ưu tiên preview, monitoring, queue, và Human Override.

Trong trạng thái Preparing/Live, UI chuyển trọng tâm từ Workspace Mode sang Live Operations Mode nhưng không đóng hoặc loại bỏ hoàn toàn công cụ chuẩn bị — hai chế độ có thể cùng xuất hiện dưới dạng dock/panel, không nhất thiết là hai màn hình tách biệt.

## Nguyên tắc
1. Layout phải phản ánh rõ trạng thái Live Session hiện tại. Khi phiên đang Live, người dùng vẫn có thể chuẩn bị hoặc chỉnh sửa Configuration không tác động trực tiếp tới runtime đang active (khớp chương 02 — Configuration độc lập với Runtime). Thay đổi ảnh hưởng tới đối tượng đang chạy phải được áp dụng qua hành động tường minh, theo luồng Event Bus hợp lệ (ADR-0004); UI không được sửa trực tiếp Runtime State.
2. Toolbar và khu vực Human Override phải luôn hiển thị cố định, không bị ẩn trong bất kỳ chế độ nào (Philosophy #2).

## Điều file này không định nghĩa
Không định nghĩa kích thước, tỷ lệ khung hình cụ thể của từng chế độ.
