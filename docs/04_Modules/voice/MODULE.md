# Voice — MODULE

[SOURCE OF TRUTH]
Status: Approved

## Purpose

Chuyển văn bản (từ AI Host hoặc kịch bản) thành giọng nói phát ra trong phiên live (GLOSSARY.md — Voice).

## Public Contract

- Nhận một đoạn văn bản và chuyển thành audio theo giọng đọc/tốc độ/tông giọng đã cấu hình.
- Phát audio đã sinh cho Scene để phát đồng thời với hiển thị.
- Cho phép đổi TTS provider (Edge/XTTS/ElevenLabs...) mà không ảnh hưởng module nghiệp vụ khác gọi tới Voice.

## Responsibilities

- Sở hữu việc chuyển đổi văn bản → audio.
- Sở hữu cấu hình giọng đọc (provider, giọng, tốc độ, tông giọng) ở lớp Workspace.
- Phát audio đã sinh dưới dạng event/dữ liệu cho Scene tiêu thụ.

## Boundaries (không làm gì)

- Không sinh nội dung, không quyết định nói gì — chỉ chuyển văn bản đã có sẵn (từ AI Host) thành giọng nói (chương 03).
- Không tự quyết định khi nào phát audio ra người xem — việc đồng bộ hiển thị/phát thuộc Scene.
- Không lộ chi tiết TTS provider đang dùng ra module nghiệp vụ khác (AI Host không biết Voice dùng provider nào — đã xác nhận ở ai_host/MODULE.md).

## Inputs

- Văn bản cần chuyển giọng nói (từ AI Host).

## Outputs

- Audio đã sinh — emit/chuyển cho Scene để phát đồng thời với hiển thị (khớp 02_Architecture/chapters/04_data_flow.md: Voice --audio--> Scene).

## Upstream Dependencies

- AI Host — nguồn nội dung text cần chuyển giọng nói.

## Downstream Consumers

- Scene — nhận audio để phát cùng hiển thị (đã xác nhận ở scene/MODULE.md, Upstream Dependencies).

## Configuration State

- Provider TTS đang chọn (Edge/XTTS/ElevenLabs...), giọng đọc, tốc độ, tông giọng — thuộc sở hữu Workspace, tái sử dụng giữa nhiều Live Session.

## Runtime State

Thuộc sở hữu Live Session theo Aggregate Boundary (chương 02, ADR-0007) — mất khi phiên Ended:
- Trạng thái đang phát/đang xử lý audio của phiên hiện tại (đang tổng hợp, đang chờ, rảnh).

## Invariants

- Voice không được tự chọn nội dung để đọc — chỉ đọc đúng văn bản nhận từ AI Host.
- Module nghiệp vụ gọi Voice (AI Host, Scene) không được phép biết hoặc phụ thuộc vào chi tiết kỹ thuật của TTS provider đang chạy — Voice là một abstraction (chương 06).

## Failure Modes

- TTS provider lỗi/timeout (phân loại Non-critical/Critical theo chương 07, tuỳ mức độ ảnh hưởng luồng chính đang Live).
- Audio sinh ra không khớp thời lượng dự kiến so với văn bản đầu vào (Non-critical).

## Recovery Strategy

- Provider lỗi/timeout: retry theo cơ chế chung ở chương 07; nếu lặp lại → cảnh báo Health Monitor, có thể chuyển sang provider dự phòng nếu đã cấu hình (Configuration State), hoặc chờ Human Override.
- Audio lệch thời lượng: cảnh báo Health Monitor ở mức thông tin, không chặn phát audio (không ảnh hưởng luồng chính).

## Extension Points

- Đổi hoặc thêm TTS provider mới là một điểm mở rộng kỹ thuật thật (khác các module trước chỉ mở rộng qua cấu hình Workspace) — tương tự một Platform Adapter tích hợp (xem 06_Integrations/tts/INTEGRATION.md). Module nghiệp vụ gọi Voice không cần biết provider nào đang chạy, đúng nguyên tắc mở rộng ở chương 06.

## Related ADRs

- ADR-0004 — Event Bus bắt buộc.
- ADR-0007 — Live Session = Runtime Aggregate Root (Runtime State của Voice thuộc sở hữu Live Session).
