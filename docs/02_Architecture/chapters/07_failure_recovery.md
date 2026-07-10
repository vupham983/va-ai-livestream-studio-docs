# 07 — Failure Recovery

[SOURCE OF TRUTH]
Status: Frozen

Hệ thống phản ứng thế nào khi một phần bị lỗi, liên kết trực tiếp với trạng thái Error/Recovering ở chương 02.

## Nguyên tắc phục hồi

Một lỗi cục bộ không được kéo sập toàn bộ Live Session (PRODUCT_GOALS.md Goal 2, NON_FUNCTIONAL_SCOPE.md — Availability). Hệ thống phân biệt hai loại lỗi:

1. **Lỗi không thiết yếu (Non-critical)** — một module phụ trợ lỗi (VD: Analytics tạm ngưng ghi số liệu). Live Session tiếp tục ở trạng thái Live, chỉ module đó chuyển sang trạng thái lỗi cục bộ, Health Monitor cảnh báo.
2. **Lỗi thiết yếu (Critical)** — một module thuộc luồng vận hành chính lỗi (VD: mất kết nối toàn bộ Platform Adapter). Live Session chuyển sang Error/Recovering, cố gắng khôi phục; nếu không khôi phục được trong thời gian hợp lý, chuyển sang Paused và yêu cầu Human Override.

## Vai trò của Health Monitor trong phục hồi

Health Monitor không tự sửa lỗi (đúng ranh giới ở chương 03) — nó chỉ phát hiện và emit event cảnh báo. Việc quyết định phục hồi thế nào là của chính module bị lỗi (tự khôi phục nếu có thể) hoặc của người vận hành (Human Override).

## Phục hồi trạng thái sau khi Engine Process crash

Nếu toàn bộ Engine Process crash (khác với lỗi cục bộ một module), đây là tình huống nghiêm trọng nhất. Chiến lược cụ thể (autosave, snapshot, khôi phục từ đâu) sẽ được quyết định ở ADR riêng (ADR về autosave/recovery, tương ứng quyết định treo #10 trước đây) khi 05_Data/SNAPSHOT.md được viết — chương này chỉ khẳng định nguyên tắc: **một crash Engine Process không được làm mất toàn bộ cấu hình Workspace**, vì Workspace không thuộc sở hữu runtime của Live Session (chương 02).

## Người vận hành luôn là phương án dự phòng cuối cùng

Khi hệ thống không tự phục hồi được, Human Override luôn là lối thoát cuối: người vận hành có thể dừng phiên thủ công, không bị AI hoặc Automation chặn quyền này trong bất kỳ trạng thái nào (Philosophy #2).

## Điều chương này không định nghĩa

Không định nghĩa cơ chế snapshot/autosave kỹ thuật cụ thể (thuộc 05_Data/SNAPSHOT.md và ADR riêng), không định nghĩa retry logic chi tiết của từng module (thuộc 04_Modules/*/MODULE.md, mục Failure modes).
