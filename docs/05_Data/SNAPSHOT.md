# Snapshot

[SOURCE OF TRUTH]
Status: Approved

Bản chụp trạng thái Runtime Data tại một thời điểm, dùng để phục hồi hoặc lưu trữ sau khi Live Session Ended hoặc khi Engine Process crash (chương 07 — Failure Recovery).

## Khi nào Snapshot được tạo

- Khi Live Session chuyển sang Ended (lưu trữ bình thường theo vòng đời).
- Định kỳ trong lúc phiên đang Live, để hỗ trợ phục hồi nếu Engine Process crash giữa chừng (autosave — cơ chế cụ thể chưa chốt, sẽ quyết định ở ADR riêng khi triển khai, tương ứng quyết định treo #10 ban đầu, liên quan chương 07).

## Nguyên tắc

1. Snapshot chỉ chứa Runtime Data (chương 02) — không chứa Configuration, vì Configuration đã tồn tại độc lập ở Workspace, không cần chụp lại (tránh trùng lặp, Principles #4).
2. Một crash Engine Process không được làm mất Workspace (chương 07) — nguyên tắc này áp dụng độc lập với Snapshot, vì Workspace vốn không thuộc phạm vi Snapshot.
3. Khôi phục từ Snapshot phải kiểm tra tính hợp lệ của tham chiếu (VD: Scene template được tham chiếu trong Snapshot vẫn còn tồn tại ở Workspace hay không) trước khi khôi phục hoàn toàn — nếu tham chiếu đã mất, xử lý theo nguyên tắc Failure Mode tương ứng ở module sở hữu dữ liệu đó (VD: scene/MODULE.md khi Media asset thiếu).

## Điều file này không định nghĩa

Không định nghĩa tần suất autosave cụ thể, định dạng lưu trữ vật lý, hay cơ chế nén/mã hoá — các quyết định này chờ ADR riêng khi bắt đầu triển khai (đã ghi nhận là quyết định treo từ chương 07).
