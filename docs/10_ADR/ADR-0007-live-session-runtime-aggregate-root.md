# ADR-0007 — Live Session là Runtime Aggregate Root

Status: Accepted

## Context

Ngoài đặc tính state machine (ADR-0001), Live Session còn cần một ranh giới sở hữu rõ ràng cho toàn bộ thực thể runtime phát sinh trong lúc một phiên đang chạy (instance Scene đang active, instance AI Host, trạng thái Automation đang thực thi, hàng đợi Comment, trạng thái Health của phiên). Nếu không có một thực thể duy nhất chịu trách nhiệm sở hữu các thực thể runtime này, chúng có nguy cơ trở thành state "mồ côi" — sống ngoài vòng đời của bất kỳ phiên nào, gây rò rỉ trạng thái hoặc lẫn lộn giữa dữ liệu cấu hình (Workspace) và dữ liệu runtime.

## Decision

Live Session là Runtime Aggregate Root — sở hữu toàn bộ trạng thái runtime phát sinh trong một phiên (xem ranh giới sở hữu chi tiết tại 02_Architecture/chapters/02_core_domain_live_session.md). Mọi thực thể runtime chỉ tồn tại trong vòng đời của đúng một Live Session; khi phiên chuyển sang Ended, toàn bộ trạng thái runtime thuộc Aggregate này được lưu trữ (snapshot) hoặc giải phóng.

Live Session KHÔNG phải là Aggregate Root của toàn bộ Workspace — cấu hình gốc (Scene template, AI Host persona định nghĩa gốc, Automation rule định nghĩa gốc, Media asset gốc) không thuộc sở hữu của Live Session, và có thể được nhiều Live Session khác nhau instance hoá đồng thời hoặc tuần tự.

## Alternatives considered

- Để mỗi module runtime tự quản lý vòng đời state của riêng mình, không có Aggregate Root chung — bị loại vì không có cơ chế đảm bảo toàn bộ state runtime bị dọn dẹp đồng thời khi phiên kết thúc, dẫn tới nguy cơ rò rỉ trạng thái giữa các phiên.
- Gộp Workspace và Live Session vào cùng một Aggregate Root — bị loại vì vi phạm nguyên tắc phân biệt cấu hình (tái sử dụng, sống lâu dài) và runtime (tạm thời, mất khi phiên Ended), phá vỡ Mission 6 (Build Once, Reuse Everywhere).

## Consequences

- Mọi MODULE.md phải phân biệt rõ Configuration State và Runtime State (xem _MODULE_TEMPLATE.md), và khai báo Runtime State nào thuộc sở hữu của Live Session.
- 05_Data/STATE.md và 05_Data/SNAPSHOT.md phải tuân theo đúng ranh giới Aggregate này khi mô tả vòng đời dữ liệu.
