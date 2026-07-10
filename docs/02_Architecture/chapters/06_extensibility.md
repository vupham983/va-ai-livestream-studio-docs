# 06 — Extensibility

[SOURCE OF TRUTH]
Status: Frozen

Cách hệ thống mở rộng module/nền tảng/AI Host mới mà không sửa lõi (Mission 4, Product Goals Goal 4).

## Nguyên tắc mở rộng

Mọi điểm mở rộng phải tuân theo một khuôn: thêm một thành phần mới = thêm một implementation theo interface đã định nghĩa, cắm vào đúng vị trí trong 3 vùng trách nhiệm (chương 00), không sửa Event Bus, không sửa Live Session.

## Mở rộng nền tảng livestream mới

Thêm một Platform Adapter mới:
1. Implement đúng interface Platform Adapter (nhận event chuẩn hoá, emit ra Event Bus theo đúng schema đã có ở 05_Data/EVENT.md).
2. Không được yêu cầu Comment Engine, Automation, hay bất kỳ module nghiệp vụ nào biết nền tảng nào đang chạy — module nghiệp vụ chỉ thấy event đã chuẩn hoá, không thấy sự khác biệt giữa TikTok hay Facebook.
3. Cấu hình xác thực (API key/token) của nền tảng mới lưu theo đúng mô hình Security đã có (NON_FUNCTIONAL_SCOPE.md), không tạo cơ chế lưu riêng.

## Mở rộng AI Host persona mới

Thêm một AI Host persona không phải là thêm module — persona là cấu hình (Workspace), không phải code. AI Host module load persona như dữ liệu, không cần sửa code khi thêm persona.

## Mở rộng module hoàn toàn mới (ngoài 10 module hiện có)

Nếu một domain nghiệp vụ mới không khớp với 10 module hiện có, thêm module mới phải:
1. Đi qua ADR trước (ARCHITECTURE_GOVERNANCE.md mục 4 — thêm/bớt module là trường hợp bắt buộc ADR).
2. Tuân thủ template ở 04_Modules/_MODULE_TEMPLATE.md.
3. Chỉ giao tiếp qua Event Bus như các module hiện có — không có ngoại lệ.

## Giới hạn của khả năng mở rộng

Không phải mọi thứ đều mở rộng được mà không đổi lõi — cụ thể, thay đổi mô hình runtime (chương 05) hoặc ranh giới Aggregate Root (chương 02) không phải là "mở rộng", mà là thay đổi kiến trúc, bắt buộc qua ADR Superseded, không thuộc phạm vi chương này.

## Điều chương này không định nghĩa

Không định nghĩa interface kỹ thuật cụ thể (tên method, tham số) — đó là chi tiết implementation, thuộc tài liệu code, không phải Architecture Bible.
