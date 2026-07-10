# ADR-0010 — Command / Event / Query / Stream taxonomy

**Applies-to:** v0.x

Status: Draft

## Context

05_Data/EVENT.md gộp chung mọi giao tiếp liên module dưới một khái niệm "Event". Hệ thống cần phân biệt: ép hành động có phản hồi, thông báo sự kiện không cần phản hồi, truy vấn không side-effect, và luồng dữ liệu tần suất cao.

## Decision

Tách 4 loại message, được truyền qua messaging infrastructure của hệ thống (gọi chung là Event Bus trong phạm vi Bible này, không ngụ ý đây là nơi lưu trữ lâu dài), khác nhau về routing/contract:

- **Command**: đích danh MỘT module, bắt buộc có phản hồi (success/failure). Con đường DUY NHẤT thay đổi state của module khác.
- **Event**: thông báo sự kiện đã xảy ra, không yêu cầu phản hồi, một producer nhiều subscriber (giữ nguyên ADR-0004).
- **Query**: đích danh một module, có phản hồi, KHÔNG side effect.
- **Stream**: chuỗi dữ liệu liên tục, có thứ tự trong phạm vi một producer, subscriber được phép sample/drop khi quá tải. MVP hiện tại (single-process, chưa có camera preview/audio meter/waveform) chưa có module nào phát Stream trong thực thi đầu tiên — nhưng loại này vẫn giữ trong taxonomy vì Roadmap đã xác định các nhu cầu này sẽ phát sinh, không xoá khỏi Bible chỉ vì chưa dùng ở MVP.

**Routing:** Command & Query = point-to-point. Event & Stream = broadcast.

Mỗi message type CHỈ được định nghĩa (tên, loại trong 4 loại, payload) ở đúng MỘT nơi duy nhất — Source of Truth là 05_Data/MESSAGE_CATALOG.md (file mới, mở rộng từ EVENT.md). MODULE.md của từng module CHỈ được tham chiếu tới message type đã định nghĩa ở đó (liệt kê message type nào module mình produce/consume), KHÔNG được tự định nghĩa lại loại hay ý nghĩa của một message type đã tồn tại. Nếu một module cần một message type mới, phải thêm vào MESSAGE_CATALOG.md trước, rồi mới được tham chiếu trong MODULE.md của mình — tránh tình trạng hai module gán loại khác nhau cho cùng một tên message type (VD: AI Host coi TextGenerated là Event trong khi Voice coi là Command).

## Alternatives considered

- Giữ một loại "Event" duy nhất — bị loại vì buộc Event Bus xử lý 2 kiểu contract mâu thuẫn dưới cùng API.
- Chỉ tách 2 loại (Command vs Event) — bị loại vì Query cần tường minh "không side-effect", Stream cần cho phép mất dữ liệu mà Event thì không.
- Bỏ hẳn Stream khỏi taxonomy vì MVP chưa dùng — bị loại vì Roadmap đã xác định nhu cầu Stream (camera preview, audio meter...) sẽ phát sinh; bỏ rồi thêm lại sau sẽ phải Supersede không cần thiết.
- Để mỗi module tự gắn nhãn message type trong MODULE.md của mình — bị loại vì dễ sinh mâu thuẫn giữa các module về cùng một message type.

## Consequences

- 05_Data/EVENT.md (Frozen) cần bổ sung section tham chiếu ADR-0010, không xoá nội dung cũ.
- 05_Data/MESSAGE_CATALOG.md (file mới) trở thành Source of Truth cho toàn bộ message type trong hệ thống; mọi MODULE.md hiện có cần rà soát lại để đảm bảo không tự định nghĩa trùng. **MESSAGE_CATALOG.md phải tồn tại và đầy đủ TRƯỚC KHI Sprint 1 bắt đầu — đây là entry condition bổ sung cho Sprint 1 (cùng cấp với các entry condition đã ghi ở IMPLEMENTATION_ORDER.md), không phải việc làm dần trong lúc code.**
- PENDING_DECISIONS.md mục "Command/Event/Query/Stream" → "Draft — chờ review, xem ADR-0010".
