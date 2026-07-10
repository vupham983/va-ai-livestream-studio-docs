# Coding Standards

[SOURCE OF TRUTH]
Status: Frozen

Quy tắc chung khi viết code cho VA AI Livestream Studio.

## Nguyên tắc chung
1. Mỗi module (04_Modules) tương ứng một package/thư mục code riêng biệt — không trộn code của hai module vào cùng một file.
2. Giao tiếp giữa module chỉ qua Event Bus (ADR-0004) — code không được import trực tiếp logic nghiệp vụ của module khác, chỉ import kiểu dữ liệu Event dùng chung nếu cần.
3. Không viết logic "tạm thời" không có test — mọi logic xử lý Runtime Data (05_Data) phải có test tương ứng trước khi merge (xem 08_Testing).
4. Ưu tiên rõ ràng hơn ngắn gọn — hàm dài nhưng dễ hiểu tốt hơn hàm ngắn nhưng cần đọc kỹ mới hiểu (Principles #5).

## Ngôn ngữ/công nghệ cụ thể
Chưa chốt ở giai đoạn tài liệu này — quyết định ngôn ngữ, framework UI, thư viện Event Bus cụ thể sẽ ghi ADR riêng khi bắt đầu triển khai kỹ thuật.

## Điều file này không định nghĩa
Không định nghĩa style code chi tiết (indent, dấu ngoặc...) — dùng công cụ format tự động (linter) khi có, không ghi tay ở tài liệu.
