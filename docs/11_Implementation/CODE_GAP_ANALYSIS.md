# Code Gap Analysis — VA AI Livestream Studio

[CHỈ THAM CHIẾU]
Status: Draft

Bản đồ di cư từ code thật (D:\ai-livestream-studio) sang kiến trúc mục tiêu trong Documentation Bible. Cập nhật sau mỗi sprint migrate, không phải tài liệu tĩnh.

## Chiến lược chung: Strangler Fig, không rewrite

Không xoá code cũ viết lại từ đầu. Từng module bọc dần qua Event Bus (Wrap), sau đó refactor nội bộ theo Bible (Refactor), rồi thay thế phần còn thiếu (Replace) — code cũ và mới cùng chạy song song trong quá trình chuyển đổi.

## Bảng đối chiếu 10 module

| Module | Trạng thái code hiện tại | Mức khớp Bible | Chiến lược | Priority | Risk |
|---|---|---|---|---|---|
| Voice | Có, hoạt động tốt (4 nguồn giọng) | Cao — lộ tên engine ra UI cần ẩn | Wrap | High | Low |
| Media | Rải rác trong index.js/avatar.js/veo.js | Thấp — thiếu metadata/validate | Refactor | Medium | Medium |
| Health Monitor | Mỗi file tự kiểm tra riêng, /health rỗng | Thấp | Replace | High | Medium |
| Platform Adapter | Chỉ gửi (RTMP), không nhận (comment) | Trung bình | Wrap + Extend | Critical | High |
| Live Session | 2 trạng thái running/stopped | Thấp — chưa phải state machine | Replace | Critical | High |
| Scene | Chưa có | — | Build | High | Medium |
| AI Host | Chưa có (kịch bản gõ tay) | — | Build | Critical | Medium |
| Automation | Chưa có | — | Build | High | Medium |
| Comment Engine | Chưa có | — | Build | Critical | High |
| Analytics | Chưa có | — | Build | Low | Low |

## Vi phạm kiến trúc cần xử lý ngay khi Wrap (không chờ Replace)

- Gọi hàm trực tiếp giữa module (VD: avatar.js → tts.synthesize()) — phải đổi thành gửi Command qua Event Bus ngay ở bước Wrap, theo ADR-0011.
- Human Override không nhất quán giữa avatar.js và streamer.js — áp dụng ADR-0014 ngay ở bước Wrap.

## Điều file này không định nghĩa

Không định nghĩa lịch trình sprint cụ thể (xem IMPLEMENTATION_ORDER.md) — chỉ ghi trạng thái đối chiếu tại một thời điểm.
