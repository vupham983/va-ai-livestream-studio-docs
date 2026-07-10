# 05_Data

[CHỈ THAM CHIẾU]
Status: Approved

Bản đồ các loại dữ liệu trong hệ thống — vòng đời, ai ghi, ai đọc, có persist hay không. Đây là SoT về vòng đời dữ liệu, để 04_Modules không phải lặp lại mô tả state ở từng module (DOCUMENTATION_SYSTEM.md mục 3).

## Danh sách

| File | Loại dữ liệu | Vòng đời |
|---|---|---|
| CONFIGURATION.md | Dữ liệu cấu hình (Workspace) | Tồn tại độc lập với phiên, tái sử dụng |
| RUNTIME_DATA.md | Dữ liệu runtime tổng quát | Gắn với vòng đời Live Session |
| STATE.md | Trạng thái hiện tại của phiên | Sống trong Engine Process, mirror qua IPC tới UI |
| EVENT.md | Sự kiện lưu thông qua Event Bus | Tồn tại ngắn, tiêu thụ xong thì hết vai trò (trừ khi được log) |
| QUEUE.md | Hàng đợi xử lý | Tồn tại trong phạm vi một phiên |
| SNAPSHOT.md | Bản chụp trạng thái để phục hồi | Persist qua đĩa, tồn tại sau khi phiên Ended |
| CACHE.md | Dữ liệu tạm để tối ưu hiệu năng | Không đảm bảo tồn tại, có thể mất bất kỳ lúc nào |

## Nguyên tắc phân loại

1. Mọi dữ liệu đều thuộc đúng một trong hai nhóm sở hữu đã chốt ở 02_Architecture/chapters/02_core_domain_live_session.md: **Configuration** (Workspace) hoặc **Runtime** (Live Session Aggregate, ADR-0007). File nào cũng phải nêu rõ nó thuộc nhóm nào.
2. Không file nào trong 05_Data được mô tả lại luồng dữ liệu ở mức hệ thống (module nào gửi cho module nào) — đó là 02_Architecture/chapters/04_data_flow.md. 05_Data chỉ mô tả hình dạng và vòng đời của dữ liệu, không mô tả luồng.
3. Cache không bao giờ là nguồn sự thật duy nhất (Single Source of Truth) cho bất kỳ dữ liệu nào — luôn có thể được xây dựng lại từ State hoặc Configuration.

## Điều file này không định nghĩa

Không định nghĩa schema cụ thể theo ngôn ngữ lập trình (kiểu dữ liệu, class) — đó là chi tiết triển khai. 05_Data mô tả ở mức khái niệm, đủ để mọi module tham chiếu nhất quán.
