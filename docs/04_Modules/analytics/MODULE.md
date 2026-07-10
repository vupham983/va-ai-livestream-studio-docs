# Analytics — MODULE

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Thu thập và tổng hợp số liệu vận hành của Live Session (lượt xem, tương tác, hiệu suất bán hàng...) cho người vận hành xem lại (GLOSSARY.md — Analytics).

## Public Contract

- Thu thập số liệu vận hành từ các sự kiện phát sinh trong phiên đang chạy.
- Tổng hợp số liệu thành báo cáo/chỉ số hiển thị được cho người vận hành.
- Không can thiệp hay phản hồi lại bất kỳ sự kiện nào — chỉ quan sát và ghi nhận.

## Responsibilities

- Sở hữu logic thu thập và tổng hợp số liệu vận hành của phiên.
- Cung cấp số liệu đã tổng hợp cho UI hiển thị tới người vận hành.

## Boundaries (không làm gì)

- Không can thiệp vào luồng vận hành đang chạy (chương 03) — thuần read-only, không emit lệnh hành động nào ảnh hưởng module khác.
- Không quyết định hay đề xuất hành động — chỉ ghi nhận số liệu, việc diễn giải/hành động thuộc về người vận hành.

## Inputs

- Sự kiện từ nhiều module khác qua Event Bus (Comment Engine, Scene, AI Host, Automation, Platform Adapter...) — subscribe diện rộng, chỉ đọc (read-only subscriber), không phải một nguồn cụ thể như các module trước.

## Outputs

- Số liệu/báo cáo đã tổng hợp — cung cấp cho UI hiển thị. Không emit sự kiện điều khiển nào ra Event Bus.

## Upstream Dependencies

- Comment Engine, Scene, AI Host, Automation, Platform Adapter (và các module khác phát sinh sự kiện đáng ghi nhận) — Analytics subscribe diện rộng qua Event Bus, chỉ đọc, không phải phụ thuộc vào một nguồn cụ thể.

## Downstream Consumers

- Không có module nghiệp vụ nào — chỉ người vận hành xem số liệu qua UI (khác 7 module trước, không có downstream là module khác trong Event Bus).

## Configuration State

- Cấu hình chỉ số/báo cáo muốn theo dõi (loại số liệu, cách tổng hợp hiển thị) — thuộc sở hữu Workspace, tái sử dụng giữa nhiều Live Session.

## Runtime State

Thuộc sở hữu Live Session theo Aggregate Boundary (chương 02, ADR-0007) — mất khi phiên Ended (trừ phần đã xuất thành báo cáo lưu trữ, thuộc 05_Data):
- Số liệu đang được tích luỹ trong phiên hiện tại (lượt xem, tương tác... tính tới thời điểm hiện tại).

## Invariants

- Analytics không được phép làm chậm hoặc chặn luồng chính khi thu thập số liệu — không xử lý đồng bộ (blocking) với module đang xử lý nghiệp vụ.
- Analytics không bao giờ emit lệnh hành động ảnh hưởng tới Live Session hay module khác.

## Failure Modes

- Mất một phần số liệu do quá tải/lỗi thu thập — Non-critical tuyệt đối (Analytics lỗi không bao giờ ảnh hưởng luồng chính, khớp PRODUCT_GOALS.md Goal 2 và chương 07 — Availability).

## Recovery Strategy

- Mất số liệu: ghi nhận khoảng trống (gap) trong báo cáo, cảnh báo Health Monitor ở mức thông tin nếu cần, không retry theo cách có thể ảnh hưởng luồng chính, không chặn hay làm chậm bất kỳ module nào khác.

## Extension Points

- Thêm loại chỉ số/báo cáo mới không phải thêm module — là thay đổi cấu hình (Workspace), theo chương 06.

## Related ADRs

- ADR-0004 — Event Bus bắt buộc.
- ADR-0007 — Live Session = Runtime Aggregate Root (Runtime State của Analytics thuộc sở hữu Live Session, phần đã tổng hợp/lưu trữ thuộc 05_Data).
