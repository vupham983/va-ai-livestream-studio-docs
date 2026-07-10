# ADR-0009 — Transition table đầy đủ cho Live Session state machine

**Applies-to:** v0.x

Status: Accepted

## Context

ADR-0001 định nghĩa các state (Idle → Preparing → Live → Paused → Ended + Error/Recovering) nhưng chỉ ở mức sơ đồ khái quát. Không có bảng chuyển trạng thái đầy đủ khiến mỗi module có thể tự suy luận khác nhau khi code Sprint 1.

## Decision

Không thêm state mới ngoài 7 state đã có ở ADR-0001. Bảng chuyển trạng thái đầy đủ:

| From | To | Trigger (Command) | Precondition | Side effect | Idempotent khi lặp lại? |
|---|---|---|---|---|---|
| Idle | Preparing | StartPreparation | Workspace đã load; chưa có Live Session instance nào đang chạy cho workspace này | Tạo Runtime Aggregate mới (ADR-0007), khởi tạo rỗng toàn bộ Runtime State container | Có theo Command ID (gửi lại cùng command_id → trả về cùng kết quả cũ, không tạo Aggregate thứ hai); một StartPreparation MỚI (command_id khác) khi session đã active bị từ chối rõ ràng |
| Preparing | Idle | CancelPreparation | Đang ở Preparing, chưa Live | Giải phóng Runtime Aggregate vừa tạo, không lưu Snapshot | Có — gọi lại khi đã Idle = no-op |
| Preparing | Live | GoLive | Có ít nhất một Streaming Target sẵn sàng theo chính sách của phiên live (chính sách "sẵn sàng" do module sở hữu Streaming Target đó định nghĩa) | Emit SessionWentLive, bắt đầu Snapshot định kỳ (autosave timer) | Có — gọi lại khi đã Live = no-op, log warning |
| Live | Paused | Pause | Đang Live | Bất biến duy nhất bị khóa: Pause KHÔNG kết thúc Live Session và KHÔNG hủy Runtime Aggregate. Hành vi cụ thể với từng Streaming Target (giữ kết nối, mute, frame chờ, ngắt/reconnect...) do module sở hữu target tự quyết theo đặc thù nền tảng. Automation/AI Host dừng phát sinh action mới; ép Snapshot ngay lập tức | Có — gọi lại khi đã Paused = no-op |
| Paused | Live | Resume | Đang Paused | Automation/AI Host tiếp tục hoạt động; mỗi module tự khôi phục trạng thái kết nối/hiển thị của nó | Có — gọi lại khi đã Live = no-op |
| Live / Paused | Ended | EndSession | Không yêu cầu precondition | Snapshot cuối cùng (archive), giải phóng toàn bộ Runtime Aggregate, mỗi module tự đóng kết nối Streaming Target của nó, emit SessionEnded | Có — gọi lại khi đã Ended = no-op |
| Preparing | Ended | (fault nghiêm trọng khi đang Preparing) | Fault severity = critical, xảy ra trước khi từng vào Live | Abort thẳng về Ended, KHÔNG qua Error/Recovering — vì chưa có runtime state nào của một phiên đã sống cần phục hồi, coi như phiên chưa từng bắt đầu | Có |
| Live/Paused/Recovering | Error | (không phải Command — do Health Monitor/module báo fault nghiêm trọng) | Fault severity = critical, đã từng vào Live | Đóng băng mọi mutation trừ Command Recovery; emit SessionError; cố gắng tạo emergency Snapshot | Có — vào Error nhiều lần = giữ nguyên Error |
| Error | Recovering | StartRecovery | Root cause đã xác định/khắc phục | Bắt đầu khôi phục từ Snapshot hợp lệ gần nhất | Không áp dụng |
| Recovering | Live hoặc Paused (state trước Error) | RecoveryComplete | Snapshot khôi phục hợp lệ; module lõi healthy trở lại | Emit SessionRecovered, tiếp tục từ state đã khôi phục | Không áp dụng |
| Recovering | Error | RecoveryFailed | Khôi phục thất bại | Escalate lên Health Monitor, chờ can thiệp thủ công | Không áp dụng |
| Error hoặc Recovering | Ended | ForceEnd (thủ công, không tự động) | Không yêu cầu — dùng khi xác định không thể phục hồi | Snapshot cứu hộ nếu có thể, giải phóng Runtime Aggregate, emit SessionEnded với flag forced=true | Có — gọi lại khi đã Ended = no-op |

**Paused không tự timeout:** Paused chờ vô hạn tới khi có Command Resume hoặc EndSession từ người vận hành. Không có cơ chế tự động thoát Paused sau một khoảng thời gian, vì tự động kết thúc một phiên đang tạm dừng giữa lúc chốt đơn hàng là rủi ro nghiệp vụ không chấp nhận được.

**Làm rõ nguyên tắc idempotency chung:** "Idempotent" trong bảng trên được hiểu theo hai tầng, không mâu thuẫn nhau: (1) idempotent theo Command ID — gửi lại đúng command_id đã xử lý sẽ trả về cùng kết quả, không thực thi lại side effect; (2) idempotent theo state đích — gọi lại một Command khi hệ thống đã ở đúng state đích thì là no-op. StartPreparation là trường hợp đặc biệt có chủ đích: idempotent theo command_id (retry an toàn) nhưng KHÔNG idempotent theo state đích — một StartPreparation mới khi đã có session active phải bị từ chối, không phải no-op, để giữ bất biến "mỗi Workspace chỉ có một Live Session đang hoạt động".

## Alternatives considered

- Giữ sơ đồ khái quát, để mỗi module tự suy luận — bị loại vì đây là nguồn gốc pending decision.
- Khóa cứng hành vi kết nối cụ thể khi Pause — bị loại vì Pause khác nhau giữa nền tảng (TikTok/Facebook/OBS); chỉ khóa bất biến ở tầng Live Session.
- Dùng "Platform Adapter healthy" làm precondition GoLive — bị loại vì thu hẹp sai phạm vi (sẽ có Streaming Target không phải Platform Adapter: local recording, OBS...).
- Thêm state Ending — bị loại để không phá vỡ ADR-0001.
- Cho Preparing lỗi cũng đi qua Error/Recovering như các state khác — bị loại vì không có runtime state thật nào cần phục hồi ở giai đoạn này, đi qua Error/Recovering chỉ thêm độ phức tạp không cần thiết.

## Consequences

- Sprint 1 phải implement đúng bảng trên.
- Mỗi module sở hữu Streaming Target tự định nghĩa "sẵn sàng" và hành vi Paused/Resumed/Ended trong MODULE.md của nó.
- Ràng buộc trực tiếp lên ADR-0011: mọi Command phải có idempotent execution contract theo command_id (không phải cam kết transport exactly-once) — xem nguyên tắc hai tầng vừa làm rõ ở trên.
- PENDING_DECISIONS.md mục "Transition table đầy đủ" → "Draft — chờ review, xem ADR-0009".
