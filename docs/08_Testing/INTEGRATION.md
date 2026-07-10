# Integration Testing

[SOURCE OF TRUTH]
Status: Frozen

## Phạm vi
Kiểm thử giao tiếp thật giữa hai hoặc nhiều module qua Event Bus — xác nhận đúng sơ đồ Upstream/Downstream ở 04_Modules/README.md và 02_Architecture/chapters/04_data_flow.md.

## Nguyên tắc
1. Mỗi cặp Upstream/Downstream đã khai ở MODULE.md phải có ít nhất một test xác nhận event thật sự truyền đúng, đúng nội dung kỳ vọng.
2. Test phải xác nhận nguyên tắc "một event một nguồn phát" (chương 04) không bị vi phạm khi nhiều module cùng hoạt động.
3. Test phải bao gồm ít nhất một kịch bản thuộc UC1-UC4 (USE_CASES.md) chạy xuyên suốt nhiều module thật.

## Điều file này không định nghĩa
Không định nghĩa môi trường test cụ thể (staging, CI pipeline) — chi tiết triển khai.
