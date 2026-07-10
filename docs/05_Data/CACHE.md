# Cache

[SOURCE OF TRUTH]
Status: Approved

Dữ liệu lưu tạm để tối ưu hiệu năng — không bao giờ là nguồn sự thật duy nhất, luôn có thể xây dựng lại từ State hoặc Configuration (05_Data/README.md — Nguyên tắc phân loại #3).

## Ví dụ dữ liệu có thể là Cache

- Bản mirror State ở UI Process (chương 05 — Runtime Model): UI không giữ bản sao độc lập, chỉ cache tạm bản phản chiếu nhận qua IPC.
- Kết quả phân loại comment đã xử lý gần đây (Comment Engine có thể cache để tránh xử lý lại comment giống hệt lặp lại liên tục).

## Nguyên tắc

1. Mất Cache không bao giờ được gây lỗi nghiêm trọng (Critical) — nếu một thành phần phụ thuộc Cache tới mức mất Cache gây lỗi nặng, dữ liệu đó thực chất là State, không phải Cache, và phải được xếp lại đúng loại.
2. Cache không có cam kết về thời gian sống — module sở hữu Cache có quyền xoá bất kỳ lúc nào mà không cần thông báo.
3. Cache không được đồng bộ ngược lại thành Configuration hay State — chiều đồng bộ luôn một chiều: State/Configuration → Cache, không bao giờ ngược lại.

## Điều file này không định nghĩa

Không định nghĩa chính sách eviction (LRU, TTL...) cụ thể — chi tiết triển khai.
