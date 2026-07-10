# TTS — INTEGRATION

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Cung cấp dịch vụ chuyển văn bản thành giọng nói cho module Voice — có thể nhiều provider (Edge, XTTS, ElevenLabs...), Voice không lộ chi tiết provider nào đang chạy ra module khác (04_Modules/voice/MODULE.md, 04_Modules/ai_host/MODULE.md).

## Auth Model

Tuỳ provider: một số miễn phí không cần auth (Edge TTS), một số cần API key trả phí (ElevenLabs). Mỗi provider cụ thể có Auth Model riêng, quản lý bởi module Voice, không lộ ra module khác.

## Rate Limits/Constraints

Tuỳ provider — cần tra cứu tài liệu chính thức từng provider cụ thể trước khi triển khai, không đoán số chung cho tất cả.

## Failure Behavior

Provider lỗi/timeout xử lý theo 04_Modules/voice/MODULE.md — Failure Modes/Recovery Strategy (có thể chuyển provider dự phòng nếu đã cấu hình).

## Data Exchanged

- Gửi: văn bản cần chuyển giọng nói.
- Nhận: audio đã tổng hợp.

## Related Module

04_Modules/voice/MODULE.md

## Related ADRs

Không có — chọn provider mặc định cho MVP chờ quyết định khi triển khai.
