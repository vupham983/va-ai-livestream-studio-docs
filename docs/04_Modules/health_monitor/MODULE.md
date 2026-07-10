# Health Monitor — MODULE

[SOURCE OF TRUTH]
Status: Approved

## Purpose

Giám sát tình trạng kết nối nền tảng và tài nguyên máy, cảnh báo sớm nguy cơ gãy Live Session (GLOSSARY.md — Health Monitor; ADR-0005).

## Public Contract

- Theo dõi liên tục tình trạng kết nối tới các nền tảng livestream và tài nguyên máy (CPU/RAM/mạng).
- Phát cảnh báo khi phát hiện nguy cơ ảnh hưởng tới phiên đang chạy.
- Không tự sửa lỗi — chỉ phát hiện và cảnh báo, để Automation hoặc người vận hành quyết định hành động.

## Responsibilities

- Sở hữu logic giám sát cả hai sub-domain: kết nối nền tảng và tài nguyên máy, trong cùng một module (ADR-0005).
- Phát cảnh báo kịp thời khi phát hiện bất thường, đủ sớm để Automation/người vận hành có thể phản ứng trước khi ảnh hưởng phiên đang chạy.

## Boundaries (không làm gì)

- Không tự sửa lỗi — chỉ phát hiện và cảnh báo (chương 03); hành động khắc phục thuộc Automation hoặc Human Override.
- Không đưa ra quyết định thay cho module khác — chỉ cung cấp thông tin cảnh báo, module nhận cảnh báo tự quyết định phản ứng.

## Inputs

- Sự kiện tình trạng kết nối từ Platform Adapter.
- Dữ liệu tài nguyên hệ điều hành (CPU/RAM/mạng) — đọc trực tiếp từ hệ điều hành, không qua Event Bus (chương 08 — giới hạn tài nguyên máy người dùng).

## Outputs

- Sự kiện cảnh báo (kết nối nền tảng bất ổn, tài nguyên máy quá tải...) — emit ra Event Bus cho module khác subscribe.

## Upstream Dependencies

- Platform Adapter — nguồn sự kiện tình trạng kết nối.
- Hệ điều hành/tài nguyên máy (chương 01 — nhóm "Hệ điều hành/tài nguyên máy") — Health Monitor là module duy nhất được phép đọc trực tiếp tài nguyên máy, không qua Event Bus, vì đây là nguồn dữ liệu hệ thống, không phải dữ liệu do module khác phát ra.

## Downstream Consumers

- Mọi module khác có thể subscribe cảnh báo từ Health Monitor để tự quyết định phản ứng — đặc biệt Automation (đã xác nhận ở automation/MODULE.md, Upstream Dependencies).

## Configuration State

- Ngưỡng cảnh báo (VD: mức CPU/RAM coi là quá tải, thời gian mất kết nối coi là bất thường) — thuộc sở hữu Workspace, tái sử dụng giữa nhiều Live Session.

## Runtime State

Thuộc sở hữu Live Session theo Aggregate Boundary (chương 02, ADR-0007) — mất khi phiên Ended:
- Trạng thái Health hiện tại của phiên (bình thường/cảnh báo/nghiêm trọng) cho từng sub-domain (kết nối nền tảng, tài nguyên máy).

## Invariants

- Health Monitor không được là nguồn gây thêm tải đáng kể cho hệ thống nó đang giám sát — không làm nặng thêm vấn đề nó đang theo dõi.
- Health Monitor không tự sửa lỗi dưới bất kỳ hình thức nào — mọi hành động khắc phục đi qua Automation hoặc Human Override.

## Failure Modes

- Bản thân Health Monitor bị lỗi/treo — trường hợp đặc biệt nghiêm trọng vì không còn ai giám sát người giám sát (Critical, theo chương 07). Phát hiện qua một cơ chế heartbeat đơn giản (Health Monitor tự báo hiệu còn hoạt động theo chu kỳ; vắng heartbeat quá một ngưỡng được coi là dấu hiệu treo) — chi tiết kỹ thuật cụ thể của cơ chế heartbeat không thuộc phạm vi Architecture Bible.
- Không nhận được dữ liệu tài nguyên máy hoặc dữ liệu kết nối trong một khoảng thời gian (Non-critical nếu tạm thời, Critical nếu kéo dài — theo chương 07).

## Recovery Strategy

- Bản thân Health Monitor treo: hệ thống (ở mức runtime, ngoài phạm vi module này) cần một cơ chế phát hiện vắng heartbeat và cảnh báo trực tiếp tới người vận hành qua UI, vì không có module giám sát nào khác đứng sau Health Monitor.
- Thiếu dữ liệu giám sát tạm thời: tiếp tục dùng trạng thái Health gần nhất đã biết, gắn nhãn "dữ liệu cũ", không tự suy diễn trạng thái mới.

## Extension Points

- Thêm loại tài nguyên/kết nối cần giám sát mới là thay đổi cấu hình ngưỡng cảnh báo (Workspace), không phải mở rộng module theo nghĩa plugin (chương 06).

## Related ADRs

- ADR-0004 — Event Bus bắt buộc.
- ADR-0005 — Health Monitor giám sát cả hai sub-domain trong một module duy nhất.
- ADR-0007 — Live Session = Runtime Aggregate Root (Runtime State của Health Monitor thuộc sở hữu Live Session).
