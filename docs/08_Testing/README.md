# 08_Testing

[CHỈ THAM CHIẾU]
Status: Frozen

Bản đồ chiến lược kiểm thử — mỗi loại test ứng với một tầng rủi ro khác nhau trong kiến trúc đã chốt.

## Danh sách
| File | Kiểm thử gì |
|---|---|
| UNIT.md | Logic riêng lẻ trong một module |
| INTEGRATION.md | Giao tiếp giữa các module qua Event Bus |
| UI.md | Giao diện Workspace/Dock |
| PERFORMANCE.md | Độ trễ, tải tài nguyên |
| RECOVERY.md | Khả năng phục hồi sau lỗi (chương 07) |
| STRESS.md | Hành vi khi quá tải (VD: comment tăng đột biến) |
| LONG_RUNNING.md | Phiên chạy liên tục nhiều giờ (Goal 2) |

## Nguyên tắc
1. Mỗi Failure Mode đã liệt kê ở 04_Modules/*/MODULE.md phải có ít nhất một test tương ứng ở RECOVERY.md hoặc STRESS.md — không được có Failure Mode nào không kiểm thử được.
2. Test không được kiểm tra chi tiết implementation — chỉ kiểm tra đúng Public Contract của module (04_Modules/_MODULE_TEMPLATE.md), để đổi implementation không phải viết lại test.

## Điều file này không định nghĩa
Không định nghĩa framework test cụ thể — chi tiết triển khai.
