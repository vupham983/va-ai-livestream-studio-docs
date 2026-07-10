# Media — MODULE

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Quản lý vòng đời tài nguyên hình ảnh/video/âm thanh (Media asset) dùng trong Scene và AI Host, từ lúc người dùng nạp vào Workspace tới khi được tham chiếu trong phiên live (GLOSSARY.md — Media).

## Public Contract

- Lưu trữ Media asset (file + metadata) người dùng nạp vào Workspace.
- Cung cấp tham chiếu tới Media asset cho module cần dùng (Scene, AI Host), kèm xác nhận asset đó còn tồn tại/hợp lệ hay không.
- Không tự quyết định asset nào được dùng ở đâu — chỉ trả lời "asset này có sẵn sàng không" khi được hỏi.

## Responsibilities

- Sở hữu toàn bộ Media asset (file hình ảnh/video/âm thanh) và metadata liên quan (tên, định dạng, kích thước, đường dẫn lưu trữ).
- Kiểm tra và xác nhận tính tồn tại/hợp lệ của một Media asset khi được module khác tham chiếu.
- Quản lý vòng đời lưu trữ (thêm mới, cập nhật, xoá) của Media asset trong Workspace.

## Boundaries (không làm gì)

- Không xử lý logic hiển thị — việc hiển thị/phát Media thuộc về Scene (chương 03).
- Không tự quyết định khi nào một Media asset được dùng trong phiên — chỉ cung cấp tham chiếu khi được hỏi.
- Không xử lý hậu quả runtime khi một Media asset bị thiếu lúc đang live (VD: Scene không kích hoạt được vì thiếu asset) — đó là trách nhiệm xử lý runtime của Scene (đã khai ở scene/MODULE.md, Failure Modes/Recovery Strategy); Media chỉ chịu trách nhiệm phát hiện và báo asset không hợp lệ.

## Inputs

- Media asset (file) và metadata do người dùng nạp trực tiếp qua Workspace.
- Yêu cầu tham chiếu/kiểm tra tồn tại một Media asset (từ Scene hoặc AI Host).

## Outputs

- Kết quả tham chiếu Media asset (đường dẫn/dữ liệu asset nếu hợp lệ, hoặc cảnh báo asset không tồn tại/hỏng nếu không hợp lệ).

## Upstream Dependencies

- Không có — Media asset được người dùng nạp trực tiếp qua Workspace, không nhận dữ liệu từ module nghiệp vụ nào khác qua Event Bus (khác các module trước).

## Downstream Consumers

- Scene — tham chiếu Media khi hiển thị (đã xác nhận ở scene/MODULE.md, Upstream Dependencies).
- AI Host — tham chiếu Media nếu cần cho ngữ cảnh, theo Knowledge Reference đã khai ở ai_host/MODULE.md, Configuration State.

## Configuration State

- Toàn bộ Media asset (file hình ảnh/video/âm thanh) và metadata (tên, định dạng, kích thước, đường dẫn) — thuộc sở hữu Workspace hoàn toàn, tái sử dụng giữa nhiều Live Session.

## Runtime State

- Không đáng kể — Media gần như thuần Workspace, không giữ trạng thái runtime riêng gắn với một phiên đang chạy. Việc "đang dùng asset nào trong phiên" thuộc Runtime State của Scene, không phải của Media.

## Invariants

- Media không tự ý xoá hoặc thay đổi asset đang được một Scene/AI Host tham chiếu trong phiên đang Live mà không có xác nhận.
- Media chỉ trả lời đúng trạng thái tồn tại/hợp lệ của asset khi được hỏi — không tự phát sự kiện chủ động khi không có yêu cầu tham chiếu.

## Failure Modes

- Media asset bị thiếu hoặc hỏng khi có module khác tham chiếu (Non-critical ở phía Media — Media chỉ phát hiện và báo cáo; hậu quả runtime cụ thể, VD: Scene không kích hoạt được, đã được xử lý ở scene/MODULE.md để tránh mô tả trùng lặp).

## Recovery Strategy

- Phát hiện asset thiếu/hỏng: trả về cảnh báo cho module đã yêu cầu tham chiếu (Scene/AI Host) và cảnh báo Health Monitor; không tự thay thế hay tự phục hồi file — quyết định xử lý runtime (dùng placeholder, đổi Scene...) thuộc module tham chiếu, theo chương 07.

## Extension Points

- Thêm định dạng Media asset mới (loại file, codec) là thay đổi cấu hình/khả năng lưu trữ (Workspace), không phải mở rộng module theo nghĩa plugin (chương 06).

## Related ADRs

- ADR-0004 — Event Bus bắt buộc (áp dụng cho phần yêu cầu/phản hồi tham chiếu qua Event Bus, dù Media không có Upstream nghiệp vụ).
