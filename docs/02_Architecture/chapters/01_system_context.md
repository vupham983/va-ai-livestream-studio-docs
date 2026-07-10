# 01 — System Context

[SOURCE OF TRUTH]
Status: Frozen

## Hệ thống đứng ở đâu

VA AI Livestream Studio chạy như một ứng dụng desktop trên máy của người vận hành — không phải một dịch vụ cloud nhiều người dùng chung. Toàn bộ Workspace và Live Session runtime sống trên máy đó (xem ADR-0003 — multi-process, chi tiết ở chương 05).

Hệ thống gọi ra ngoài tới các dịch vụ đám mây khi cần (LLM, TTS, Platform API), nhưng không phụ thuộc một backend trung tâm do Anthropic/VA vận hành để chạy được một phiên live cơ bản.

## Các nhóm bên ngoài hệ thống giao tiếp

1. Nền tảng livestream — TikTok, Facebook, YouTube, Shopee. Giao tiếp một chiều: hệ thống gửi luồng phát và nhận sự kiện (comment, lượt xem) qua Platform Adapter tương ứng.
2. Dịch vụ AI bên thứ ba — LLM (sinh nội dung AI Host), TTS (chuyển văn bản thành giọng nói). Đây là dịch vụ được gọi theo yêu cầu (request-response), không giữ trạng thái phiên live.
3. Người vận hành — tương tác qua UI (Workspace lúc chuẩn bị, Dock giám sát lúc đang live). Đây là nhóm duy nhất có quyền Human Override.
4. Hệ điều hành/tài nguyên máy — CPU, RAM, mạng cục bộ. Health Monitor giám sát nhóm này (xem ADR-0005).

## Ranh giới tin cậy (Trust Boundary)

- Dữ liệu từ nền tảng livestream (comment, sự kiện) được coi là đầu vào không tin cậy — phải qua Comment Engine phân loại trước khi bất kỳ module nào (đặc biệt AI Host, Automation) xử lý.
- Thông tin xác thực (API key, token) tới từng nền tảng chỉ được Platform Adapter tương ứng truy cập — không module nghiệp vụ nào khác cần hoặc được phép đọc trực tiếp (xem NON_FUNCTIONAL_SCOPE.md — Security).
- Trust Boundary áp dụng cho nội dung do khán giả tạo ra (comment, tin nhắn) — đi qua Comment Engine. Telemetry kết nối (trạng thái online/offline của nền tảng) không thuộc phạm vi này — Health Monitor nhận trực tiếp từ Platform Adapter vì đây là tín hiệu hạ tầng, không phải nội dung do người dùng tạo ra.

## Điều chương này không định nghĩa

Chương này không mô tả cấu trúc nội bộ của Live Session (chương 02), không liệt kê chi tiết từng module (chương 03), và không mô tả cách Platform Adapter hoạt động bên trong (thuộc 04_Modules/platform_adapter và 06_Integrations).
