# 11_Implementation

[CHỈ THAM CHIẾU]
Status: Approved

Kế hoạch triển khai kỹ thuật chi tiết — khác TECHNICAL_ROADMAP.md (09_Roadmap, chỉ nêu thứ tự lớn theo module), nhóm này chia nhỏ thành sprint/task cụ thể, dùng để bắt đầu code thật.

## Danh sách
| File | Nội dung |
|---|---|
| IMPLEMENTATION_ORDER.md | Thứ tự sprint chi tiết, mỗi sprint gắn với module/PENDING_DECISIONS liên quan |
| TASK_BREAKDOWN.md | Chia nhỏ từng sprint thành task cụ thể |
| DEFINITION_OF_DONE.md | Tiêu chí một task được coi là hoàn thành |
| CODING_WORKFLOW.md | Quy trình làm việc hằng ngày (nhánh git, review, merge) |

## Nguyên tắc
1. Không sprint nào được bắt đầu nếu các mục liên quan trong 10_ADR/PENDING_DECISIONS.md thuộc phạm vi sprint đó chưa được resolve bằng ADR (ARCHITECTURE_GOVERNANCE.md).
2. Thứ tự ở đây phải nhất quán với 09_Roadmap/TECHNICAL_ROADMAP.md — không tự sắp xếp lại thứ tự module.

## Điều nhóm này không định nghĩa
Không viết code, không chọn ngôn ngữ/framework cụ thể — đó là 07_Development khi đã có ADR chọn công nghệ.
