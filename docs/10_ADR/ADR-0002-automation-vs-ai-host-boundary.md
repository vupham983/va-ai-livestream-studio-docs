# ADR-0002 — Ranh giới automation vs ai_host

**Applies-to:** v0.x

Status: Accepted

## Context

Hệ thống có hai loại hành vi dễ nhầm lẫn: chạy rule cố định và sinh nội dung không xác định trước. Cần một ranh giới rõ để tránh logic sinh nội dung bị rải rác trong engine rule.

## Decision

`automation` = engine chạy rule/trigger xác định trước (if X → do Y), không tự sinh nội dung. `ai_host` = agent sinh nội dung không xác định trước (lời thoại, phản ứng). Automation chỉ được gọi AI Host như một hành động (action) trong rule của nó, không được chứa logic sinh nội dung.

## Alternatives considered

Gộp chung automation và ai_host vào một module duy nhất — bị loại vì làm mờ ranh giới giữa rule cố định và nội dung sinh động, gây khó bảo trì lâu dài.

## Consequences

automation không được chứa logic sinh nội dung; mọi nội dung sinh động phải đi qua ai_host như một action.
