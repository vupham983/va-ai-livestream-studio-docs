# Workspace (UI)

[SOURCE OF TRUTH]
Status: Frozen

## Vai trò
Nơi tạo và chỉnh sửa Configuration (Scene, AI Host persona, Automation rule, Media) — tương ứng vùng Configuration ở 02_Architecture/chapters/00_overview.md. Có thể tiếp tục sử dụng trong lúc Live đối với các thay đổi không tác động trực tiếp tới runtime đang active (LAYOUT.md).

## Cấu trúc
- Danh sách Scene đã tạo, có thể chọn/sửa/xoá.
- Khu vực cấu hình AI Host persona.
- Khu vực định nghĩa Automation rule.
- Thư viện Media asset.

## Nguyên tắc
1. Mọi thao tác ở Workspace chỉ sửa Configuration (05_Data/CONFIGURATION.md) — không ảnh hưởng Runtime Data của bất kỳ phiên nào đang chạy.
2. UI phải hiển thị rõ trạng thái "chưa lưu" (dirty state) nếu có. Cơ chế lưu cụ thể chờ quyết định ở 10_ADR/PENDING_DECISIONS.md (mục Workspace/Persistence).

## Điều file này không định nghĩa
Không định nghĩa UI chi tiết từng form nhập liệu.
