# 07_Development

[CHỈ THAM CHIẾU]
Status: Frozen

Bản đồ quy tắc phát triển code. Khác 04_Modules (mô tả kiến trúc), nhóm này mô tả cách viết code cụ thể — chỉ áp dụng khi bắt đầu triển khai, không ảnh hưởng các quyết định kiến trúc đã Frozen.

## Danh sách
| File | Nội dung |
|---|---|
| CODING_STANDARDS.md | Quy tắc viết code chung |
| NAMING_CONVENTIONS.md | Quy tắc đặt tên |
| FOLDER_RULES.md | Tổ chức thư mục source code |
| DEPENDENCY_RULES.md | Quy tắc phụ thuộc giữa các phần code |
| REVIEW_CHECKLIST.md | Checklist review code trước khi merge |

## Nguyên tắc
1. Mọi quy tắc ở đây phải phục vụ ranh giới đã chốt ở 02_Architecture và 04_Modules — không được mâu thuẫn (VD: FOLDER_RULES.md phải phản ánh đúng 10 module, không tự tổ chức khác).
2. Đây là nhóm duy nhất được phép nói về ngôn ngữ lập trình, framework cụ thể — các nhóm khác (00-06) giữ ở mức khái niệm.

## Điều file này không định nghĩa
Không định nghĩa kiến trúc (đã ở 02_Architecture), không định nghĩa module (đã ở 04_Modules).
