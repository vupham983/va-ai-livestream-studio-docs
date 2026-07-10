# Automation — MODULE

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Một Rule Engine chạy rule/trigger xác định trước (if X → do Y) để phản ứng tự động theo sự kiện trong phiên live, không tự sinh nội dung (GLOSSARY.md — Automation; ADR-0002).

## Public Contract

- Định nghĩa và lưu rule (điều kiện trigger + hành động) trong Workspace, tái sử dụng giữa nhiều Live Session.
- Theo dõi sự kiện trong phiên đang chạy và tự động thực thi hành động khi điều kiện rule khớp.
- Gọi AI Host như một hành động khi rule cần nội dung linh hoạt; gọi Scene như một hành động khi rule cần đổi hiển thị.

## Responsibilities

- Sở hữu định nghĩa rule (điều kiện, hành động) ở lớp Workspace.
- Sở hữu trạng thái thực thi rule trong một phiên đang chạy (rule nào đang theo dõi, đã trigger bao nhiêu lần).
- Là điểm duy nhất quyết định "khi nào" một hành động tự động được thực thi, dựa trên rule đã cấu hình trước.

## Boundaries (không làm gì)

- Không tự sinh nội dung tự do — khi cần nội dung phải gọi AI Host như một action, không chứa logic sinh nội dung (ADR-0002, chương 03).
- Không tự quyết định hiển thị — khi cần đổi Scene phải gọi Scene như một action, không tự vẽ/hiển thị.
- Không tự suy luận hoặc mở rộng phạm vi ngoài rule đã cấu hình — chỉ thực thi đúng điều kiện đã định nghĩa trước.

## Inputs

- Sự kiện từ Comment Engine (câu hỏi/sự kiện cần xử lý đã được phân loại).
- Cảnh báo từ Health Monitor (có thể dùng làm điều kiện trigger cho rule, VD: tự động chuyển Scene dự phòng khi có cảnh báo).
- Sự kiện trạng thái phiên từ Live Session (Idle/Preparing/Live/Paused/Ended) — Automation là module hợp lệ duy nhất lắng nghe trực tiếp event trạng thái phiên để chạy rule (khác AI Host, vốn không được nghe trực tiếp Live Session — xem ai_host/MODULE.md, Upstream Dependencies).

## Outputs

- Lệnh hành động (gọi AI Host sinh nội dung, gọi Scene đổi hiển thị, hoặc hành động khác đã cấu hình trong rule) — emit ra Event Bus.

## Upstream Dependencies

- Comment Engine — nguồn sự kiện/câu hỏi cần xử lý.
- Health Monitor — nguồn cảnh báo có thể làm điều kiện trigger.
- Live Session — nguồn event trạng thái phiên làm trigger cho rule (VD: chào khi Live bắt đầu, tạm biệt khi Ended).

## Downstream Consumers

- AI Host — được gọi như một action khi rule cần nội dung linh hoạt (ADR-0002).
- Scene — được gọi như một action khi rule cần đổi Scene.

## Configuration State

- Rule definition: điều kiện trigger + hành động tương ứng — thuộc sở hữu Workspace, tái sử dụng giữa nhiều Live Session (Mission 6, Philosophy #3).

## Runtime State

Thuộc sở hữu Live Session theo Aggregate Boundary (chương 02, ADR-0007) — mất khi phiên Ended:
- Danh sách rule đang được theo dõi trong phiên hiện tại.
- Số lần mỗi rule đã trigger trong phiên.
- Trạng thái chờ phản hồi (nếu rule vừa gọi AI Host/Scene và đang chờ xác nhận).

## Invariants

- Một rule chỉ trigger đúng theo điều kiện định nghĩa trước, không tự suy luận hoặc mở rộng phạm vi ngoài rule đã cấu hình.
- Automation không được chứa logic sinh nội dung dưới bất kỳ hình thức nào — mọi nội dung linh hoạt phải qua AI Host.
- Automation không được tự ý đổi trạng thái phiên trực tiếp — chỉ được gửi yêu cầu qua Event Bus tới Live Session (chương 02).

## Failure Modes

- Rule bị trigger sai hoặc trigger lặp lại ngoài ý muốn (Non-critical).
- Rule gọi AI Host hoặc Scene nhưng không nhận phản hồi trong thời gian hợp lý (phân loại Non-critical/Critical theo chương 07, tuỳ rule đó thuộc luồng chính hay phụ trợ).

## Recovery Strategy

- Rule trigger sai/lặp: cảnh báo Health Monitor, cho phép người vận hành tạm ngưng (disable) rule đó qua Human Override mà không ảnh hưởng các rule khác.
- Không nhận phản hồi từ AI Host/Scene: retry theo cơ chế chung ở chương 07; nếu vẫn lỗi và thuộc luồng chính → cảnh báo Health Monitor, chờ Human Override.

## Extension Points

- Thêm rule mới không phải thêm module — rule là cấu hình (Workspace), Automation nạp rule như dữ liệu, không cần sửa code (chương 06).

## Related ADRs

- ADR-0002 — Ranh giới Automation vs AI Host.
- ADR-0004 — Event Bus bắt buộc.
- ADR-0007 — Live Session = Runtime Aggregate Root (Runtime State của Automation thuộc sở hữu Live Session).
