# Folder Rules

[SOURCE OF TRUTH]
Status: Frozen

## Nguyên tắc tổ chức thư mục source code
1. Cấu trúc thư mục source phải phản ánh đúng 10 module đã chốt ở 04_Modules — mỗi module một thư mục cấp cao, không gộp nhiều module vào một thư mục chung "misc" hay "utils" chứa logic nghiệp vụ.
2. UI Process và Engine Process (chương 05 — Runtime Model) phải tách thư mục rõ ràng ở cấp cao nhất — không trộn code hiển thị và code nghiệp vụ trong cùng thư mục.
3. Code dùng chung thật sự (không thuộc nghiệp vụ module nào, VD: kiểu dữ liệu Event chung, tiện ích) đặt ở một thư mục `shared/` riêng — nhưng `shared/` không được chứa logic nghiệp vụ, chỉ kiểu dữ liệu/tiện ích thuần.

## Điều file này không định nghĩa
Không định nghĩa cấu trúc thư mục đầy đủ dạng cây — sẽ dựng khi bắt đầu triển khai, dựa trên nguyên tắc ở đây.
