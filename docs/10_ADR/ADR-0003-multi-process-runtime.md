# ADR-0003 — Runtime multi-process ngay từ đầu

**Applies-to:** v0.x

Status: Accepted

## Context

UI không được đứng hình khi Engine tải nặng (AI inference, encode). Đổi mô hình runtime sau này rất đắt cho một dự án vận hành 10 năm.

## Decision

Runtime tách UI process khỏi Engine process ngay từ đầu (mô hình OBS).

## Alternatives considered

Chạy UI và Engine chung một process (đơn giản hơn ban đầu) — bị loại vì rủi ro UI đứng hình khi Engine tải nặng, và chi phí tái cấu trúc sau này quá cao.

## Consequences

Là nền cho `chapters/05_runtime_model.md`. Ảnh hưởng `05_Data`: State/Cache phải qua cơ chế IPC, không dùng shared memory ngầm định.
