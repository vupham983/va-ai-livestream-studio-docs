# ADR-0013 — Live Session: Aggregate mutation model

**Applies-to:** v0.x

Status: Draft

## Context

ADR-0007 (Accepted, Frozen) đã khoá Live Session là Runtime Aggregate Root — sở hữu vòng đời của toàn bộ Runtime State phát sinh trong một phiên. Nhưng ADR-0007 chỉ trả lời câu hỏi "ai sở hữu vòng đời" (creation/destruction boundary), chưa trả lời câu hỏi thứ hai trong PENDING_DECISIONS.md: khi Live Session đã là Aggregate Root, điều đó có bắt buộc MỌI mutation của Runtime State phải đi qua Command tới chính Live Session không (Aggregate thật theo nghĩa DDD chặt), hay Live Session chỉ là process/lifecycle coordinator (gate tạo/huỷ) còn field-level mutation vẫn do module con tự làm trực tiếp? Không trả lời rõ thì mỗi module sẽ tự chọn cách khác nhau khi code Sprint 1.

## Decision

**Làm rõ thuật ngữ "Aggregate Root" dùng ở ADR-0007:** Trong phạm vi ADR-0007 và ADR-0013, cụm "Live Session là Runtime Aggregate Root" được dùng theo nghĩa **lifecycle ownership** (ai gate việc tạo/huỷ, ai chịu trách nhiệm dọn dẹp khi phiên kết thúc) — KHÔNG theo nghĩa DDD chặt (toàn bộ mutation của mọi state con phải chảy qua một transactional consistency boundary duy nhất tại root). Các Runtime sub-entity thuộc về Live Session theo nghĩa vòng đời (session-scoped module state), nhưng mỗi module sở hữu consistency boundary mutation của riêng state đó — không phải sub-entity nội bộ của một DDD Aggregate cổ điển. ADR này KHÔNG Supersede ADR-0007 — chỉ xác định rõ nghĩa của thuật ngữ đã dùng, để tránh lập trình viên quen với DDD đọc ADR-0007 rồi hiểu nhầm là mọi mutation phải chảy qua Live Session.

Chọn cả hai vai trò cho Live Session, không phải một trong hai:

1. **Lifecycle coordinator (đã khoá ở ADR-0007):** Live Session gate việc tạo/huỷ mọi Runtime sub-entity nó sở hữu (Scene runtime instance, Automation execution state, AI Host session state, Comment queue...) theo state hiện tại của chính nó (VD: không sub-entity nào được tạo khi Live Session đang Idle; toàn bộ bị giải phóng/snapshot khi chuyển Ended) — theo đúng bảng transition ADR-0009.

2. **Mutation discipline (bổ sung mới ở ADR-0013):** Field-level mutation của một Runtime sub-entity KHÔNG bắt buộc phải đi qua Live Session làm trung gian — module sở hữu sub-entity đó (VD: Comment Engine sở hữu Comment Queue) tự nhận Command trực tiếp và tự mutate state của mình, theo đúng taxonomy ADR-0010 (Command đích danh module sở hữu). Live Session KHÔNG phải là điểm trung chuyển bắt buộc cho mọi Command trong hệ thống — điều đó sẽ biến nó thành một bottleneck không cần thiết.

   Ràng buộc bắt buộc còn lại: một module KHÔNG BAO GIỜ được ghi trực tiếp (đọc thì được) vào Runtime State thuộc sở hữu của module khác — chỉ được mutate qua Command gửi tới đúng module sở hữu (nguyên tắc này đã ngầm định ở ADR-0004/ADR-0010, ADR-0013 chỉ khẳng định lại rõ ràng trong ngữ cảnh Aggregate). Live Session là module DUY NHẤT sở hữu state machine của chính nó (state Idle/Preparing/Live/...) — state này chỉ đổi qua Command tới Live Session theo đúng bảng ADR-0009, không module nào khác được set trực tiếp.

Tóm lại: Live Session là Aggregate Root theo nghĩa **ranh giới sở hữu vòng đời + gate lifecycle của sub-entity**, KHÔNG phải Aggregate Root theo nghĩa **mọi mutation phải chảy qua một cửa duy nhất**. Mutation discipline được thực thi phân tán — mỗi module tự bảo vệ state của chính mình bằng cách chỉ nhận Command, không bị module khác ghi trực tiếp.

## Alternatives considered

- Aggregate thật kiểu DDD chặt — mọi mutation của mọi sub-entity đều phải qua Command gửi tới Live Session, Live Session tự uỷ quyền xuống module con — bị loại vì biến Live Session thành bottleneck, và không cần thiết vì taxonomy Command (ADR-0010) đã đảm bảo tính an toàn mutation (đích danh module sở hữu) mà không cần một điểm trung chuyển duy nhất.
- Live Session chỉ là lifecycle coordinator thuần, không có ràng buộc gì thêm về mutation — bị loại vì để ngỏ khả năng module A ghi trực tiếp vào state của module B, phá vỡ nguyên tắc Event Bus bắt buộc (ADR-0004).

## Consequences

- Mỗi MODULE.md sở hữu Runtime sub-entity phải khai rõ: sub-entity nào bị gate bởi Live Session lifecycle (tạo/huỷ theo state), và xác nhận chỉ nhận mutation qua Command gửi trực tiếp tới mình (không qua trung gian Live Session).
- ADR-0007 không bị Supersede — ADR-0013 chỉ bổ sung phần mutation discipline và làm rõ nghĩa thuật ngữ "Aggregate Root" mà ADR-0007 chưa nói rõ.
- Trong văn nói/tài liệu không chính thức (thảo luận, code comment), có thể gọi Live Session là "Runtime Session Root" hoặc "Session Lifecycle Root" để tránh nhầm với DDD Aggregate Root cổ điển — nhưng thuật ngữ chính thức trong ADR-0007 (Frozen) không đổi tên, tránh phải Supersede không cần thiết.
- PENDING_DECISIONS.md mục "Live Session là Aggregate thật... hay chỉ process/lifecycle coordinator" → "Draft — chờ review, xem ADR-0013 (kết hợp với ADR-0007)".
