# 05 — Runtime Model

[SOURCE OF TRUTH]
Status: Frozen

Mô hình tiến trình của ứng dụng — quyết định đã chốt tại ADR-0003 (multi-process, mô hình OBS).

## Hai tiến trình chính

```
+-------------------+        IPC        +-------------------+
|   UI Process       |<------------------>|  Engine Process     |
|                     |                    |                     |
| Workspace, Dock,    |                    | Live Session,       |
| hiển thị trạng      |                    | Event Bus, toàn bộ  |
| thái, nhận input    |                    | 10 module nghiệp    |
| người dùng          |                    | vụ, Platform        |
|                     |                    | Adapter             |
+-------------------+                    +-------------------+
```

- UI Process — chịu trách nhiệm hiển thị Workspace lúc chuẩn bị và Dock giám sát lúc live. Không chạy logic nghiệp vụ.
- Engine Process — chạy Live Session, Event Bus, toàn bộ 10 module, và giao tiếp với Platform Adapter. Đây là nơi AI inference, encode, và mọi tải nặng diễn ra.

## Vì sao tách hai tiến trình (nhắc lại lý do đã chốt ở ADR-0003)

UI không được đứng hình khi Engine tải nặng — một phiên live chạy AI inference và encode liên tục nhiều giờ không được phép làm người vận hành mất khả năng thao tác Human Override đúng lúc cần nhất.

## Giao tiếp giữa hai tiến trình

UI Process và Engine Process giao tiếp qua IPC (Inter-Process Communication) — đây là ngoại lệ duy nhất với nguyên tắc "chỉ qua Event Bus" ở chương 04, vì Event Bus vận hành bên trong Engine Process; kênh IPC là cầu nối ở lớp thấp hơn để đưa event/trạng thái từ Engine sang UI hiển thị, và đưa lệnh người dùng từ UI vào Engine. Cơ chế IPC cụ thể (giao thức, định dạng) sẽ được quyết định bằng ADR riêng khi bắt đầu triển khai.

## Hệ quả cho State/Cache

State và Cache (05_Data/STATE.md, 05_Data/CACHE.md) sống trong Engine Process — UI Process không giữ bản sao trạng thái độc lập, chỉ hiển thị bản phản chiếu (mirror) nhận qua IPC. Điều này tránh tình trạng UI và Engine lệch trạng thái nhau.

## Điều chương này không định nghĩa

Không định nghĩa giao thức IPC cụ thể, không định nghĩa cách từng module bên trong Engine Process được lập lịch (scheduling) — đó là chi tiết triển khai, không phải kiến trúc mức Bible.
