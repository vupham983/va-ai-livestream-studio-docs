# Dock System

[SOURCE OF TRUTH]
Status: Frozen

## Vai trò
Cơ chế cho phép các vùng giao diện (Dock — GLOSSARY.md) sắp xếp lại vị trí, theo phong cách tham chiếu OBS Studio/vMix.

## Các Dock đã biết
- Dock danh sách Scene.
- Dock Comment feed.
- Dock trạng thái Health Monitor.
- Dock Analytics.

## Nguyên tắc
1. Người dùng có thể tuỳ biến vị trí Dock, nhưng vị trí Human Override và trạng thái Live Session hiện tại không được phép bị ẩn hoàn toàn khỏi layout (Philosophy #2).
2. Dock chỉ hiển thị dữ liệu — không chứa logic nghiệp vụ (chương 05).
3. Hệ thống phải cho phép lưu, khôi phục, và reset bố cục Dock. Một bố cục không hợp lệ hoặc panel nằm ngoài vùng hiển thị phải có thể phục hồi về bố cục an toàn.
4. Khi chuyển sang Live Operations Mode, hệ thống có thể áp dụng một layout preset ưu tiên monitoring và Human Override, nhưng không được xoá layout tuỳ chỉnh của người dùng.

## Điều file này không định nghĩa
Không định nghĩa cơ chế kỹ thuật kéo-thả cụ thể.
