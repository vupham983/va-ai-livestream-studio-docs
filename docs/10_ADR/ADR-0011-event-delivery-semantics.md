# ADR-0011 — Event/Stream delivery semantics

**Applies-to:** v0.x

Status: Draft

## Context

QUEUE.md (Frozen) khóa nguyên tắc "không âm thầm rớt dữ liệu" nhưng chưa định nghĩa delivery cụ thể.

## Decision

**Nguyên tắc khoá contract, không khoá implementation:** ADR này khoá CONTRACT (đảm bảo delivery gì, ordering gì) — không khoá cơ chế hiện thực hoá. MVP hiện tại chạy trong một tiến trình đơn (Node.js), mọi module PHẢI gọi nhau qua API của Event Bus (publish/subscribe/sendCommand), KHÔNG được gọi thẳng hàm của module khác (VD: avatar.js gọi thẳng tts.synthesize() là vi phạm ADR-0004, phải sửa thành gửi Command qua Event Bus) — kể cả khi cả hai đang chạy chung một process. Việc Event Bus hiện thực bằng lời gọi hàm đồng bộ trong cùng process là MỘT cách hiện thực hợp lệ của contract này ở giai đoạn MVP; khi hệ thống chuyển sang multi-process/IPC (ADR-0003), Event Bus đổi cách hiện thực nhưng KHÔNG cần Supersede ADR này vì contract không đổi.

- **Command/Query**: at-most-once, đồng bộ, phản hồi ngay. Retry là trách nhiệm bên gửi. Command bắt buộc idempotent (ADR-0009).
- **Event/Stream**: Event Bus contract hỗ trợ mô hình at-least-once — nghĩa là consumer KHÔNG được giả định một Event chỉ đến đúng một lần. Việc một implementation cụ thể có thực sự retry/phát lại hay không là chi tiết triển khai (MVP in-process có thể không bao giờ retry trong thực tế). Bất biến bắt buộc: mọi consumer PHẢI xử lý an toàn khi nhận trùng (idempotent theo event ID), bất kể implementation hiện tại có retry hay không.
- **Ordering**: chỉ đảm bảo per-producer FIFO, KHÔNG đảm bảo thứ tự toàn cục. Nhân-quả xuyên module do module phát tự đảm bảo (Command xong mới emit Event).
- **Bounded queue**: mọi inbox có giới hạn; vượt ngưỡng báo Health Monitor (theo QUEUE.md nguyên tắc 3).
- **Khi đầy**: Stream → drop-oldest (đếm số lượng đã drop). Event → chờ timeout rồi chuyển Dead Letter Log, không xoá.
- **Dead Letter Log**: tập trung mọi item fail delivery sau khi hết retry; Health Monitor/Analytics đọc được.

## Alternatives considered

- Exactly-once cho Event/Stream — bị loại vì chi phí triển khai lớn không tương xứng lợi ích ở quy mô hiện tại.
- Global ordering — bị loại vì không có use case cần so sánh thứ tự giữa 2 producer độc lập.
- Drop âm thầm khi Event queue đầy — bị loại vì mâu thuẫn trực tiếp QUEUE.md nguyên tắc 3 (Frozen).
- Khoá cứng "đồng bộ in-process" như một quyết định kiến trúc vĩnh viễn — bị loại vì đó là chi tiết implementation của MVP, không phải contract; khoá cứng sẽ buộc Supersede khi lên multi-process.

## Consequences

- Sprint 1 Event Bus phải implement per-producer ordering, bounded inbox, Dead Letter Log.
- 05_Data/QUEUE.md không bị sửa, chỉ bổ sung phần còn thiếu.
- PENDING_DECISIONS.md mục "Event delivery semantics" → "Draft — chờ review, xem ADR-0011".
