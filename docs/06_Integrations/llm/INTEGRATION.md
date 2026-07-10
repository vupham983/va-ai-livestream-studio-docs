# LLM — INTEGRATION

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Cung cấp khả năng sinh nội dung không xác định trước cho module AI Host — có thể nhiều provider (OpenAI, DeepSeek, Gemini...), AI Host không lộ chi tiết provider nào đang chạy ra module khác.

## Auth Model

API key theo từng provider LLM — quản lý bởi module AI Host, không lộ ra module khác (NON_FUNCTIONAL_SCOPE.md — Security).

## Rate Limits/Constraints

Tuỳ provider — cần tra cứu tài liệu chính thức từng provider trước khi triển khai, không đoán số.

## Failure Behavior

Provider lỗi/timeout xử lý theo 04_Modules/ai_host/MODULE.md — Failure Modes (Lỗi kỹ thuật)/Recovery Strategy.

## Data Exchanged

- Gửi: ngữ cảnh hội thoại + yêu cầu sinh nội dung.
- Nhận: nội dung văn bản đã sinh.

## Related Module

04_Modules/ai_host/MODULE.md

## Related ADRs

Không có — chọn provider mặc định cho MVP chờ quyết định khi triển khai.
