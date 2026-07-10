# Performance Testing

[SOURCE OF TRUTH]
Status: Frozen

## Phạm vi
Đo độ trễ và tải tài nguyên, đối chiếu NON_FUNCTIONAL_SCOPE.md — Responsiveness.

## Nguyên tắc
1. Ngưỡng độ trễ cụ thể chưa được chốt ở tài liệu Product (NON_FUNCTIONAL_SCOPE.md ghi rõ "chờ ADR khi có dữ liệu thực đo được") — Performance Testing là nơi thu thập dữ liệu đó lần đầu, sau đó đề xuất ADR chốt ngưỡng chính thức.
2. Test phải chạy trong điều kiện giới hạn tài nguyên máy người dùng thực tế (chương 08 — Platform Constraints), không chỉ trên máy phát triển mạnh.

## Điều file này không định nghĩa
Không tự đặt ngưỡng số cụ thể — đó là kết quả của quá trình test, không phải giả định trước.
