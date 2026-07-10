# Non Functional Scope

[SOURCE OF TRUTH]
Status: Frozen

## Độ ổn định (Reliability)

Phiên live phải chạy liên tục nhiều giờ mà không cần khởi động lại ứng dụng trong điều kiện hoạt động bình thường (xem PRODUCT_GOALS.md Goal 2).

## Khả năng phục hồi (Recoverability)

Một lỗi cục bộ (một module lỗi, mất kết nối tạm thời) không được làm dừng toàn bộ phiên live đang chạy.

## Khả năng sẵn sàng (Availability)

Hệ thống phải duy trì khả năng hoạt động ngay cả khi một thành phần không thiết yếu bị lỗi — khác với Recoverability (phục hồi sau lỗi), Availability là duy trì hoạt động trong khi lỗi đang xảy ra ở một phần khác.

## Khả năng quan sát (Observability)

Người vận hành luôn biết trạng thái hiện tại của Live Session, hàng đợi xử lý, kết nối, và cảnh báo — thông qua giao diện, không cần xem log kỹ thuật.

## Độ trễ (Responsiveness)

Phản hồi của AI Host và Comment Engine trong lúc live phải đủ nhanh để không tạo cảm giác trễ so với nhịp trò chuyện tự nhiên. Ngưỡng số cụ thể sẽ được chốt bằng ADR khi có dữ liệu thực đo được, không đặt số tuỳ ý ở giai đoạn tài liệu này.

## Khả năng mở rộng (Extensibility)

Thêm một nền tảng livestream mới không được yêu cầu sửa lõi hệ thống (xem PRODUCT_GOALS.md Goal 4).

## Khả năng bảo trì lâu dài (Maintainability)

Kiến trúc phải cho phép mở rộng và bảo trì trong 10 năm mà không tạo nợ kỹ thuật không cần thiết (xem PHILOSOPHY.md #5).

## Khả năng vận hành bởi người không biết lập trình (Usability)

Toàn bộ chức năng trong FUNCTIONAL_SCOPE.md phải thao tác được qua giao diện, không yêu cầu người dùng chỉnh file cấu hình hay dùng dòng lệnh.

## Bảo mật dữ liệu vận hành (Security)

Thông tin xác thực kết nối tới các nền tảng livestream (API key, token) phải được lưu trữ cục bộ an toàn, không truyền ra ngoài ngoại trừ tới đúng nền tảng tương ứng.
