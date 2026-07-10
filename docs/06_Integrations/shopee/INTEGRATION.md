# Shopee — INTEGRATION

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Kết nối hệ thống với Shopee Live để nhận sự kiện (comment, lượt xem) và gửi luồng phát trực tiếp, đồng thời hỗ trợ ngữ cảnh bán hàng (giới thiệu sản phẩm) đặc thù nền tảng thương mại điện tử.

## Auth Model

Xác thực qua API/token cấp bởi Shopee cho nhà bán hàng/đối tác livestream — cơ chế cụ thể cần tra cứu tài liệu chính thức Shopee Live API trước khi triển khai.

## Rate Limits/Constraints

Chưa xác định — cần tra cứu tài liệu chính thức Shopee trước khi triển khai.

## Failure Behavior

Xử lý theo nguyên tắc chung ở 04_Modules/platform_adapter/MODULE.md.

## Data Exchanged

- Nhận: comment, lượt xem, có thể gồm sự kiện tương tác sản phẩm (nếu API hỗ trợ).
- Gửi: luồng hình ảnh/âm thanh đã ghép từ Scene.

## Related Module

04_Modules/platform_adapter/MODULE.md

## Related ADRs

Không có.
