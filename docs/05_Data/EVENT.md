# Event

[SOURCE OF TRUTH]
Status: Frozen

Đơn vị giao tiếp duy nhất giữa các module, truyền qua Event Bus (ADR-0004). File này mô tả hình dạng và vòng đời của Event — luồng đi của nó (module nào gửi, module nào nhận) thuộc 02_Architecture/chapters/04_data_flow.md, không lặp ở đây.

## Hình dạng chung của một Event

Mỗi Event có tối thiểu:
- Loại sự kiện (event type) — xác định module nào subscribe.
- Nguồn phát (producer) — đúng một module, theo nguyên tắc "một event một nguồn phát" (chương 04).
- Payload — dữ liệu đi kèm, hình dạng cụ thể tuỳ loại event (không định nghĩa chi tiết ở tài liệu mức Bible này).
- Thời điểm phát sinh.

## Vòng đời

Một Event tồn tại ngắn — được phát ra, các module subscriber tiêu thụ gần như ngay lập tức, rồi hết vai trò. Event không phải là nơi lưu trữ lâu dài — nếu cần lưu vết một sự kiện đã xảy ra (cho Analytics hay Snapshot), module tiêu thụ phải tự trích xuất và lưu riêng, không dựa vào việc Event Bus giữ lại lịch sử.

## Nguyên tắc

1. Một loại Event chỉ có đúng một module được phép phát (single producer per event type, chương 04).
2. Nhiều module có thể cùng subscribe một Event.
3. Event không mang lệnh trực tiếp ép buộc — module nhận Event tự quyết định phản ứng theo trách nhiệm của mình (không có module nào "ra lệnh" cho module khác qua Event, chỉ thông báo sự kiện đã xảy ra hoặc yêu cầu).

## Phân loại chi tiết (ADR-0010)

"Event" ở file này là một trong 4 loại message trong taxonomy đầy đủ: Command, Event, Query, Stream (ADR-0010). 3 loại còn lại (Command, Query, Stream) tuân theo nguyên tắc chung của file này (routing, vòng đời) trừ các điểm khác biệt đã nêu ở ADR-0010/ADR-0011 (Command/Query point-to-point có phản hồi; Stream cho phép sample/drop). Danh mục đầy đủ từng message type cụ thể (tên, loại, producer/target) nằm ở 05_Data/MESSAGE_CATALOG.md (ADR-0010) — đây là Source of Truth duy nhất, không định nghĩa lại ở MODULE.md từng module.

## Điều file này không định nghĩa

Không liệt kê danh sách đầy đủ từng loại event cụ thể với schema chi tiết — danh sách đó nằm ở MESSAGE_CATALOG.md, không lặp lại ở đây.
