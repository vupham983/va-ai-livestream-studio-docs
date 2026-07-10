# Dependency Rules

[SOURCE OF TRUTH]
Status: Frozen

## Nguyên tắc phụ thuộc trong code
1. Code của một module không được import trực tiếp code của module khác — mọi giao tiếp qua Event Bus (ADR-0004), khớp đúng 04_Modules/README.md (sơ đồ Upstream/Downstream).
2. Thư viện bên thứ ba dùng trong một module không được rò rỉ ra module khác (VD: nếu Voice dùng SDK của một TTS provider cụ thể, module khác không được import trực tiếp SDK đó) — khớp nguyên tắc "Voice là abstraction" đã chốt ở 04_Modules/voice/MODULE.md.
3. Dependency mới (thư viện ngoài) phải được cân nhắc theo Principles #3 (đơn giản thắng linh hoạt) — không thêm thư viện chỉ để dùng một hàm nhỏ có thể tự viết.

## Điều file này không định nghĩa
Không liệt kê danh sách thư viện cụ thể được phép dùng — quyết định khi chọn công nghệ triển khai.
