# ADR-0011 — Event/Stream delivery semantics

**Applies-to:** v0.x

Status: Draft

## Context

QUEUE.md (Frozen) khóa nguyên tắc "không âm thầm rớt dữ liệu" nhưng chưa định nghĩa delivery cụ thể.

## Decision

**Nguyên tắc khoá contract, không khoá implementation:** ADR này khoá CONTRACT (đảm bảo delivery gì, ordering gì) — không khoá cơ chế hiện thực hoá. MVP hiện tại chạy trong một tiến trình đơn (Node.js), mọi module PHẢI gọi nhau qua API của Event Bus (publish/subscribe/sendCommand), KHÔNG được gọi thẳng hàm của module khác (VD: avatar.js gọi thẳng tts.synthesize() là vi phạm ADR-0004, phải sửa thành gửi Command qua Event Bus) — kể cả khi cả hai đang chạy chung một process. Việc Event Bus hiện thực bằng lời gọi hàm đồng bộ trong cùng process là MỘT cách hiện thực hợp lệ của contract này ở giai đoạn MVP; khi hệ thống chuyển sang multi-process/IPC (ADR-0003), Event Bus đổi cách hiện thực nhưng KHÔNG cần Supersede ADR này vì contract không đổi.

- **Command/Query**: tách rõ 3 tầng để tránh hiểu lầm — (1) Delivery: bên gửi có thể retry, transport KHÔNG cam kết exactly-once; (2) Execution: hạ tầng dedupe theo command_id, đảm bảo side effect thực thi đúng một lần cho mỗi command_id; (3) Outcome: gửi lại cùng command_id luôn nhận lại đúng kết quả đã trả về lần đầu. Nói gọn: Command có idempotent execution contract theo command_id, KHÔNG phải cam kết transport exactly-once — đây chính là lý do ADR-0009 yêu cầu mọi Command idempotent theo command_id.
- **Event**: consumer PHẢI an toàn khi nhận trùng (idempotent theo event ID) — delivery có thể retry (at-least-once ở mức contract), dù implementation MVP hiện tại có thể chưa bao giờ thực sự retry trong thực tế.
- **Stream**: best-effort, bounded delivery — subscriber được phép sample/drop theo policy khi quá tải (đã khoá ở ADR-0010). Stream KHÔNG có cam kết delivery tối thiểu (không phải at-least-once) và KHÔNG có replay guarantee mặc định — khác bản chất với Event.
- **Ordering**: chỉ đảm bảo per-producer FIFO, KHÔNG đảm bảo thứ tự toàn cục. Nhân-quả xuyên module do module phát tự đảm bảo (Command xong mới emit Event).
- **Bounded queue**: mọi inbox có giới hạn; vượt ngưỡng báo Health Monitor (theo QUEUE.md nguyên tắc 3).
- **Khi đầy**: Stream → drop-oldest (đếm số lượng đã drop, không cần lưu vào Dead Letter Store vì bản chất Stream chấp nhận mất). Event → chờ timeout theo retry policy hiện hành rồi chuyển vào Dead Letter Store, không xoá.
- **Dead Letter Store**: nơi tập trung mọi Event/Command không thể giao hoặc xử lý được sau khi hết retry policy (số lần retry, timeout là implementation configuration, nhưng phải có telemetry và cho phép inspect từng item). Health Monitor và Analytics có thể ĐỌC Dead Letter Store, nhưng KHÔNG mặc định có quyền replay — replay là một Operational Command riêng, phải được gọi tường minh bởi người vận hành hoặc quy trình đã định nghĩa, không tự động.

## Alternatives considered

- Exactly-once cho Event/Stream — bị loại vì chi phí triển khai lớn không tương xứng lợi ích ở quy mô hiện tại.
- Global ordering — bị loại vì không có use case cần so sánh thứ tự giữa 2 producer độc lập.
- Drop âm thầm khi Event queue đầy — bị loại vì mâu thuẫn trực tiếp QUEUE.md nguyên tắc 3 (Frozen).
- Khoá cứng "đồng bộ in-process" như một quyết định kiến trúc vĩnh viễn — bị loại vì đó là chi tiết implementation của MVP, không phải contract; khoá cứng sẽ buộc Supersede khi lên multi-process.

## Consequences

- Sprint 1 Event Bus phải implement per-producer ordering, bounded inbox, Dead Letter Store (không phải "Log" — có telemetry, inspect được, replay là Command riêng).
- 05_Data/QUEUE.md không bị sửa, chỉ bổ sung phần còn thiếu.
- PENDING_DECISIONS.md mục "Event delivery semantics" → "Draft — chờ review, xem ADR-0011".
