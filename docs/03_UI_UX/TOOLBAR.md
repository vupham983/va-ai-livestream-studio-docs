# Toolbar

[SOURCE OF TRUTH]
Status: Frozen

## Vai trò
Thanh công cụ cố định, chứa hành động thường dùng nhất và lối vào chính cho Human Override.

## Human Override là nhóm hành động, không phải một nút
Human Override gồm nhiều hành động ưu tiên cao (Pause AI Host, Pause Automation, Stop Voice, Cancel pending action, Take Control, chuyển Scene dự phòng, Emergency Stop...). Một primary override control luôn hiển thị; các override chuyên biệt được truy cập trong tối đa một thao tác bổ sung.

## Nhóm hành động
- Điều khiển phiên (Start/Pause/End Live Session) — ánh xạ yêu cầu chuyển trạng thái ở chương 02.
- Primary Human Override (luôn hiển thị khi Live/Paused).
- Chuyển đổi giữa Workspace Mode và Live Operations Mode.

## Nguyên tắc
Primary override control phải luôn ở vị trí dễ thấy nhất, không bị che bởi Dock hay Dialog nào khi đang Live (Philosophy #2, Principles #2).

## Điều file này không định nghĩa
Không định nghĩa icon, vị trí pixel cụ thể.
