# ADR-0006 — Docs versioning qua ADR

**Applies-to:** v0.x

Status: Accepted

## Context

Cần một cách theo dõi lịch sử thay đổi kiến trúc theo phiên bản mà không phát sinh thêm một hệ thống tài liệu song song.

## Decision

Mỗi ADR gắn header `Applies-to: v0.x`. Không tạo changelog riêng cho docs/ — ADR tự đóng vai trò lịch sử thay đổi kiến trúc.

## Alternatives considered

Tạo một CHANGELOG.md riêng cho docs/ — bị loại vì trùng lặp vai trò với ADR và có nguy cơ hai nơi ghi lịch sử lệch nhau.

## Consequences

Mọi ADR mới phải có header Applies-to; không có file changelog riêng cho tài liệu.
