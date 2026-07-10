# Naming Conventions

[SOURCE OF TRUTH]
Status: Frozen

## Nguyên tắc
1. Tên module trong code phải khớp chính xác tên đã dùng ở 04_Modules và GLOSSARY.md (VD: `live_session`, `ai_host`, `comment_engine`) — không dịch nghĩa, không viết tắt khác đi.
2. Tên Event phải mô tả sự việc đã xảy ra (past tense), không mô tả lệnh (VD: `scene_activated`, không phải `activate_scene`) — vì Event thông báo sự việc, không ra lệnh (05_Data/EVENT.md).
3. Tên biến/hàm liên quan tới Configuration vs Runtime phải phân biệt rõ trong code (VD: prefix hoặc namespace khác nhau) — phản ánh đúng ranh giới Aggregate đã chốt ở chương 02.

## Điều file này không định nghĩa
Không định nghĩa quy ước cụ thể theo ngôn ngữ lập trình (camelCase/snake_case) — chờ chọn ngôn ngữ chính thức.
