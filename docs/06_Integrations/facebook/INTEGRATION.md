# Facebook — INTEGRATION

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Kết nối hệ thống với Facebook Live để nhận sự kiện (comment, reaction, lượt xem) và gửi luồng phát trực tiếp.

## Auth Model

Xác thực qua Facebook Graph API (OAuth token cho Page/tài khoản livestream) — cơ chế cụ thể cần tra cứu tài liệu chính thức Facebook Live API trước khi triển khai.

## Rate Limits/Constraints

Chưa xác định — cần tra cứu tài liệu chính thức Facebook trước khi triển khai.

## Failure Behavior

Xử lý theo nguyên tắc chung ở 04_Modules/platform_adapter/MODULE.md.

## Data Exchanged

- Nhận: comment, reaction, lượt xem.
- Gửi: luồng hình ảnh/âm thanh đã ghép từ Scene.

## Related Module

04_Modules/platform_adapter/MODULE.md

## Related ADRs

Không có.
