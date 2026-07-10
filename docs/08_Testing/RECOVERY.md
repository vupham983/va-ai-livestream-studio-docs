# Recovery Testing

[SOURCE OF TRUTH]
Status: Frozen

## Phạm vi
Kiểm thử khả năng phục hồi theo đúng phân loại Non-critical/Critical ở 02_Architecture/chapters/07_failure_recovery.md.

## Nguyên tắc
1. Mỗi Failure Mode + Recovery Strategy đã khai ở từng 04_Modules/*/MODULE.md phải có một test giả lập đúng lỗi đó xảy ra, xác nhận Recovery Strategy hoạt động như mô tả.
2. Phải có test kill cứng THẬT cho mọi cơ chế liên quan tới Snapshot/autosave — spawn tiến trình con thật và gọi os._exit() (hoặc tương đương ở ngôn ngữ triển khai) đúng vào thời điểm rủi ro cao nhất (giữa lúc ghi file tạm, trước khi rename/commit), sau đó xác nhận dữ liệu gốc (bản trước đó) vẫn nguyên vẹn, đọc được, checksum đúng. Test mô phỏng bằng logic thuần (VD: giả lập file bị cắt cụt, checksum reject) KHÔNG được coi là bằng chứng tương đương — đây là hai loại rủi ro khác nhau (mô phỏng ở tầng ứng dụng vs crash thật ở tầng hệ điều hành/tiến trình). Không được tick Definition of Done cho task Recovery nếu chỉ có test mô phỏng.
3. Phải có test riêng cho trường hợp Health Monitor tự treo (health_monitor/MODULE.md — Failure Modes), vì đây là điểm mù kiến trúc đã biết trước.

## Điều file này không định nghĩa
Không định nghĩa cơ chế giả lập lỗi cụ thể (chaos engineering tool...) — chi tiết triển khai.

---
Cập nhật 2026-07-10, làm rõ tiêu chuẩn "kill cứng thật" — không đảo ngược quyết định, chỉ tăng độ cụ thể của yêu cầu test đã có.
