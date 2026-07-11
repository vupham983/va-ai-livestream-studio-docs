# Message Catalog

[SOURCE OF TRUTH]
Status: Draft

Danh mục đầy đủ mọi message type trong hệ thống, mỗi loại được định nghĩa CHỈ MỘT LẦN tại đây (ADR-0010) — MODULE.md của từng module chỉ được tham chiếu, không được tự định nghĩa lại. Thêm message type mới phải thêm vào đây trước, rồi mới tham chiếu trong MODULE.md.

## Nguyên tắc control-plane vs data-plane

Event Bus (Command/Event/Query/Stream, ADR-0010) chỉ mang **message điều khiển** (control-plane) — tín hiệu báo hiệu, lệnh, trạng thái. Event Bus KHÔNG mang **dữ liệu media nặng** (audio byte thật, video frame thật) — đó là data-plane, truyền qua Media Pipe riêng (cơ chế cụ thể — shared buffer/ffmpeg pipe/khác — là chi tiết triển khai, chưa chốt ADR). Khi một message dưới đây mô tả "audio", "video", "output đã ghép", nó chỉ mang THAM CHIẾU (ID, đường dẫn, con trỏ buffer) tới dữ liệu media thật, không mang chính dữ liệu đó.

## Quy tắc đặt tên

Với message có NHIỀU instance nguồn cùng loại đang chạy song song (VD: nhiều Platform Adapter — TikTok, Facebook, Shopee... — đây chính là lý do ADR-0012 yêu cầu namespace): dùng `<domain>.<source>.<action>`, chữ thường, cách nhau bằng dấu chấm. VD: `comment.tiktok.raw`, `platform.facebook.connection_status_changed`.

Với module chỉ có đúng MỘT instance trong toàn hệ thống tại một thời điểm (Live Session, AI Host, Voice, Scene — tại một thời điểm chỉ một Scene đang active, không có nhiều Scene cùng phát sự kiện trùng tên cần phân biệt): dùng PascalCase, không cần namespace instance. VD: `GoLive`, `SessionWentLive`, `SceneActivated`, `AudioReady`. Không đổi các tên này sang dạng có dấu chấm chỉ để nhất quán hình thức — namespace chỉ tồn tại khi thực sự cần phân biệt nhiều nguồn cùng loại.

## Live Session (04_Modules/live_session/MODULE.md)

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| StartPreparation | Command | Target: Live Session | workspace_id |
| CancelPreparation | Command | Target: Live Session | session_id |
| GoLive | Command | Target: Live Session | session_id |
| Pause | Command | Target: Live Session | session_id |
| Resume | Command | Target: Live Session | session_id |
| EndSession | Command | Target: Live Session | session_id |
| StartRecovery | Command | Target: Live Session | session_id |
| RecoveryComplete | Command | Target: Live Session | session_id |
| RecoveryFailed | Command | Target: Live Session | session_id |
| ForceEnd | Command | Target: Live Session | session_id |
| SessionWentLive | Event | Producer: Live Session | session_id, timestamp |
| SessionPaused | Event | Producer: Live Session | session_id, timestamp |
| SessionResumed | Event | Producer: Live Session | session_id, timestamp |
| SessionEnded | Event | Producer: Live Session | session_id, forced (bool), timestamp |
| SessionError | Event | Producer: Live Session | session_id, fault_reason |
| SessionRecovered | Event | Producer: Live Session | session_id, timestamp |

## Platform Adapter — Streamer (04_Modules/platform_adapter/MODULE.md, server/core/platform_adapter_streamer.js)

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| StartStream | Command | Target: Platform Adapter (streamer) | rtmpUrl, segmentFiles |
| StopStream | Command | Target: Platform Adapter (streamer) | — |

## Comment / Platform Adapter (04_Modules/platform_adapter, comment_engine/MODULE.md)

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| comment.\<platform\>.raw (VD: comment.tiktok.raw, comment.facebook.raw, comment.youtube.raw, comment.shopee.raw) | Event | Producer: Platform Adapter tương ứng (ADR-0012, mỗi adapter namespace riêng) | raw comment/message text, platform user id, timestamp |
| comment.normalized.received | Event | Producer: Comment Engine (ADR-0012, producer DUY NHẤT) | classified_type (FAQ/complex/spam), normalized text, source platform |
| platform.\<name\>.connection_status_changed | Event | Producer: Platform Adapter tương ứng | platform name, status (connected/retrying/disconnected) |

## Automation → AI Host / Scene (04_Modules/automation/MODULE.md)

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| GenerateContent | Command | Target: AI Host (gửi từ Automation theo rule, hoặc Human Override trực tiếp) | context (câu hỏi/sự kiện trigger), persona đang active |
| ActivateScene | Command | Target: Scene (gửi từ Automation theo rule, hoặc người vận hành) | scene_template_id |

## AI Host → Voice (04_Modules/ai_host, voice/MODULE.md)

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| SynthesizeSpeech | Command | Target: Voice (gửi từ AI Host) | text cần chuyển giọng nói |

(Đây là ví dụ cụ thể ADR-0010 nêu để tránh mâu thuẫn: AI Host KHÔNG coi đây là Event — đây là Command đích danh Voice, có phản hồi.)

## Voice → Scene

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| AudioReady | Event | Producer: Voice | tham chiếu tới audio đã sinh (KHÔNG mang byte audio thật — xem nguyên tắc control-plane/data-plane ở trên) |

## Scene → Live Session / Platform Adapter / Health Monitor / Analytics

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| SceneActivated | Event | Producer: Scene | scene_instance_id, timestamp |
| ComposedOutputReady | Event | Producer: Scene | tham chiếu tới output đã ghép, cần Platform Adapter gửi lên nền tảng (KHÔNG mang video/audio frame thật — truyền qua Media Pipe riêng, xem nguyên tắc ở trên) |

## Media (04_Modules/media/MODULE.md)

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| GetMediaAsset | Query | Target: Media (gửi từ Scene hoặc AI Host) | Request: tham chiếu asset cần kiểm tra. Response: MediaAssetDescriptor (mô tả tồn tại/hợp lệ hay không, kèm vị trí truy cập nếu hợp lệ) — schema chi tiết không định nghĩa ở Bible |

## Health Monitor (04_Modules/health_monitor/MODULE.md)

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| HealthAlertRaised | Event | Producer: Health Monitor | sub-domain (kết nối/tài nguyên máy), mức độ (warning/critical), chi tiết |

## Human Override (ADR-0014) — áp dụng cho mọi module runtime kéo dài

| Message type | Kind | Target/Producer | Payload tóm tắt |
|---|---|---|---|
| OverrideGracefulStop | Command | Target: module cụ thể được chỉ định | module_id/target |
| OverrideForceStop | Command | Target: module cụ thể được chỉ định (mọi module PHẢI hỗ trợ, ADR-0014) | module_id/target |
| OverrideCancel | Command | Target: module sở hữu Queue cụ thể | module_id/target, item_id (việc cần huỷ khỏi hàng đợi) |
| override.\<module_name\>.completed (VD: override.platform_adapter_streamer.completed) | Event | Producer: đúng module vừa xử lý xong Override — MỖI module namespace riêng theo Quy tắc đặt tên ở trên, KHÔNG dùng chung một tên PascalCase, vì nhiều module cùng emit "hoàn tất override" sẽ vi phạm single-producer-per-event-type (ADR-0012) nếu dùng chung tên | module_id, override_type, kết quả cuối (completed/failed) |

Quyết định đặt tên (2026-07-11): ban đầu dự định OverrideCompleted dùng chung một tên cho mọi module, nhưng phát hiện xung đột với ADR-0012 khi module thứ hai (sau platform_adapter_streamer) cũng cần emit sự kiện tương tự — chọn namespace theo module (nhất quán cách ADR-0012 đã xử lý comment.<platform>.raw), không nới lỏng ADR-0012.

Ghi chú: outcome tức thời của Command Override (accepted/already_stopped/not_supported/not_found/failed) KHÔNG phải message type riêng — đó là giá trị trả về đồng bộ của chính Command (Delivery/Execution/Outcome, ADR-0011), định nghĩa đầy đủ ở ADR-0014, không lặp lại ở Catalog này.

## Điều file này không định nghĩa

Không định nghĩa schema chi tiết từng field (kiểu dữ liệu chính xác, validation) — đó là chi tiết triển khai code, không phải Architecture Bible. Danh sách trên là TOÀN BỘ message type đã biết tính đến Sprint 0 — module nào phát hiện cần message type mới trong lúc code Sprint 1 trở đi phải bổ sung vào đây trước, không tự định nghĩa riêng trong MODULE.md hay trong code.
