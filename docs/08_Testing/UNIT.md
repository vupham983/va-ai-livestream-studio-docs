# Unit Testing

[SOURCE OF TRUTH]
Status: Frozen

## Phạm vi
Kiểm thử logic riêng lẻ bên trong một module, không qua Event Bus, không phụ thuộc module khác.

## Nguyên tắc
1. Mỗi Public Contract trong MODULE.md phải có ít nhất một unit test xác nhận đúng hành vi mô tả.
2. Mỗi Invariant trong MODULE.md phải có test xác nhận invariant đó không bị vi phạm trong các tình huống thông thường.
3. Unit test không được gọi thật ra bên ngoài (Platform Adapter, LLM, TTS) — dùng giả lập (mock/stub) cho mọi tích hợp bên thứ ba (06_Integrations).

## Điều file này không định nghĩa
Không định nghĩa công cụ mock cụ thể — chi tiết triển khai.
