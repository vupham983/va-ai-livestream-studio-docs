# ADR-0008 — Mở rộng _MODULE_TEMPLATE.md từ 10 mục lên 15 mục

Status: Accepted

## Context

_MODULE_TEMPLATE.md ban đầu (DOCUMENTATION_SYSTEM.md mục 3, Sprint 0)
được viết trước khi có Architecture Bible, gồm 10 mục cơ bản. Sau
khi hoàn tất 02_Architecture (9 chương), phát sinh nhu cầu mô tả
rõ hơn: hợp đồng ổn định của module (Public Contract), hướng phụ
thuộc rõ ràng (Upstream/Downstream thay vì Dependencies chung),
phân biệt state cấu hình và runtime (khớp ranh giới Aggregate ở
chương 02), quy tắc bất biến (Invariants), và chiến lược phục hồi
gắn với phân loại lỗi đã có ở chương 07.

## Decision

_MODULE_TEMPLATE.md dùng cấu trúc 15 mục: Purpose, Public Contract,
Responsibilities, Boundaries, Inputs, Outputs, Upstream Dependencies,
Downstream Consumers, Configuration State, Runtime State, Invariants,
Failure Modes, Recovery Strategy, Extension Points, Related ADRs.

"Events emitted/consumed" của bản cũ không bị loại bỏ — được tách
rõ vào Inputs (tiêu thụ) và Outputs (phát ra).

## Alternatives considered

- Giữ nguyên 10 mục — loại bỏ vì thiếu chỗ mô tả hợp đồng ổn định
  và chiến lược phục hồi, hai thứ quan trọng cho một hệ thống sống
  10 năm.

## Consequences

- DOCUMENTATION_SYSTEM.md mục 3 phải cập nhật để khớp cấu trúc mới,
  tránh hai nguồn mô tả template mâu thuẫn nhau.
- Toàn bộ 10 MODULE.md viết sau ADR này dùng đúng 15 mục, không có
  ngoại lệ.
