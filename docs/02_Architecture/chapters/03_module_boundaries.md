# 03 — Module Boundaries

[SOURCE OF TRUTH]
Status: Frozen

Chương này chỉ định ranh giới trách nhiệm giữa 10 module — không
mô tả chi tiết implementation (thuộc 04_Modules/*/MODULE.md).

## Nguyên tắc phân ranh giới

1. Mỗi module sở hữu đúng một trách nhiệm lõi, không chồng lấn
   module khác (PRINCIPLES.md #4 — Reuse thắng Duplicate, áp dụng
   cả cho ranh giới trách nhiệm).
2. Module nghiệp vụ giao tiếp qua Event Bus (ADR-0004), không gọi
   trực tiếp lẫn nhau.
3. Một module chỉ sở hữu state runtime nằm trong phạm vi Live
   Session (chương 02) — không module nào giữ state runtime "mồ côi"
   ngoài vòng đời một phiên.

## Bảng ranh giới 10 module

| Module | Sở hữu | Không được làm |
|---|---|---|
| Live Session | Vòng đời phiên, aggregate root | Không tự sinh nội dung, không chứa logic nghiệp vụ cụ thể của module khác |
| Scene | Cấu hình + instance hiển thị/âm thanh | Không tự quyết định khi nào kích hoạt (do Automation/người dùng ra lệnh) |
| AI Host | Sinh nội dung không xác định trước | Không tự trigger hành động ngoài phạm vi được gọi (ADR quyết định #2) |
| Automation | Rule/trigger xác định trước | Không tự sinh nội dung tự do — chỉ được gọi AI Host như một action |
| Comment Engine | Tiếp nhận, phân loại bình luận | Không tự quyết định phản hồi phức tạp — việc đó là của AI Host khi được Automation/rule gọi |
| Voice | Chuyển văn bản → giọng nói | Không sinh nội dung, không quyết định nói gì |
| Media | Tài nguyên hình ảnh/video/âm thanh | Không xử lý logic hiển thị (đó là Scene) |
| Analytics | Thu thập, tổng hợp số liệu | Không can thiệp vào luồng vận hành đang chạy |
| Health Monitor | Giám sát kết nối nền tảng + tài nguyên máy (ADR quyết định #5) | Không tự động sửa lỗi — chỉ cảnh báo, hành động sửa thuộc Automation/người dùng |
| Platform Adapter | Dịch giao thức/dữ liệu/sự kiện với bên thứ ba | Không quyết định hành vi hệ thống (chương 00) |

## Ranh giới đặc biệt: Automation vs AI Host

Đây là ranh giới dễ nhầm lẫn nhất (đã ghi ADR riêng): Automation
chạy rule xác định trước, AI Host sinh nội dung không xác định
trước. Khi một rule Automation cần nội dung linh hoạt, nó gọi
AI Host như một hành động trong rule — không tự chứa logic sinh
nội dung.

## Điều chương này không định nghĩa

Không mô tả input/output chi tiết, event cụ thể module phát/nhận
(04_Modules/*/MODULE.md), không mô tả cơ chế Event Bus kỹ thuật
(chương 04).
