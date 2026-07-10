# Event

[SOURCE OF TRUTH]
Status: Approved

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

## Điều file này không định nghĩa

Không liệt kê danh sách đầy đủ từng loại event cụ thể với schema chi tiết — sẽ phát sinh dần trong quá trình triển khai, ghi nhận ở tài liệu code, không phải ở Architecture Bible.
