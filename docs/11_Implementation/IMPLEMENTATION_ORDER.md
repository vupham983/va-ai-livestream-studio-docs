# Implementation Order

[SOURCE OF TRUTH]
Status: Frozen

Chi tiết hoá TECHNICAL_ROADMAP.md thành sprint kỹ thuật cụ thể.

## Sprint 0 — Nền tảng chặn code
Resolve toàn bộ mục "Bắt buộc trước khi code Live Session / Event Bus" ở PENDING_DECISIONS.md bằng ADR (transition table đầy đủ, Command/Event/Query taxonomy, delivery semantics, single-producer enforcement, Aggregate mutation model). Không viết code nào trước khi Sprint 0 xong.

## Sprint 1 — Live Session + Event Bus
Dựng state machine đầy đủ (theo transition table đã ADR ở Sprint 0), Event Bus theo đúng taxonomy đã chọn. Điều kiện vào: ADR Sprint 0 đã Accepted.

## Sprint 2 — Platform Adapter (một nền tảng đầu) + Comment Engine
Điều kiện vào: PENDING_DECISIONS mục "Platform Adapter/Integration" đã resolve (phân nhóm Adapter, backpressure, clock model tối thiểu cho một nền tảng).

## Sprint 3 — Scene + Media
Điều kiện vào: PENDING_DECISIONS mục Workspace/Persistence liên quan tới Scene (schema, storage) đã resolve.

## Sprint 4 — Automation + AI Host + Voice
Điều kiện vào: ranh giới Automation/AI Host (ADR-0002) đã đủ để code — không cần thêm ADR mới trừ khi phát sinh vấn đề khi code Sprint 1-3.

## Sprint 5 — Health Monitor + Analytics
Điều kiện vào: Engine supervision/crash recovery (PENDING_DECISIONS) đã resolve, vì Health Monitor phụ thuộc trực tiếp cơ chế đó.

## Sprint 6 — IPC + UI Process
Điều kiện vào: giao thức IPC (PENDING_DECISIONS) đã resolve bằng ADR.

## Điều file này không định nghĩa
Không ước lượng thời gian cụ thể cho từng sprint — đó là RELEASES.md khi có dữ liệu triển khai thật.
