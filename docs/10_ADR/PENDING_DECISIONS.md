# Pending Decisions Registry

[CHỈ THAM CHIẾU]
Status: Draft

Danh sách quyết định kỹ thuật CHƯA được giải quyết ở Documentation Bible, phát hiện qua audit độc lập (2026-07-10). Mỗi mục PHẢI được giải quyết bằng một ADR trước khi bắt đầu code phần liên quan — không được code trước rồi suy ra quyết định.

## Bắt buộc trước khi code Live Session / Event Bus
- Transition table đầy đủ cho state machine — Resolved, xem ADR-0009.
- Mô hình phân loại Command/Event/Query/Stream — Resolved, xem ADR-0010.
- Event delivery semantics — Resolved, xem ADR-0011.
- Cơ chế thực thi "một event một nguồn phát" — Resolved, xem ADR-0012.
- Live Session là Aggregate thật hay chỉ process/lifecycle coordinator — Resolved, xem ADR-0007 + ADR-0013.

## Bắt buộc trước khi code Engine/UI Process
- Giao thức IPC cụ thể (đã ghi nhận ở chương 05, nhắc lại ở đây để không bị quên).
- Ai là process supervisor khi Engine Process crash — UI, hay cần process thứ ba (watchdog)?
- Concurrency model của Engine Process (event loop/thread pool, CPU-bound vs I/O-bound).
- Override có huỷ (cancel) task đang chạy dở hay để chạy xong mới dừng — Resolved, xem ADR-0014.
- Override là global hay per-module, và protocol chung để mọi module tuân theo nhất quán — Resolved, xem ADR-0014.

## Bắt buộc trước khi code Workspace/Persistence
- Kiến trúc lưu trữ: database hay file-based, atomic save, schema version, migration, backward compatibility.
- Workspace Configuration State nằm ở UI Process hay Engine Process, và cơ chế UI sửa cấu hình (ghi trực tiếp hay qua IPC command).
- Chia sẻ Workspace giữa nhiều máy/nhiều người (Persona 2 cần, chưa có cơ chế).
- Mã hoá API key/credential tại chỗ (at-rest) trên máy người dùng.
- "Snapshot" đang gánh 4 nghĩa khác nhau (crash recovery, archive, resume, analytics) — cần tách rõ trước khi thiết kế schema.

## Bắt buộc trước khi code Platform Adapter/Integration
- Tách Platform Adapter hiện tại thành 3 nhóm bản chất khác nhau (Broadcast Platform, Production Control/OBS, AI Provider) hay giữ một contract chung — ảnh hưởng trực tiếp interface.
- OBS ownership: Scene là domain model hay renderer, ai là nguồn sự thật khi người dùng đổi Scene trực tiếp trong OBS.
- Backpressure khi tốc độ comment vượt khả năng xử lý (drop/sample/dedupe/priority).
- Clock/time model: timestamp tạo ở đâu, xử lý event đến trễ, clock drift qua phiên dài.
- Identity model cho các entity runtime (SceneRuntimeInstanceId, SessionId, correlation ID...).

## Bắt buộc trước khi thương mại hoá
- Security architecture đầy đủ (nơi lưu OAuth/API key, token refresh, log redaction).
- Privacy/data-retention policy (comment, voice, conversation history lưu bao lâu, xoá ở đâu).
- Update/migration/compatibility strategy giữa các phiên bản.

## Nguyên tắc dùng file này
1. Không mục nào trong đây tự động trở thành quyết định — mỗi mục cần một ADR riêng khi tới lúc code phần liên quan.
2. Trước khi bắt đầu code một khu vực (Live Session, Engine/UI, Workspace, Platform Adapter), TẤT CẢ mục thuộc khu vực đó trong registry này phải có ADR tương ứng — không được bỏ qua vì "sẽ tự quyết lúc code".
3. Khi một mục được giải quyết bằng ADR, đánh dấu "Resolved — xem ADR-00XX" ngay tại đây, không xoá dòng.
