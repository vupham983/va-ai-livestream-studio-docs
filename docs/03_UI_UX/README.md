# 03_UI_UX

[CHỈ THAM CHIẾU]
Status: Frozen

Bản đồ cấu trúc UI/UX ở mức khái niệm — không mô tả pixel/màu cụ thể (đó là design system, làm ở giai đoạn riêng sau Bible này). Mỗi file mô tả cấu trúc và hành vi của một vùng UI, khớp đúng thuật ngữ đã chốt ở GLOSSARY.md (Workspace, Dock) và tôn trọng ranh giới UI Process/Engine Process ở 02_Architecture/chapters/05_runtime_model.md.

## Danh sách
| File | Nội dung |
|---|---|
| LAYOUT.md | Bố cục tổng thể màn hình chính |
| WORKSPACE.md | Không gian làm việc cấu hình |
| DOCK_SYSTEM.md | Cơ chế các vùng giao diện có thể sắp xếp lại |
| TOOLBAR.md | Thanh công cụ |
| INSPECTOR.md | Bảng chỉnh sửa chi tiết một đối tượng |
| THEME.md | Design tokens ở mức khái niệm |
| INTERACTION_PATTERNS.md | Mẫu tương tác lặp lại xuyên suốt UI |
| SHORTCUTS.md | Phím tắt |
| NOTIFICATIONS.md | Thông báo/cảnh báo hiển thị |
| DIALOGS.md | Hộp thoại xác nhận/nhập liệu |

## Nguyên tắc
1. UI chỉ hiển thị bản mirror của State nhận qua IPC (chương 05) — không file nào ở đây được mô tả UI tự giữ logic nghiệp vụ.
2. Mọi hành động Human Override phải có đường dẫn UI tường minh: primary override control luôn hiển thị, hành động chuyên biệt không sâu hơn một thao tác bổ sung.
3. THEME.md là SoT duy nhất cho design tokens khái niệm — file thiết kế chi tiết (Figma...) chỉ được link khi có, không copy nội dung vào đây.

## Điều file này không định nghĩa
Không định nghĩa màu sắc, font, kích thước pixel cụ thể — đó là design system, giai đoạn riêng sau Bible.
