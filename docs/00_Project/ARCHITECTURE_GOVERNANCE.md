# Architecture Governance

[SOURCE OF TRUTH]

Quy định quyền quyết định, quy trình review, và cách xử lý
thay đổi kiến trúc cho toàn bộ dự án VA AI Livestream Studio.

## 1. Vai trò

- **Approver (Người quyết định cuối cùng):** Chủ dự án (Pham).
  Mọi tài liệu chỉ chuyển trạng thái Approved/Frozen khi Approver
  xác nhận.
- **Architect (Người soạn thảo, đề xuất, phản biện):** Claude,
  chủ trì xuyên suốt dự án — giữ ngữ cảnh toàn bộ quyết định đã
  chốt, đề xuất hướng đi, chỉ ra trade-off, viết ADR.
- **External Input (Phản biện bên ngoài):** Mọi ý kiến từ nguồn
  khác (công cụ AI khác, cố vấn, v.v.) được xem là **input tham
  khảo**, đi qua Approver để quyết có áp dụng hay không. External
  Input KHÔNG có quyền trực tiếp sửa, duyệt, hay đóng băng tài liệu.
  Lý do: nhiều nguồn có quyền kiến trúc song song sẽ phá vỡ
  nguyên tắc Single Source of Truth ở cấp quy trình, không chỉ
  cấp nội dung.

## 2. Vòng đời tài liệu

`Draft → Review → Approved → Frozen → Superseded`

- **Draft:** đang soạn nội dung thật (sau khi đã có skeleton).
- **Review:** Architect trình bản Draft, Approver đọc và phản biện.
- **Approved:** Approver xác nhận đạt yêu cầu.
- **Frozen:** không được sửa trực tiếp nữa. Mọi thay đổi phải
  đi qua ADR mới.
- **Superseded:** bị thay thế bởi quyết định mới hơn (ghi rõ
  ADR nào thay thế).

## 3. Mô hình Sprint

Tài liệu được phát triển theo từng thư mục cấp 1 (00 → 10),
theo đúng thứ tự ở DOCUMENTATION_SYSTEM.md mục 5. Mỗi sprint:

1. Architect soạn nội dung Draft cho toàn bộ file trong thư mục đó.
2. Approver Review, có thể yêu cầu sửa nhiều vòng.
3. Khi Approved → Frozen cả thư mục.
4. Chỉ sau khi Frozen mới mở sprint kế tiếp.

Không mở song song nhiều sprint để tránh quyết định ở sprint sau
làm mâu thuẫn ngầm với sprint trước chưa kịp phát hiện.

## 4. Khi nào bắt buộc tạo ADR

- Thay đổi bất kỳ file đã Frozen.
- Thêm/bớt module trong 04_Modules.
- Thay đổi ranh giới trách nhiệm giữa hai module.
- Thay đổi mô hình runtime, data flow, hoặc Event Bus.
- Đảo ngược bất kỳ quyết định nào trong DOCUMENTATION_SYSTEM.md
  mục 6.

Lưu ý phân biệt: thay đổi nội dung Product (Personas, Goals,
Journeys, Use Cases...) do nghiên cứu người dùng hoặc quyết định
kinh doanh KHÔNG bắt buộc ADR — ADR chỉ bắt buộc cho các trường
hợp liệt kê ở trên. Thay đổi Product đã Frozen vẫn cần Approver
duyệt lại và ghi rõ lý do thay đổi, nhưng không cần theo format ADR.

## 5. Definition of Done cho một tài liệu

Một file được coi là Done khi:
- Không mâu thuẫn với GLOSSARY.md.
- Không trùng lặp nội dung đã có ở file SoT khác.
- Đúng vai trò đã định nghĩa ở DOCUMENTATION_SYSTEM.md mục 3.
- Được Approver Approve.

## 6. Xử lý mâu thuẫn

Khi hai tài liệu hoặc hai ý kiến mâu thuẫn nhau:
PRINCIPLES.md (00_Project) là trọng tài cấp cao nhất. Nếu
PRINCIPLES.md chưa đủ để phân xử, Approver quyết định và
việc quyết định đó phải được ghi lại thành ADR.

## 7. Ngoại lệ thứ tự Sprint (ghi nhận 2026-07-10)

03_UI_UX được hoãn sau 06_Integrations và 07-09, khác thứ tự gốc
ở DOCUMENTATION_SYSTEM.md mục 5. Lý do: 03_UI_UX cần quyết định
thiết kế cụ thể (design tokens, layout thật) chưa sẵn sàng tại
thời điểm này, trong khi 07-09 viết được ở mức nguyên tắc thuần
tuý dựa trên Architecture đã Frozen, không phụ thuộc UI. 03_UI_UX
vẫn ở dạng skeleton, sẽ hoàn thiện ở sprint riêng sau đó.

## Trạng thái phiên bản tài liệu

Trạng thái tổng thể: v1.0 Candidate — đã hoàn tất Sprint 1-9 (00_Project
đến 09_Roadmap, trừ 03_UI_UX hoãn có chủ đích), chờ một vòng Cold
Review trước khi đổi thành v1.0 Final.

Cold Review: để bộ tài liệu "nghỉ" vài ngày làm việc, không chỉnh
sửa trong thời gian đó, sau đó đọc lại toàn bộ như người chưa từng
tham gia dự án. Nếu không phát hiện vấn đề lớn, đổi trạng thái
thành v1.0 Final.

Đề xuất chưa xử lý, dành cho v1.1 trở đi: Decision Log
(docs/11_Decisions/) — ghi lịch sử các đề xuất/quyết định không
nhất thiết thành ADR (ngày, ai đề xuất, đã review chưa, có thành
ADR không, đã huỷ chưa). Chưa tạo thư mục này ở v1.0, vì đây là
thay đổi cấu trúc tài liệu, không nên chèn vội vào cuối một sprint.
