# ADR-0014 — Human Override Semantics

**Applies-to:** v0.x

Status: Draft

## Context

Audit code thật (2026-07-10) phát hiện avatar.js và streamer.js xử lý override của người vận hành theo hai cách không nhất quán: avatar không hủy job đang chạy dở, streamer giết tiến trình ngay lập tức. Đây không phải bug lẻ tẻ mà là thiếu một mô hình chung. Giải quyết sớm 2 mục trong nhóm "Bắt buộc trước khi code Engine/UI Process" của PENDING_DECISIONS.md.

## Decision

Định nghĩa 3 loại Human Override Command, áp dụng thống nhất cho mọi module có hành động runtime kéo dài:

- **GracefulStop**: hoàn tất đơn vị công việc hiện tại, không nhận việc mới, rồi dừng.
- **ForceStop**: dừng ngay lập tức bất kể trạng thái dở dang. Module nhận ForceStop bắt buộc tự dọn dẹp tài nguyên đã cấp phát (file tạm, tiến trình con, kết nối mạng) một cách idempotent.
- **Cancel**: hủy một mục việc còn đang CHỜ xử lý (chưa bắt đầu chạy) khỏi hàng đợi, không ảnh hưởng việc đang chạy dở.

Mỗi module sở hữu hành động runtime kéo dài PHẢI implement tối thiểu ForceStop; GracefulStop và Cancel tuỳ chọn theo bản chất công việc, phải khai báo rõ trong MODULE.md mình hỗ trợ loại nào.

Override là Command theo taxonomy ADR-0010 — luôn đích danh MỘT module cụ thể (per-module), không có "nút kill toàn cục" duy nhất. Live Session điều phối quá trình dừng toàn bộ phiên bằng cách phát override Command tới từng module đang có việc chạy dở, nhưng ADR này KHÔNG quy định thứ tự dừng cụ thể giữa các module — thứ tự là trách nhiệm của orchestration policy, có thể thay đổi theo tình huống mà không cần Supersede ADR này.

Áp dụng cụ thể: avatar.js (job MuseTalk) phải hỗ trợ Cancel (xoá khỏi hàng đợi nếu chưa chạy) và ForceStop (nếu đang chạy: kill subprocess lip-sync + xoá file output dở dang); streamer.js giữ hành vi kill ngay hiện tại nhưng map đúng vào ForceStop, đảm bảo giải phóng kết nối RTMP.

**Kết quả chuẩn cho Override Command:** Mỗi Override Command PHẢI trả về một trong các outcome: `accepted` (yêu cầu được nhận, bắt đầu xử lý), `already_stopped` (module đã ở trạng thái dừng, no-op), `not_supported` (module không hỗ trợ loại override này), `not_found` (không có việc đang chạy/chờ để override), `failed` (không thể thực hiện). Với ForceStop có cleanup bất đồng bộ: `accepted` CHỈ nghĩa là yêu cầu dừng đã được nhận — KHÔNG đồng nghĩa cleanup đã hoàn tất. Module phải phát Event riêng khi cleanup thực sự xong (VD: `overrideCompleted` / `overrideFailed`), phân biệt rõ accepted (đã nhận yêu cầu) vs completed (đã dọn xong) vs failed (dọn thất bại).

## Alternatives considered

- Một loại Override duy nhất áp dụng chung mọi module — bị loại vì bản chất công việc khác nhau thật sự cần xử lý khác nhau.
- Override toàn cục (một lệnh dừng hết) — bị loại vì không cho phép dừng có chọn lọc từng module.
- Khoá cứng thứ tự dừng module trong ADR — bị loại vì đây là chính sách vận hành có thể thay đổi theo tình huống, không phải bất biến kiến trúc.

## Consequences

- Mỗi MODULE.md có hành động runtime kéo dài phải khai báo phần "Override Support: GracefulStop/ForceStop/Cancel — có/không".
- PENDING_DECISIONS.md nhóm "Bắt buộc trước khi code Engine/UI Process" → 2 mục override "Draft — chờ review, xem ADR-0014".
- Việc sửa avatar.js và streamer.js theo ADR này (implement cụ thể Cancel/ForceStop + outcome contract) KHÔNG thực hiện trong Sprint 0 — đây là công việc migration code, thuộc Sprint 1 hoặc một Migration Sprint riêng ("Wrap legacy runtime"), đúng như đã ghi nhận trong CODE_GAP_ANALYSIS.md. ADR-0014 tự nó có thể Accepted độc lập với việc code chưa migrate.
