# ADR-0004 — Event Bus bắt buộc

**Applies-to:** v0.x

Status: Accepted

## Context

Nếu các module gọi trực tiếp lẫn nhau qua function call, hệ thống sẽ hình thành phụ thuộc chặt và khó mở rộng/khó cô lập lỗi.

## Decision

Mọi module giao tiếp qua event, không gọi trực tiếp module khác qua function call. Định nghĩa nằm ở `05_Data/EVENT.md` + `QUEUE.md`, viết trước khi bất kỳ `MODULE.md` nào được viết nội dung thật.

## Alternatives considered

Cho phép module gọi trực tiếp function của nhau trong một số trường hợp 'đơn giản' — bị loại vì phá vỡ nguyên tắc tách rời, tạo tiền lệ phụ thuộc chặt.

## Consequences

`05_Data/EVENT.md` và `QUEUE.md` phải được viết trước khi viết nội dung thật cho bất kỳ `MODULE.md` nào.
