# Platform Adapter — MODULE

[SOURCE OF TRUTH]
Status: Approved

## Purpose

Dịch giao thức/dữ liệu/sự kiện giữa hệ thống và một nền tảng livestream cụ thể (TikTok, Facebook, YouTube, Shopee...) — cả chiều nhận vào lẫn chiều gửi ra (GLOSSARY.md — Platform Adapter; chương 00).

## Public Contract

- Nhận sự kiện/dữ liệu thô từ một nền tảng bên ngoài (comment, lượt xem) và chuẩn hoá thành sự kiện nội bộ.
- Nhận đầu ra hiển thị/âm thanh đã ghép từ Scene và gửi (broadcast) lên đúng nền tảng tương ứng.
- Báo cáo tình trạng kết nối theo thời gian thực cho Health Monitor.
- Không quyết định hành vi hệ thống — chỉ là lớp chuyển đổi giữa hệ thống và nền tảng.

## Responsibilities

- Sở hữu việc giao tiếp hai chiều với đúng một nền tảng: nhận inbound (comment, sự kiện) và gửi outbound (luồng phát đã ghép từ Scene).
- Sở hữu và bảo vệ thông tin xác thực (API key/token) của đúng nền tảng mình phụ trách.
- Chuẩn hoá dữ liệu inbound thành định dạng nội bộ trước khi phát ra Event Bus cho Comment Engine xử lý tiếp.

## Boundaries (không làm gì)

- Không quyết định hành vi hệ thống (chương 00) — chỉ dịch giao thức/dữ liệu/sự kiện, không chứa logic nghiệp vụ.
- Không tự phân loại hay diễn giải nội dung comment/sự kiện — việc đó thuộc Comment Engine.
- Không tự quyết định khi nào bắt đầu/dừng gửi luồng phát — chỉ thực thi theo trạng thái Live Session yêu cầu (qua Event Bus).

## Inputs

- Sự kiện/dữ liệu thô từ nền tảng bên ngoài (comment, lượt xem, trạng thái kết nối).
- Đầu ra hiển thị/âm thanh đã ghép từ Scene, cần gửi (outbound/broadcast) lên nền tảng.

## Outputs

- Sự kiện đã chuẩn hoá từ dữ liệu inbound (comment, sự kiện) — emit ra Event Bus cho Comment Engine.
- Sự kiện tình trạng kết nối — emit ra Event Bus cho Health Monitor.
- Luồng phát thực tế gửi lên nền tảng bên ngoài (outbound, không qua Event Bus — đây là giao tiếp trực tiếp với bên ngoài hệ thống).

## Upstream Dependencies

- External Platforms (TikTok/Facebook/YouTube/Shopee...) — nguồn sự kiện inbound (comment, lượt xem, trạng thái kết nối).
- Scene — nguồn đầu ra hiển thị/âm thanh đã ghép, cần gửi (outbound) lên nền tảng. Platform Adapter là module duy nhất trong 10 module vừa nhận inbound từ bên ngoài vừa gửi outbound ra bên ngoài — không module nào khác có cả hai chiều này.

## Downstream Consumers

- Comment Engine — nhận sự kiện inbound thô đã chuẩn hoá để phân loại tiếp.
- Health Monitor — nhận tình trạng kết nối (đã xác nhận ở health_monitor/MODULE.md, Upstream Dependencies).
- Analytics — nhận sự kiện để thống kê (lượt xem, tương tác theo nền tảng).

## Configuration State

- Thông tin xác thực (API key/token) và cấu hình kết nối của đúng một nền tảng — thuộc sở hữu Workspace, tái sử dụng giữa nhiều Live Session.

## Runtime State

Thuộc sở hữu Live Session theo Aggregate Boundary (chương 02, ADR-0007) — mất khi phiên Ended:
- Trạng thái kết nối hiện tại tới nền tảng (đã kết nối/đang thử lại/mất kết nối) trong phiên đang chạy.

## Invariants

- Chỉ Platform Adapter tương ứng được truy cập thông tin xác thực (API key/token) của đúng nền tảng đó — module khác không đọc trực tiếp (Trust Boundary, chương 01; Security, NON_FUNCTIONAL_SCOPE.md).
- Một Platform Adapter mất kết nối không được ảnh hưởng tới các Platform Adapter khác đang chạy song song trong cùng phiên.
- Platform Adapter không phát dữ liệu inbound thô, chưa chuẩn hoá, ra Event Bus.

## Failure Modes

- Mất kết nối tới một nền tảng (Non-critical nếu các nền tảng khác vẫn chạy song song bình thường; Critical nếu đây là nền tảng duy nhất đang phát — theo chương 07).
- Lỗi xác thực (token hết hạn) — Critical đối với chính nền tảng đó, không ảnh hưởng các Platform Adapter khác.

## Recovery Strategy

- Mất kết nối: tự động thử kết nối lại theo cơ chế chung ở chương 07; nếu không khôi phục được trong thời gian hợp lý, cảnh báo Health Monitor và chờ Human Override quyết định (dừng nền tảng đó hoặc tiếp tục các nền tảng khác).
- Lỗi xác thực: cảnh báo ngay tới người vận hành qua Health Monitor, không tự thử refresh token nếu chưa có cơ chế xác nhận — đây là quyết định triển khai cụ thể, ghi ADR riêng khi bắt đầu 06_Integrations.

## Extension Points

- Điểm mở rộng rõ nhất trong toàn hệ thống: thêm một nền tảng livestream mới = thêm một Platform Adapter mới, implement đúng interface đã định nghĩa, không sửa lõi hệ thống (chương 06). Module nghiệp vụ khác (Comment Engine, Automation...) không cần biết nền tảng nào đang chạy.

## Related ADRs

- ADR-0004 — Event Bus bắt buộc.
- ADR-0007 — Live Session = Runtime Aggregate Root (Runtime State của Platform Adapter thuộc sở hữu Live Session).
