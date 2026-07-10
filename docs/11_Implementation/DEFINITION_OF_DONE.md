# Definition of Done

[SOURCE OF TRUTH]
Status: Frozen

## Một task được coi là Done khi
1. Code khớp đúng Public Contract/Boundaries của module tương ứng (04_Modules/*/MODULE.md).
2. Có test tương ứng theo đúng phân loại ở 08_Testing (Unit tối thiểu; Integration nếu task liên quan giao tiếp giữa module).
3. Đã qua checklist ở 07_Development/REVIEW_CHECKLIST.md, không có mục nào chưa tick.
4. Không vi phạm bất kỳ Invariant nào trong MODULE.md liên quan.
5. Nếu task chạm tới một mục trong PENDING_DECISIONS.md, mục đó phải đã Resolved bằng ADR trước khi merge.

## Điều file này không định nghĩa
Không định nghĩa công cụ CI/CD cụ thể chạy các bước kiểm tra này.
