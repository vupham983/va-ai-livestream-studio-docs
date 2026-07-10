# 06_Integrations

[CHỈ THAM CHIẾU]
Status: Frozen

Bản đồ các tích hợp bên thứ ba. Mỗi tích hợp mô tả đặc tính của bên thứ ba đó — không mô tả logic nghiệp vụ (đó là 04_Modules/platform_adapter hoặc 04_Modules/voice).

## Danh sách

| Tích hợp | Loại | Module sở hữu |
|---|---|---|
| obs | Streaming output (tuỳ chọn, không bắt buộc MVP) | Platform Adapter |
| tiktok | Nền tảng livestream | Platform Adapter |
| facebook | Nền tảng livestream | Platform Adapter |
| youtube | Nền tảng livestream | Platform Adapter |
| shopee | Nền tảng livestream | Platform Adapter |
| tts | Text-to-Speech | Voice |
| llm | Sinh nội dung | AI Host |

## Nguyên tắc

1. Mỗi INTEGRATION.md không được chứa logic nghiệp vụ — chỉ mô tả đặc tính bên thứ ba (Auth, Rate Limit, Failure Behavior), theo đúng _INTEGRATION_TEMPLATE.md.
2. Module nghiệp vụ (Comment Engine, Automation...) không được biết chi tiết tích hợp cụ thể nào đang chạy — chỉ Platform Adapter/Voice/AI Host tương ứng mới đọc file tích hợp này (chương 06 — Extensibility).
3. MVP chỉ cần một nền tảng livestream đầu tiên (MVP_DEFINITION.md) — các tích hợp còn lại có khung sẵn, nội dung đầy đủ sẽ hoàn thiện dần khi triển khai từng nền tảng.
4. Khi một tích hợp không nhận được phản hồi trong thời gian hợp lệ hoặc gặp lỗi kết nối, xử lý theo phân loại Non-critical/Critical đã có ở 02_Architecture/chapters/07_failure_recovery.md — không tự định nghĩa phân loại lỗi riêng ở 06_Integrations.

## Điều file này không định nghĩa

Không quyết định nền tảng nào được ưu tiên tích hợp trước — quyết định đó chờ ADR riêng khi bắt đầu triển khai (đã nêu ở MVP_DEFINITION.md).
