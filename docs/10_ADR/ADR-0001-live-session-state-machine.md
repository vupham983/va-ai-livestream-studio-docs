# ADR-0001 — Live Session là state machine

**Applies-to:** v0.x

Status: Accepted

## Context

Domain lõi của hệ thống cần một mô hình trạng thái tường minh để mọi module khác có thể phản ứng nhất quán, thay vì mỗi module tự suy luận trạng thái live theo cách riêng.

## Decision

Live Session được mô hình hoá như một state machine với các trạng thái: `Idle → Preparing → Live → Paused → Ended` (+ `Error/Recovering`). Mọi module khác subscribe theo state qua Event Bus, không tự suy luận trạng thái.

## Alternatives considered

Để module tự suy luận trạng thái qua tín hiệu rời rạc (không có state machine tường minh) — bị loại vì dễ lệch trạng thái giữa các module theo thời gian.

## Consequences

Là nền cho `chapters/02_core_domain_live_session.md`. Mọi module phải subscribe Event Bus thay vì tự kiểm tra trạng thái live.
