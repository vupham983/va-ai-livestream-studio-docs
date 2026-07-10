# 04 — Data Flow

[SOURCE OF TRUTH]
Status: Frozen

Sơ đồ luồng dữ liệu ở mức hệ thống — module nào gửi gì cho module nào, qua kênh nào. Hình dạng dữ liệu và vòng đời chi tiết thuộc 05_Data/*, không lặp ở đây (02_Architecture/README.md quy tắc 3).

## Kênh giao tiếp duy nhất: Event Bus

Toàn bộ luồng dưới đây đi qua Event Bus (ADR-0004) — không có mũi tên nào trong sơ đồ là function call trực tiếp.

## Sơ đồ luồng chính trong một phiên Live

```
Platform (TikTok/FB/YouTube...)
      | (bình luận, sự kiện)
      v
Platform Adapter --emit--> Event Bus
                              |
                +-------------+-------------+
                v             v             v
        Comment Engine   Health Monitor   Analytics
                |
                | (câu hỏi cần trả lời)
                v
        Event Bus <--emit-- Comment Engine
                |
    +-----------+-----------+
    v           v
Automation   AI Host (được Automation gọi khi cần nội dung)
    |           |
    |           v
    |        Voice --audio--> Scene (hiển thị/phát)
    v
Event Bus --lệnh điều khiển--> Live Session (yêu cầu đổi trạng thái)
```

## Ba luồng chính

1. Luồng đầu vào (Inbound) — Platform Adapter nhận sự kiện từ bên ngoài, emit vào Event Bus dưới dạng chuẩn hoá nội bộ (không giữ định dạng gốc của từng nền tảng).
2. Luồng xử lý (Processing) — Comment Engine, Automation, AI Host tiêu thụ event, có thể emit event mới (VD: Comment Engine emit "cần AI Host trả lời", Automation emit "trigger action X").
3. Luồng đầu ra (Outbound) — Voice, Scene tiêu thụ event để tạo ra kết quả hiển thị/phát âm thanh thực tế trong phiên live.

## Nguyên tắc luồng dữ liệu

1. Một event chỉ có một nguồn phát (single producer per event type) — tránh nhầm lẫn ai chịu trách nhiệm phát sự kiện gì.
2. Nhiều module có thể cùng subscribe một event (VD: cả Health Monitor và Analytics cùng nghe event từ Platform Adapter) — đây là lý do chính Event Bus tồn tại thay vì gọi trực tiếp.
3. Live Session không tiêu thụ toàn bộ event nghiệp vụ chi tiết — chỉ nhận các event liên quan tới thay đổi trạng thái phiên (yêu cầu Pause, Resume, kết thúc...), theo đúng ranh giới ở chương 02.

## Điều chương này không định nghĩa

Không định nghĩa schema cụ thể của từng loại event, cơ chế hàng đợi (Queue) kỹ thuật, hay thời gian sống của event trong hệ thống — các nội dung đó thuộc 05_Data/EVENT.md và 05_Data/QUEUE.md.
