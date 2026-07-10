# ADR-0012 — Cơ chế thực thi single-producer (Producer Registry)

**Applies-to:** v0.x

Status: Accepted

## Context

ADR-0004 khoá nguyên tắc "một loại Event chỉ một producer" nhưng chưa định nghĩa cơ chế thực thi. Thực tế sẽ có nhiều Platform Adapter (TikTok, Shopee, Facebook...) cùng nhận comment thô từ nguồn khác nhau, dễ nhầm là "nhiều producer cho cùng một event type".

## Decision

Engine Process duy trì một Producer Registry tĩnh: bảng ánh xạ event_type → module sở hữu duy nhất, kiểm tra lúc khởi động (boot-time), không phải runtime. Nếu 2 module cùng khai báo là producer của một event type, Engine từ chối khởi động (fail-fast), không cảnh báo suông.

Với trường hợp nhiều Platform Adapter cùng nhận comment: mỗi adapter phát event RIÊNG có namespace theo nền tảng (VD: comment.tiktok.raw, comment.shopee.raw) — mỗi adapter là producer duy nhất của event namespace của chính nó. Comment Engine subscribe tất cả các event namespace này và tự phát ra MỘT event đã chuẩn hoá (VD: comment.normalized.received) mà Comment Engine là producer DUY NHẤT. Nguyên tắc single-producer áp dụng ở CẢ HAI tầng (raw theo adapter, và normalized ở Comment Engine) — không có ngoại lệ "producer ở mức nhóm/interface" cho phép nhiều module cùng phát một event type giống hệt nhau.

## Alternatives considered

- Khai producer ở "mức interface Platform Adapter" — bị loại vì đây chỉ là đổi tên gọi cho đỡ vi phạm, không giải quyết vấn đề gốc.
- Kiểm tra trùng producer lúc runtime thay vì boot-time — bị loại vì phát hiện lỗi quá trễ.

## Consequences

- Mỗi module khai báo producer của mình trong MODULE.md; Engine build Producer Registry từ các khai báo này lúc khởi động.
- Comment Engine trở thành lớp chuẩn hoá bắt buộc giữa nhiều Platform Adapter và phần còn lại của hệ thống.
- PENDING_DECISIONS.md mục "Cơ chế thực thi một event một nguồn phát" → "Draft — chờ review, xem ADR-0012".
