# Review Checklist

[SOURCE OF TRUTH]
Status: Frozen

Checklist review code trước khi merge.

## Checklist
- [ ] Code có khớp đúng Responsibility/Boundaries của module tương ứng ở 04_Modules/*/MODULE.md không?
- [ ] Có import trực tiếp module khác thay vì qua Event Bus không (vi phạm DEPENDENCY_RULES.md)?
- [ ] Có test tương ứng chưa (xem 08_Testing)?
- [ ] Có Invariant nào ở MODULE.md bị vi phạm bởi đoạn code này không?
- [ ] Nếu code này thay đổi hành vi đã có ADR, có ADR mới ghi nhận thay đổi đó không (ARCHITECTURE_GOVERNANCE.md mục 4)?

## Điều file này không định nghĩa
Không định nghĩa công cụ CI/CD cụ thể chạy checklist này — chi tiết triển khai.
