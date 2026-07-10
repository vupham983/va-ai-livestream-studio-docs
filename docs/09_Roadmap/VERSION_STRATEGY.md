# Version Strategy

[SOURCE OF TRUTH]
Status: Frozen

## Nguyên tắc
1. Version gắn với Milestone (MILESTONES.md), không gắn với ngày tháng.
2. Mỗi ADR có header `Applies-to: v0.x` (DOCUMENTATION_SYSTEM.md mục 6) — version thực tế đầu tiên bắt đầu từ v0.1 khi M1 (MVP) hoàn tất, không phải từ lúc bắt đầu code.
3. Thay đổi phá vỡ tương thích với ADR đã Frozen (VD: đổi mô hình runtime) chỉ được thực hiện ở version mới kèm ADR Superseded tương ứng.

## Điều file này không định nghĩa
Không định nghĩa cách đánh số chi tiết (semver đầy đủ) — chọn khi bắt đầu triển khai.
