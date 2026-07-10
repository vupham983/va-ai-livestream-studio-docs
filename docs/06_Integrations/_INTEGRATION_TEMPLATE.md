# _INTEGRATION_TEMPLATE

[SOURCE OF TRUTH]
Status: Frozen

Khuôn bắt buộc cho mọi INTEGRATION.md. Không thêm/bớt mục.

## Purpose

Một câu: tích hợp này kết nối hệ thống với bên thứ ba nào, để làm gì.

## Auth Model

Cơ chế xác thực với bên thứ ba (API key, OAuth, token...). Không ghi giá trị thật, chỉ mô tả loại cơ chế.

## Rate Limits/Constraints

Giới hạn đã biết từ bên thứ ba (số request/phút, kích thước payload...). Nếu chưa biết cụ thể, ghi "Chưa xác định — cần tra cứu tài liệu chính thức của bên thứ ba trước khi triển khai", không đoán số.

## Failure Behavior

Bên thứ ba phản ứng thế nào khi lỗi (timeout, mã lỗi phổ biến), và module nào trong hệ thống xử lý (luôn là Platform Adapter hoặc Voice/tương ứng — không phải module nghiệp vụ khác).

## Data Exchanged

Loại dữ liệu trao đổi hai chiều (gửi gì, nhận gì) — mô tả ở mức khái niệm, không phải schema kỹ thuật chi tiết.

## Related Module

Module nào trong 04_Modules sở hữu tích hợp này (thường là Platform Adapter hoặc Voice).

## Related ADRs

ADR liên quan, nếu có.
