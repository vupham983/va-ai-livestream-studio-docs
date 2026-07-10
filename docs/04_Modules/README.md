# 04_Modules

[CHỈ THAM CHIẾU]
Status: Approved

Bản đồ 10 module và phụ thuộc giữa chúng. Chi tiết từng module nằm ở MODULE.md tương ứng — file này không lặp lại nội dung đó.

## Danh sách 10 module

| Module | Vùng trách nhiệm (00_overview.md) | File |
|---|---|---|
| Live Session | Runtime (Aggregate Root) | live_session/MODULE.md |
| Scene | Runtime + Cấu hình | scene/MODULE.md |
| AI Host | Runtime | ai_host/MODULE.md |
| Automation | Runtime | automation/MODULE.md |
| Comment Engine | Runtime | comment_engine/MODULE.md |
| Voice | Runtime | voice/MODULE.md |
| Media | Cấu hình | media/MODULE.md |
| Analytics | Runtime | analytics/MODULE.md |
| Health Monitor | Runtime | health_monitor/MODULE.md |
| Platform Adapter | Tích hợp | platform_adapter/MODULE.md |

## Sơ đồ phụ thuộc (Upstream → Downstream)

```
Platform Adapter
      |
      v
Comment Engine ------+--------------+
      |               |              |
      v               v              v
  Automation      AI Host      Analytics
      |               |
      |               v
      |            Voice
      |               |
      v               v
      +--------->   Scene
                      |
                      v
               Live Session (điều phối vòng đời, nhận yêu cầu đổi trạng thái từ mọi module qua Event Bus)
```

Health Monitor — theo dõi song song toàn bộ luồng trên, không nằm trong chuỗi nghiệp vụ chính (chỉ tiêu thụ event để giám sát).

Media — cấu hình được Scene và AI Host tham chiếu khi cần tài nguyên hình ảnh/âm thanh, không nằm trong luồng runtime chính.

## Ánh xạ năng lực sản phẩm → module phụ trách

Bảng này là điểm nối một chiều giữa FUNCTIONAL_SCOPE.md (góc nhìn người dùng) và module kỹ thuật — theo đúng ranh giới đã thống nhất ở DOCUMENTATION_SYSTEM.md mục 7 (không nêu tên module trong FUNCTIONAL_SCOPE.md, ánh xạ đặt ở đây).

| Năng lực (FUNCTIONAL_SCOPE.md) | Module phụ trách chính |
|---|---|
| 1. Workspace/Project Management | Không thuộc module runtime riêng — xem ghi chú dưới |
| 2. Live Session Lifecycle | Live Session |
| 3. Scene | Scene |
| 4. AI Host | AI Host |
| 5. Automation rule | Automation |
| 6. Tiếp nhận/phân loại bình luận | Comment Engine |
| 7. Voice | Voice |
| 8. Quản lý Media | Media |
| 9. Số liệu vận hành | Analytics |
| 10. Cảnh báo sớm | Health Monitor |
| 11. Kết nối nền tảng | Platform Adapter |
| 12. Human Override | Xuyên suốt mọi module — mỗi module tự đảm bảo khả năng bị tiếp quản, xem Invariants trong MODULE.md tương ứng |

Ghi chú: Workspace/Project Management (năng lực #1) không có module runtime riêng. Theo 00_overview.md, Workspace là một vùng trách nhiệm (Responsibility Layer), không phải module runtime — mỗi module (Scene, AI Host, Automation, Media) tự sở hữu phần Configuration State của chính mình (xem _MODULE_TEMPLATE.md). Không tạo module thứ 11 chỉ để "quản lý Workspace", vì module đó sẽ không có Responsibility rõ ràng, vi phạm nguyên tắc phân ranh giới ở chương 03.

## Điều file này không định nghĩa

Không mô tả input/output event chi tiết của từng module (thuộc từng MODULE.md), không mô tả interface kỹ thuật.
