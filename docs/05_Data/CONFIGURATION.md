# Configuration

[SOURCE OF TRUTH]
Status: Frozen

Dữ liệu cấu hình sống trong Workspace — độc lập với vòng đời bất kỳ Live Session nào, tái sử dụng giữa nhiều phiên (Mission 6, Philosophy #3, ADR-0007).

## Các loại Configuration

- Scene template (bố cục, overlay, tham chiếu Media) — sở hữu bởi module Scene.
- AI Host persona (giọng, phong cách, prompt, knowledge reference) — sở hữu bởi module AI Host.
- Automation rule (điều kiện trigger + hành động) — sở hữu bởi module Automation.
- Media asset (file + metadata) — sở hữu bởi module Media.
- Ngưỡng cảnh báo Health Monitor — sở hữu bởi module Health Monitor.
- Thông tin xác thực nền tảng (API key/token) — sở hữu bởi module Platform Adapter tương ứng, không module khác đọc trực tiếp (NON_FUNCTIONAL_SCOPE.md — Security).
- Cấu hình TTS provider, giọng đọc — sở hữu bởi module Voice.
- Cấu hình quy tắc phân loại comment — sở hữu bởi module Comment Engine.
- Cấu hình LLM provider/API key — sở hữu bởi module AI Host.

## Vòng đời

Configuration được tạo/sửa bởi người dùng qua Workspace, tồn tại độc lập với bất kỳ Live Session nào đang chạy hay không. Khi một Live Session khởi tạo (Preparing), nó **đọc một bản sao/tham chiếu** Configuration để instance hoá thành Runtime Data — không sửa trực tiếp Configuration gốc trong lúc phiên đang chạy (chương 02).

## Cách lưu ngược từ Runtime về Configuration

Nếu người dùng muốn giữ lại thay đổi phát sinh trong lúc live (VD: chỉnh sửa Automation rule ngay trong phiên), đây phải là **thao tác tường minh** do người dùng chủ động thực hiện — hệ thống không tự động đồng bộ ngược Runtime vào Configuration (chương 02, tránh phá Mission 6).

## Điều file này không định nghĩa

Không định nghĩa định dạng lưu trữ vật lý (file JSON, database...) — đó là quyết định triển khai.
