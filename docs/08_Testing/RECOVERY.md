# Recovery Testing

[SOURCE OF TRUTH]
Status: Frozen

## Phạm vi
Kiểm thử khả năng phục hồi theo đúng phân loại Non-critical/Critical ở 02_Architecture/chapters/07_failure_recovery.md.

## Nguyên tắc
1. Mỗi Failure Mode + Recovery Strategy đã khai ở từng 04_Modules/*/MODULE.md phải có một test giả lập đúng lỗi đó xảy ra, xác nhận Recovery Strategy hoạt động như mô tả.
2. Phải có test giả lập Engine Process crash, xác nhận Workspace không mất (chương 07 — nguyên tắc "một crash không được làm mất Workspace").
3. Phải có test riêng cho trường hợp Health Monitor tự treo (health_monitor/MODULE.md — Failure Modes), vì đây là điểm mù kiến trúc đã biết trước.

## Điều file này không định nghĩa
Không định nghĩa cơ chế giả lập lỗi cụ thể (chaos engineering tool...) — chi tiết triển khai.
