# 02_Architecture — Architecture Bible

[CHỈ THAM CHIẾU]
Status: Frozen

Đây là mục lục và quy tắc vận hành của Architecture Bible. Bản
thân file này không chứa quyết định kiến trúc — quyết định nằm ở
từng chương trong `chapters/`.

## Mục lục

| # | Chương | Trả lời câu hỏi |
|---|---|---|
| 00 | overview.md | Kiến trúc tổng thể trông như thế nào, đọc theo thứ tự nào |
| 01 | system_context.md | Hệ thống đứng ở đâu trong bối cảnh (máy người dùng, nền tảng bên ngoài, không phải cloud service) |
| 02 | core_domain_live_session.md | Live Session là gì về mặt kiến trúc — state machine + aggregate root (ADR-0001, ADR-0007) |
| 03 | module_boundaries.md | Ranh giới trách nhiệm giữa 10 module, ai không được làm gì |
| 04 | data_flow.md | Luồng dữ liệu ở mức hệ thống — module nào gửi gì cho module nào qua kênh nào |
| 05 | runtime_model.md | Mô hình tiến trình (multi-process, ADR liên quan) |
| 06 | extensibility.md | Thêm module/nền tảng/AI Host mới bằng cách nào mà không sửa lõi |
| 07 | failure_recovery.md | Hệ thống phản ứng thế nào khi một phần bị lỗi |
| 08 | platform_constraints.md | Giới hạn từ việc chạy trên desktop, không phải server |

## Quy tắc tham chiếu giữa chương

1. Một chương chỉ được tham chiếu chương có số thứ tự nhỏ hơn
   hoặc bằng chính nó — không phụ thuộc ngược (VD: chương 02
   không được giả định trước nội dung chương 05).
2. Thuật ngữ dùng trong mọi chương phải khớp 00_Project/GLOSSARY.md.
   Nếu một chương cần một khái niệm chưa có trong Glossary, phải bổ
   sung vào Glossary trước (qua ADR nếu Glossary đã Frozen), không
   tự định nghĩa lại trong chương.
3. Một chương không được mô tả lại nội dung đã có ở 04_Modules,
   05_Data, hay 03_UI_UX — chỉ dẫn chiếu. Cụ thể:
   - Chi tiết trách nhiệm từng module → 04_Modules/*/MODULE.md,
     không lặp ở chương 03.
   - Hình dạng/vòng đời dữ liệu → 05_Data/*, không lặp ở chương 04
     (chương 04 chỉ vẽ sơ đồ luồng ở mức hệ thống).
   - Bố cục giao diện → 03_UI_UX/*, không xuất hiện trong
     Architecture Bible.
4. Mọi quyết định trong các chương phải có ADR tương ứng trong
   10_ADR/. Một chương không được tự đưa ra quyết định mới mà
   không có ADR đi kèm — chương trình bày quyết định đã có ADR,
   không tạo quyết định tại chỗ.
5. Khi một chương mâu thuẫn với ADR đã Accepted, ADR thắng; sửa
   lại chương, không sửa ADR (trừ khi mở ADR Superseded mới theo
   ARCHITECTURE_GOVERNANCE.md).

## Trạng thái các chương

Toàn bộ 9 chương hiện ở dạng khung mục lục (heading rỗng), theo
đúng scope giai đoạn hiện tại: chưa viết nội dung kiến trúc. Nội
dung thật sẽ được viết theo thứ tự 00 → 08 ở sprint kế tiếp, sau
khi 02_Architecture/README.md này được Approve.
