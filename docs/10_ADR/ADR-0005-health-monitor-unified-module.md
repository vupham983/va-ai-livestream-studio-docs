# ADR-0005 — Health Monitor là một module thống nhất

**Applies-to:** v0.x

Status: Accepted

## Context

Hệ thống cần giám sát cả kết nối nền tảng (TikTok/FB API...) lẫn tài nguyên máy (CPU/RAM/mạng), cả hai đều phục vụ cùng một mục đích: cảnh báo sớm nguy cơ gãy Live Session.

## Decision

Health Monitor giám sát cả hai domain — kết nối nền tảng và tài nguyên máy — nhưng gộp trong một module duy nhất với 2 sub-domain. Không tách module riêng.

## Alternatives considered

Tách thành hai module riêng biệt (platform health monitor và resource health monitor) — bị loại vì cả hai cùng phục vụ một mục đích duy nhất (cảnh báo sớm nguy cơ gãy Live Session), tách ra sẽ tạo phối hợp thừa.

## Consequences

Health Monitor sở hữu cả hai sub-domain giám sát, không có module giám sát thứ hai song song.
