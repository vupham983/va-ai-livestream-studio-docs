# Interaction Patterns

[SOURCE OF TRUTH]
Status: Frozen

## Ba loại hành động cần phân biệt
1. Destructive configuration action (VD: xoá Scene template) — luôn qua Dialog xác nhận.
2. Live operational action (VD: chuyển Scene đang phát) — có thể cần xác nhận tuỳ mức rủi ro, không mặc định dùng Dialog.
3. Emergency override action (VD: Stop runaway voice) — không bao giờ qua Dialog, thực thi ngay (Philosophy #2).

## Mẫu tương tác
- Destructive configuration action luôn qua Dialog (xem DIALOGS.md).
- Emergency override không cần xác nhận thêm — đây là ngoại lệ tường minh với nguyên tắc xác nhận ở trên.
- Trạng thái đang tải/đang xử lý phải hiển thị rõ, không để UI đứng im không phản hồi.

## Nguyên tắc
Interaction Pattern không được mâu thuẫn PRINCIPLES.md #2 (Con người thắng tự động hoá) — bất kỳ pattern nào yêu cầu xác nhận thêm cho Emergency Override đều vi phạm nguyên tắc này.

## Điều file này không định nghĩa
Không định nghĩa animation, transition timing cụ thể.
