# OBS — INTEGRATION

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Cho phép hệ thống xuất luồng/tương tác với OBS Studio — tuỳ chọn, phục vụ người dùng muốn kết hợp VA AI Livestream Studio với thiết lập OBS đã có sẵn. Không bắt buộc trong MVP (MVP_DEFINITION.md). Đây là tích hợp với phần mềm OBS Studio thật chạy trên máy người dùng — khác với "mô hình OBS" nhắc ở ADR-0003/02_Architecture/chapters/05_runtime_model.md, vốn chỉ là kiến trúc UI/Engine tách tiến trình lấy cảm hứng từ cách OBS Studio tổ chức nội bộ, không liên quan tới tích hợp này.

## Auth Model

OBS WebSocket protocol — xác thực qua password cục bộ (nếu người dùng bật), không phải OAuth/API key như các nền tảng khác — đây là kết nối máy-tới-máy cục bộ, không qua internet.

## Rate Limits/Constraints

Không áp dụng theo nghĩa nền tảng đám mây — giới hạn thực tế là hiệu năng máy cục bộ (chương 08 — Platform Constraints).

## Failure Behavior

Mất kết nối OBS WebSocket không được ảnh hưởng luồng phát chính của hệ thống (nếu người dùng không phát qua OBS) — phân loại Non-critical theo nguyên tắc chung ở 02_Architecture/chapters/07_failure_recovery.md (lỗi một thành phần phụ trợ, không thuộc luồng vận hành chính), xử lý cụ thể theo 04_Modules/platform_adapter/MODULE.md.

Lưu ý: "OBS" ở đây là OBS Studio (phần mềm phát trực tiếp bên thứ ba), khác với khái niệm "Scene" trong hệ thống — không có quan hệ kế thừa hay tương đương giữa OBS Scene và Scene của VA AI Livestream Studio, dù cùng tên gọi.

## Data Exchanged

- Gửi: lệnh điều khiển Scene/nguồn hiển thị tới OBS (nếu tích hợp hai chiều).
- Nhận: trạng thái OBS hiện tại (đang phát/dừng).

## Related Module

04_Modules/platform_adapter/MODULE.md

## Related ADRs

Không có — mức độ tích hợp sâu tới đâu với OBS chờ quyết định khi có nhu cầu thực tế rõ ràng từ người dùng (Future Vision, không phải MVP).
