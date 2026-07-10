# TikTok — INTEGRATION

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Kết nối hệ thống với TikTok Live để nhận sự kiện (comment, lượt xem) và gửi luồng phát trực tiếp.

## Auth Model

Xác thực qua API key/token cấp bởi TikTok cho nhà phát triển/đối tác — cơ chế cụ thể (OAuth hay API key tĩnh) chưa xác định, cần tra cứu tài liệu chính thức TikTok Live API trước khi triển khai.

## Rate Limits/Constraints

Chưa xác định — cần tra cứu tài liệu chính thức TikTok trước khi triển khai, không đoán số.

## Failure Behavior

Mất kết nối hoặc lỗi xác thực được Platform Adapter (instance phụ trách TikTok) xử lý theo nguyên tắc chung ở 04_Modules/platform_adapter/MODULE.md — Failure Modes/Recovery Strategy. Không có module nghiệp vụ nào khác xử lý lỗi kết nối TikTok trực tiếp.

## Data Exchanged

- Nhận: comment, lượt xem, trạng thái phòng live.
- Gửi: luồng hình ảnh/âm thanh đã ghép từ Scene.

## Related Module

04_Modules/platform_adapter/MODULE.md

## Related ADRs

Không có — quyết định kỹ thuật cụ thể (SDK, giao thức stream) chờ ADR riêng khi triển khai.
