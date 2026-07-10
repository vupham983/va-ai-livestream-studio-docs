# Coding Workflow

[SOURCE OF TRUTH]
Status: Frozen

Quy trình làm việc hằng ngày khi triển khai code — áp dụng cùng mô hình Draft → Review → Approved → Frozen đã dùng cho tài liệu (ARCHITECTURE_GOVERNANCE.md), nhưng ở cấp độ code.

## Quy trình
1. Mỗi task (TASK_BREAKDOWN.md) tương ứng một nhánh git riêng, đặt tên theo module + task.
2. Trước khi mở Pull Request, tự chạy checklist DEFINITION_OF_DONE.md.
3. Review dựa trên 07_Development/REVIEW_CHECKLIST.md — không merge nếu có mục chưa tick.
4. Nếu code phát hiện một mâu thuẫn hoặc khoảng trống mới trong Bible (giống 2 vòng audit đã làm), dừng lại, ghi nhận thành vấn đề, không tự ý code theo phỏng đoán — quay lại sửa tài liệu trước (hoặc mở ADR nếu là quyết định kiến trúc).
5. Không merge trực tiếp vào nhánh chính nếu chưa qua ít nhất một review độc lập.

## Điều file này không định nghĩa
Không chọn công cụ CI/CD, git hosting cụ thể — chi tiết triển khai.
