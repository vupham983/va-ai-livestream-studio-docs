# _MODULE_TEMPLATE

[SOURCE OF TRUTH]
Status: Frozen

Khuôn bắt buộc cho mọi MODULE.md. Không thêm/bớt mục — nếu một mục không áp dụng, ghi "Không áp dụng" kèm lý do ngắn, không xoá mục.

## Purpose

Một câu duy nhất: module này tồn tại để làm gì.

## Public Contract

Capability module này cung cấp cho hệ thống — hệ thống có thể "yêu cầu" module này làm gì. Không mô tả implementation, không mô tả event kỹ thuật (đó là Inputs/Outputs bên dưới). Đây là hợp đồng ổn định — implementation bên trong có thể đổi mà không phá vỡ mục này.

## Responsibilities

Trách nhiệm lõi module sở hữu, khớp đúng dòng "Sở hữu" của module này ở 02_Architecture/chapters/03_module_boundaries.md — không được mâu thuẫn hay mở rộng thêm so với bảng đó.

## Boundaries (không làm gì)

Trách nhiệm module KHÔNG được làm, khớp đúng dòng "Không được làm" ở chương 03 — có thể diễn giải chi tiết hơn nhưng không được nới lỏng ranh giới đã chốt.

## Inputs

Event/dữ liệu module này tiêu thụ (consume) từ Event Bus. Ghi tên event, không ghi schema chi tiết (schema thuộc 05_Data/EVENT.md).

## Outputs

Event/dữ liệu module này phát (emit) ra Event Bus.

## Upstream Dependencies

Module nào phát ra dữ liệu/event mà module này cần để hoạt động.

## Downstream Consumers

Module nào tiêu thụ dữ liệu/event mà module này phát ra.

(Cả hai mục trên chỉ liệt kê tên module — không mô tả cách gọi, vì không có gọi trực tiếp, chỉ qua Event Bus, ADR-0004.)

## Configuration State

Trạng thái cấu hình module này giữ — thuộc sở hữu Workspace, tái sử dụng giữa các phiên (chương 02).

## Runtime State

Trạng thái runtime module này giữ trong lúc một phiên đang chạy — thuộc sở hữu Live Session (Aggregate Root, chương 02), mất khi phiên Ended.

## Invariants

Những điều luôn đúng với module này, không phụ thuộc tình huống — "luật bất biến". Ngắn gọn, mỗi dòng một quy tắc, không diễn giải dài. Phải nhất quán với Boundaries ở trên, không mâu thuẫn.

## Failure Modes

Các cách module này có thể lỗi, mỗi lỗi phân loại Non-critical hay Critical theo đúng định nghĩa ở 02_Architecture/chapters/07_failure_recovery.md.

## Recovery Strategy

Với mỗi Failure Mode ở trên, chiến lược xử lý theo đúng chuỗi đã có ở chương 07: tự phục hồi → cảnh báo Health Monitor → Human Override (không tự phát minh chiến lược ngoài các bước hệ thống đã định nghĩa).

## Extension Points

Module này cho phép mở rộng ở đâu, theo đúng nguyên tắc ở 02_Architecture/chapters/06_extensibility.md. Nếu module không có điểm mở rộng nào, ghi "Không có" — không suy diễn thêm.

## Related ADRs

Danh sách ADR liên quan trực tiếp tới module này (nếu có).
