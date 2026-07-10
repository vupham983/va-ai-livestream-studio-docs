# Live Session — MODULE

[SOURCE OF TRUTH]
Status: Approved

## Purpose

Điều phối vòng đời một phiên livestream và làm ranh giới sở hữu
(Aggregate Root) cho toàn bộ trạng thái runtime phát sinh trong
phiên đó.

## Public Contract

- Khởi tạo một phiên mới từ cấu hình Workspace đã chọn.
- Chuyển trạng thái phiên (Preparing → Live → Paused → Ended) khi
  nhận yêu cầu hợp lệ.
- Cung cấp trạng thái hiện tại của phiên cho bất kỳ module nào cần
  biết (qua Event Bus, không qua truy vấn trực tiếp).
- Chấp nhận yêu cầu Human Override để chuyển trạng thái ngay lập tức,
  bất kể module nào đang ở giữa hoạt động.

## Responsibilities

- Sở hữu và thực thi state machine của phiên (chương 02).
- Sở hữu Aggregate Boundary — quyết định thực thể runtime nào
  thuộc về phiên hiện tại.
- Là điểm duy nhất được phép thay đổi trạng thái phiên.

## Boundaries (không làm gì)

- Không tự sinh nội dung (đó là AI Host).
- Không chứa logic nghiệp vụ cụ thể của bất kỳ module nào khác.
- Không đọc/ghi trực tiếp cấu hình Workspace — chỉ đọc một lần lúc
  Preparing để instance hoá runtime.

## Inputs

- Yêu cầu khởi tạo phiên (từ UI, qua IPC → Event Bus).
- Yêu cầu chuyển trạng thái (Pause/Resume/End) từ bất kỳ module nào
  hoặc từ Human Override.
- Sự kiện lỗi Critical từ các module khác (để chuyển sang
  Error/Recovering).

## Outputs

- Sự kiện thay đổi trạng thái phiên (phát mỗi khi state machine
  chuyển bước) — mọi module khác subscribe event này để hành xử
  theo trạng thái hiện tại.

## Upstream Dependencies

- Không có — Live Session là gốc của luồng runtime, không phụ
  thuộc dữ liệu đầu vào từ module nghiệp vụ nào khác để tồn tại.

## Downstream Consumers

- Toàn bộ 9 module còn lại — tất cả đều subscribe sự kiện trạng
  thái phiên để biết khi nào được hoạt động, khi nào phải dừng.

## Configuration State

- Không áp dụng — Live Session không có Configuration State theo
  nghĩa Workspace; nó chỉ đọc cấu hình từ các module khác (Scene,
  AI Host...) lúc Preparing, không tự sở hữu cấu hình.

## Runtime State

- Trạng thái hiện tại (Idle/Preparing/Live/Paused/Error/Ended).
- Danh sách tham chiếu tới các instance runtime thuộc phiên (Scene
  đang active, AI Host instance, Automation state, Comment queue,
  Health status) — xem Ranh giới sở hữu ở chương 02.
- Thời điểm bắt đầu/kết thúc phiên.

## Invariants

- Không module nào khác được phép tự ý đổi trạng thái phiên bằng
  cách khác ngoài gửi yêu cầu qua Event Bus.
- Một máy tại một thời điểm chỉ có đúng một Live Session ở trạng
  thái Live hoặc Paused (không chạy song song hai phiên Live trên
  cùng instance ứng dụng ở MVP — xem MVP_DEFINITION.md, đa phiên
  song song thuộc Future Vision).
- Khi phiên chuyển sang Ended, toàn bộ Runtime State phải được xử
  lý (lưu trữ hoặc giải phóng) trước khi cho phép khởi tạo phiên mới.

## Failure Modes

- Không nhận được xác nhận khởi tạo từ một module runtime trong
  bước Preparing (Non-critical nếu module đó không thiết yếu,
  Critical nếu là module luồng chính — phân loại theo chương 07).
- Toàn bộ Engine Process crash giữa lúc Live (Critical).

## Recovery Strategy

- Module runtime không phản hồi lúc Preparing: retry nạp cấu hình
  một lần, nếu vẫn lỗi → cảnh báo Health Monitor, chặn chuyển sang
  Live cho tới khi người vận hành xác nhận qua Human Override.
- Engine Process crash: xử lý theo nguyên tắc chương 07 (không mất
  Workspace, khôi phục runtime theo ADR autosave/recovery sắp tới).

## Extension Points

- Không có — vòng đời state machine là lõi cố định, không phải
  điểm cắm plugin (khác với AI Host hay Platform Adapter).

## Related ADRs

- ADR-0001 — Live Session = state machine.
- ADR-0003 — Runtime multi-process.
- ADR-0004 — Event Bus bắt buộc.
- ADR-0007 — Live Session = Runtime Aggregate Root.
