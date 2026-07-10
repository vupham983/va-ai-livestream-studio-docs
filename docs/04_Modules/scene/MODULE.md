# Scene — MODULE

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Quản lý cấu hình hiển thị/âm thanh (bố cục camera, overlay, tham chiếu Media) và kích hoạt cấu hình đó thành trạng thái hiển thị thực tế trong một phiên live.

## Public Contract

- Lưu một Scene mới hoặc chỉnh sửa Scene đã có trong Workspace.
- Kích hoạt (activate) một Scene đã cấu hình khi nhận lệnh hợp lệ từ Automation hoặc người vận hành.
- Cung cấp trạng thái hiển thị hiện tại (Scene nào đang active) cho Live Session qua Event Bus.
- Cho phép nhiều Scene được định nghĩa trong một Workspace và tái sử dụng ở nhiều Live Session khác nhau (Mission 6, Philosophy #3).

## Responsibilities

- Sở hữu định nghĩa cấu hình Scene (bố cục hiển thị, overlay, tham chiếu Media asset) ở lớp Workspace.
- Sở hữu instance Scene đang active trong một phiên live cụ thể.
- Chuyển đổi giữa các Scene khi nhận lệnh kích hoạt hợp lệ.

## Boundaries (không làm gì)

- Không tự quyết định khi nào kích hoạt một Scene — quyết định đó thuộc về Automation (theo rule) hoặc người vận hành (thao tác thủ công), Scene chỉ thực thi lệnh (chương 03).
- Không xử lý logic hiển thị của module khác (VD: không quyết định AI Host nói gì, không xử lý bình luận).
- Không tự tải hoặc quản lý vòng đời file Media — chỉ tham chiếu tới Media asset đã có (đó là trách nhiệm của module Media).

## Inputs

- Lệnh kích hoạt Scene (từ Automation hoặc từ người vận hành qua UI/IPC).
- Dữ liệu Scene template đã lưu ở Workspace (đọc lúc Preparing hoặc khi chuyển Scene).

## Outputs

- Sự kiện Scene đã kích hoạt / đổi Scene (emit ra Event Bus để Live Session và các module giám sát biết trạng thái hiển thị hiện tại).

## Upstream Dependencies

- Automation — phát lệnh kích hoạt Scene theo rule.
- Voice — cung cấp audio để Scene phát đồng thời với hiển thị
  (khớp 02_Architecture/chapters/04_data_flow.md: Voice --audio--> Scene).
- Media — cung cấp tài nguyên hình ảnh/video/âm thanh mà Scene tham chiếu khi hiển thị.

## Downstream Consumers

- Live Session — nhận sự kiện Scene active để cập nhật Runtime State của phiên (chương 02).
- Health Monitor, Analytics — tiêu thụ sự kiện Scene ở mức giám sát/thống kê, không thuộc luồng nghiệp vụ chính (04_Modules/README.md).

## Configuration State

- Scene template: bố cục hiển thị/âm thanh, overlay, danh sách tham chiếu Media asset. Thuộc sở hữu Workspace, tái sử dụng giữa nhiều Live Session — không thuộc sở hữu runtime của bất kỳ phiên nào (chương 02).

## Runtime State

- Scene instance đang active trong phiên hiện tại — thuộc sở hữu Live Session theo Aggregate Boundary (chương 02, ADR-0007). Mất khi phiên chuyển sang Ended, trừ khi cấu hình gốc đã được lưu riêng ở Workspace.

## Invariants

- Tại một thời điểm, một Live Session chỉ có đúng một Scene đang active.
- Kích hoạt Scene không được sửa trực tiếp Scene template gốc ở Workspace — mọi thay đổi phát sinh trong lúc live chỉ áp dụng cho instance runtime, trừ khi người dùng chủ động lưu ngược lại (chương 02).

## Failure Modes

- Scene template tham chiếu một Media asset không còn tồn tại (Non-critical — Scene có thể kích hoạt với placeholder/cảnh báo, không kéo sập phiên).
- Không nhận được xác nhận kích hoạt trong thời gian hợp lý (Critical nếu xảy ra ngay lúc chuyển Scene chính trong khi đang Live — theo phân loại chương 07).

## Recovery Strategy

- Media asset thiếu: cảnh báo Health Monitor, giữ Scene hiện tại, không tự động chuyển Scene khi chưa rõ nguyên nhân.
- Kích hoạt thất bại: retry một lần, nếu vẫn lỗi → cảnh báo Health Monitor, chờ Human Override quyết định chuyển Scene dự phòng hoặc giữ nguyên (theo chương 07 và UC3 ở USE_CASES.md).

## Extension Points

- Không có điểm mở rộng plugin riêng ở mức kiến trúc hiện tại — thêm loại overlay/bố cục mới là thay đổi cấu hình (Workspace), không phải mở rộng module.

## Related ADRs

- ADR-0001 — Live Session = state machine (Scene chỉ hoạt động trong ngữ cảnh trạng thái phiên hợp lệ).
- ADR-0004 — Event Bus bắt buộc.
- ADR-0007 — Live Session = Runtime Aggregate Root (xác định Scene instance thuộc sở hữu runtime của Live Session).
