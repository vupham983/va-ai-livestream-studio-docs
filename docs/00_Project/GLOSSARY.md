# Glossary

[SOURCE OF TRUTH]
Status: Frozen

Định nghĩa chuẩn cho thuật ngữ dùng xuyên suốt dự án. Mọi tài liệu
khác phải dùng đúng nghĩa ở đây — không được định nghĩa lại.

**Live Session** — Một phiên livestream đang diễn ra, từ lúc bắt
đầu chuẩn bị (Preparing) tới lúc kết thúc (Ended). Domain lõi của
toàn hệ thống, được mô hình hoá dưới dạng state machine.

**Scene** — Một cấu hình hiển thị/âm thanh cụ thể (bố cục camera,
overlay, media) có thể được kích hoạt trong một Live Session. Một
Scene được tạo một lần, dùng lại được ở nhiều Session (xem
Philosophy #3).

**AI Host** — Thành phần sinh nội dung không xác định trước (lời
thoại, phản ứng) trong lúc live, đóng vai người dẫn chương trình ảo.

**Automation** — Engine chạy rule/trigger xác định trước (if X →
do Y). Không tự sinh nội dung; khi cần nội dung, gọi AI Host như
một hành động.

**Comment Engine** — Module tiếp nhận, phân loại, và xử lý bình
luận/tin nhắn từ khán giả trong lúc live.

**Voice** — Module chuyển văn bản (từ AI Host hoặc kịch bản) thành
giọng nói phát ra trong phiên live.

**Media** — Module quản lý tài nguyên hình ảnh/video/âm thanh dùng
trong Scene.

**Analytics** — Module thu thập và tổng hợp số liệu vận hành của
Live Session (lượt xem, tương tác, hiệu suất bán hàng...).

**Health Monitor** — Module giám sát tình trạng kết nối nền tảng
và tài nguyên máy, cảnh báo sớm nguy cơ gãy Live Session.

**Platform Adapter** — Lớp kết nối giữa hệ thống và một nền tảng
livestream cụ thể (TikTok, Facebook, YouTube, Shopee...). Thêm
nền tảng mới = thêm một Adapter, không sửa lõi hệ thống.

**Event Bus** — Kênh giao tiếp duy nhất giữa các module. Module
không gọi trực tiếp module khác qua function call, chỉ phát
(emit) và nhận (consume) event qua Event Bus.

**Workspace** — Không gian làm việc chính trên UI, chứa các Dock.

**Dock** — Một vùng giao diện có thể sắp xếp lại vị trí trong
Workspace (VD: dock Scene list, dock Comment feed).

**Human Override** — Khả năng con người can thiệp trực tiếp vào
bất kỳ hành động nào của AI Host hoặc Automation, bất kỳ lúc nào
trong Live Session, mà không cần dừng toàn bộ phiên (xem Philosophy #2).

**ADR (Architecture Decision Record)** — Bản ghi một quyết định
kiến trúc, bất biến sau khi Accepted (xem ARCHITECTURE_GOVERNANCE.md).

**SoT (Source of Truth)** — File duy nhất được phép định nghĩa
một sự thật trong hệ thống tài liệu.
