# Inspector

[SOURCE OF TRUTH]
Status: Frozen

## Vai trò
Bảng hiển thị/chỉnh sửa chi tiết một đối tượng đang được chọn.

## Nguyên tắc
1. Inspector chỉnh sửa Configuration khi ở Workspace Mode. Ở Live Operations Mode, Inspector có thể phát hành động yêu cầu (VD: "Activate", "Queue", "Apply") đối với đối tượng runtime, nhưng không được chỉnh sửa trực tiếp Runtime State — mọi thay đổi runtime phải qua luồng Event Bus hợp lệ (ADR-0004, chương 02).
2. Nếu đối tượng đang chọn không còn hợp lệ giữa lúc Inspector đang mở (VD: Media asset bị xoá), Inspector phải hiển thị rõ trạng thái "không còn hợp lệ", không hiển thị dữ liệu cũ như thể vẫn đúng.

## Điều file này không định nghĩa
Không định nghĩa form field cụ thể cho từng loại đối tượng.
