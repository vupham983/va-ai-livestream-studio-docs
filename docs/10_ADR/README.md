# 10_ADR — README

[CHỈ THAM CHIẾU]
Status: Frozen

Mục lục ADR. ADR bất biến sau khi Accepted — sửa quyết định = tạo ADR mới, đánh dấu ADR cũ Superseded (ARCHITECTURE_GOVERNANCE.md mục 2).

## Danh sách

| ADR | Tên | Status | Relates to |
|---|---|---|---|
| ADR-0001 | Live Session là state machine | Accepted | 02_Architecture/chapters/02_core_domain_live_session.md; ADR-0007 |
| ADR-0002 | Ranh giới automation vs ai_host | Accepted | 04_Modules/automation/MODULE.md, ai_host/MODULE.md |
| ADR-0003 | Runtime multi-process ngay từ đầu (mô hình OBS) | Accepted | 02_Architecture/chapters/05_runtime_model.md; 05_Data (State/Cache qua IPC) |
| ADR-0004 | Event Bus bắt buộc | Accepted | 05_Data/EVENT.md, QUEUE.md; toàn bộ 04_Modules/*/MODULE.md |
| ADR-0005 | Health Monitor là một module thống nhất | Accepted | 04_Modules/health_monitor/MODULE.md |
| ADR-0006 | Docs versioning qua ADR | Accepted | Toàn bộ ADR (header Applies-to: v0.x) |
| ADR-0007 | Live Session là Runtime Aggregate Root | Accepted | ADR-0001; 02_Architecture/chapters/02_core_domain_live_session.md; 04_Modules/_MODULE_TEMPLATE.md; 05_Data/STATE.md, SNAPSHOT.md |
| ADR-0008 | Mở rộng _MODULE_TEMPLATE.md từ 10 mục lên 15 mục | Accepted | DOCUMENTATION_SYSTEM.md mục 3; toàn bộ 04_Modules/*/MODULE.md |

Chưa có ADR nào bị Superseded tính đến thời điểm này.

## Điều file này không định nghĩa

Không lặp lại nội dung Context/Decision/Alternatives/Consequences của từng ADR — đọc file ADR tương ứng.
