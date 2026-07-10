# 02 — Core Domain: Live Session

[SOURCE OF TRUTH]
Status: Frozen

## Live Session là gì về mặt kiến trúc

Live Session là domain lõi của hệ thống, có hai đặc tính đồng thời
(xem ADR-0001, ADR-0007):

1. State Machine — có vòng đời trạng thái tường minh, module
   khác hành xử theo trạng thái hiện tại, không tự suy luận.
2. Runtime Aggregate Root — sở hữu toàn bộ thực thể runtime
   phát sinh trong lúc phiên đang chạy; các thực thể đó bị huỷ
   hoặc lưu trữ cùng lúc phiên kết thúc.

## Vòng đời trạng thái

```
Idle → Preparing → Live → Paused → Ended
                     │
                     ▼
              Error/Recovering
```

- Idle — chưa có phiên nào được khởi tạo.
- Preparing — phiên đã tạo, đang nạp cấu hình từ Workspace
  (Scene, AI Host, Automation instance hoá thành runtime).
- Live — đang phát sóng, Event Bus hoạt động đầy đủ.
- Paused — tạm dừng phát sóng nhưng phiên chưa kết thúc
  (VD: Human Override tạm dừng để xử lý sự cố).
- Error/Recovering — phát hiện lỗi ở một module, đang cố phục
  hồi mà không huỷ toàn bộ phiên (chi tiết chiến lược phục hồi ở
  chương 07).
- Ended — phiên kết thúc, toàn bộ trạng thái runtime được lưu
  trữ (snapshot) hoặc giải phóng.

Chuyển trạng thái chỉ được thực hiện bởi chính Live Session (thông
qua lệnh nội bộ), không module nào khác được phép ép Live Session
đổi trạng thái trực tiếp — module khác chỉ có thể yêu cầu chuyển
trạng thái qua Event Bus, và Live Session quyết định có chấp nhận
hay không.

## Ranh giới sở hữu (Aggregate Boundary)

Thuộc sở hữu của Live Session (runtime, mất khi phiên Ended):
- Instance Scene đang active.
- Instance AI Host đang chạy trong phiên (persona được nạp, trạng
  thái hội thoại hiện tại).
- Trạng thái Automation đang thực thi (rule nào đang theo dõi,
  đã trigger bao nhiêu lần).
- Hàng đợi Comment đang chờ xử lý.
- Trạng thái Health hiện tại của phiên.

KHÔNG thuộc sở hữu của Live Session (cấu hình, sống ở Workspace,
xem chương 00):
- Scene template, AI Host persona định nghĩa gốc, Automation rule
  định nghĩa gốc, Media asset gốc.

Một entity cấu hình có thể được nhiều Live Session khác nhau
instance hoá đồng thời hoặc tuần tự — nhưng mỗi instance runtime
chỉ thuộc về đúng một Live Session.

## Vì sao cần phân biệt rạch ròi hai loại sở hữu này

Nếu không phân biệt, có nguy cơ một module vô tình sửa trực tiếp
cấu hình gốc trong Workspace từ giữa một phiên live đang chạy
(VD: AI Host "học" và tự sửa persona gốc) — vi phạm Mission 6
(Build Once, Reuse Everywhere) vì phá tính ổn định của cấu hình
dùng chung. Mọi thay đổi phát sinh trong lúc live chỉ áp dụng cho
instance runtime của phiên đó, trừ khi người dùng chủ động lưu
ngược lại thành cấu hình mới ở Workspace (thao tác tường minh,
không tự động).

## Điều chương này không định nghĩa

Chương này không liệt kê chi tiết trách nhiệm từng module tương
tác với Live Session (chương 03), không mô tả cơ chế Event Bus kỹ
thuật (chương 04), và không mô tả cách phục hồi cụ thể khi
Error/Recovering (chương 07).
