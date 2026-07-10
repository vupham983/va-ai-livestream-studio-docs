# Comment Engine — MODULE

[SOURCE OF TRUTH]
Status: Approved

## Purpose

Tiếp nhận, phân loại, và chuẩn hoá bình luận/tin nhắn từ khán giả trong lúc live (GLOSSARY.md — Comment Engine), đồng thời là điểm gác đầu tiên của Trust Boundary — nơi dữ liệu không tin cậy từ bên ngoài được lọc trước khi đi tiếp vào hệ thống.

## Public Contract

- Tiếp nhận comment/tin nhắn thô từ Platform Adapter.
- Phân loại comment (câu hỏi thường gặp, câu hỏi phức tạp, spam/nhiễu...) và chuẩn hoá thành sự kiện nội bộ.
- Chuyển câu hỏi/sự kiện đã phân loại cho Automation để rule quyết định hành động tiếp theo.

## Responsibilities

- Sở hữu logic phân loại và chuẩn hoá comment/tin nhắn.
- Là điểm duy nhất trong hệ thống chịu trách nhiệm lọc dữ liệu không tin cậy từ nền tảng trước khi phát vào Event Bus cho module khác tiêu thụ (Trust Boundary, chương 01).
- Phát sự kiện đã chuẩn hoá, không phát dữ liệu thô chưa qua xử lý.

## Boundaries (không làm gì)

- Không tự quyết định phản hồi phức tạp — chỉ phân loại và chuẩn hoá; việc sinh nội dung trả lời thuộc AI Host, chỉ được gọi khi Automation/rule quyết định (chương 03).
- Không tự gọi AI Host trực tiếp — mọi kích hoạt nội dung linh hoạt phải đi qua Automation trước (nhất quán với automation/MODULE.md, ai_host/MODULE.md).
- Không kiểm tra logic nghiệp vụ của module khác — chỉ đảm bảo dữ liệu đưa vào Event Bus đã được chuẩn hoá và đáng tin ở mức cấu trúc.

## Inputs

- Comment/tin nhắn thô từ Platform Adapter (định dạng đã chuẩn hoá cấp giao thức bởi Adapter, nhưng chưa được phân loại nội dung).

## Outputs

- Sự kiện comment đã phân loại/chuẩn hoá (câu hỏi thường gặp, câu hỏi phức tạp, spam/nhiễu...) — emit ra Event Bus.

## Upstream Dependencies

- Platform Adapter — nguồn comment/sự kiện thô từ nền tảng.

## Downstream Consumers

- Automation — nhận câu hỏi/sự kiện đã phân loại để rule xử lý tiếp (không có Downstream trực tiếp tới AI Host — mọi nội dung linh hoạt phải qua Automation trước, theo đúng ranh giới đã duyệt ở automation/MODULE.md và ai_host/MODULE.md).

## Configuration State

- Cấu hình quy tắc phân loại (danh sách từ khoá/câu hỏi thường gặp, ngưỡng nhận diện spam...) — thuộc sở hữu Workspace, tái sử dụng giữa nhiều Live Session.

## Runtime State

Thuộc sở hữu Live Session theo Aggregate Boundary (chương 02, ADR-0007) — mất khi phiên Ended:
- Hàng đợi comment đang chờ xử lý trong phiên hiện tại.
- Số liệu tạm thời về khối lượng comment đang xử lý (phục vụ phát hiện quá tải).

## Invariants

- Mọi comment/tin nhắn từ Platform Adapter phải đi qua Comment Engine trước khi bất kỳ module nào khác (đặc biệt AI Host, Automation) được xử lý (Trust Boundary, chương 01) — module downstream không tự kiểm tra lại độ tin cậy dữ liệu.
- Comment Engine không phát dữ liệu thô chưa phân loại ra Event Bus.

## Failure Modes

- Không phân loại được một câu hỏi (Non-critical) — có thể đẩy nguyên văn cho Automation xử lý theo rule mặc định.
- Quá tải khi lượng comment tăng đột biến (phân loại Non-critical/Critical theo chương 07, tuỳ mức độ ảnh hưởng tới khả năng phản hồi chung của phiên).

## Recovery Strategy

- Không phân loại được: đẩy nguyên văn kèm nhãn "chưa phân loại" cho Automation, để rule mặc định xử lý (không chặn luồng xử lý).
- Quá tải: cảnh báo Health Monitor; nếu vượt ngưỡng ảnh hưởng luồng chính, áp dụng chiến lược phục hồi chung ở chương 07 (bao gồm khả năng chờ Human Override can thiệp nếu cần).

## Extension Points

- Thêm quy tắc phân loại mới (từ khoá, mẫu câu hỏi) không phải thêm module — là thay đổi cấu hình (Workspace), theo chương 06.

## Related ADRs

- ADR-0004 — Event Bus bắt buộc.
- ADR-0007 — Live Session = Runtime Aggregate Root (Runtime State của Comment Engine thuộc sở hữu Live Session).
