# YouTube — INTEGRATION

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Kết nối hệ thống với YouTube Live để nhận sự kiện (comment, super chat, lượt xem) và gửi luồng phát trực tiếp.

## Auth Model

Xác thực qua YouTube Data API/Live Streaming API (OAuth) — cơ chế cụ thể cần tra cứu tài liệu chính thức trước khi triển khai.

## Rate Limits/Constraints

Chưa xác định — cần tra cứu tài liệu chính thức YouTube trước khi triển khai.

## Failure Behavior

Xử lý theo nguyên tắc chung ở 04_Modules/platform_adapter/MODULE.md.

## Data Exchanged

- Nhận: comment, super chat, lượt xem.
- Gửi: luồng hình ảnh/âm thanh đã ghép từ Scene.

## Related Module

04_Modules/platform_adapter/MODULE.md

## Related ADRs

Không có.
