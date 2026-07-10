# 00 — Overview

[SOURCE OF TRUTH]
Status: Frozen

## Hình dạng tổng thể

VA AI Livestream Studio là một ứng dụng desktop, xoay quanh một
domain lõi duy nhất: Live Session (xem GLOSSARY.md, ADR-0001,
ADR-0007). Mọi thứ khác trong kiến trúc — 10 module, Event Bus,
runtime, tích hợp nền tảng — tồn tại để phục vụ hoặc điều phối
xoay quanh Live Session, không tồn tại như trung tâm độc lập
(PHILOSOPHY.md #1).

Ở mức hình dạng cao nhất, hệ thống có ba vùng trách nhiệm
(Responsibility Layers):

1. **Vùng Cấu hình (Workspace)** — nơi người dùng chuẩn bị Scene,
   AI Host persona, Automation rule, Media trước khi live. Các
   thực thể ở vùng này tái sử dụng được giữa nhiều phiên (Mission 6,
   Philosophy #3). Workspace không chứa trạng thái runtime của bất
   kỳ Live Session nào.
2. **Vùng Runtime (Live Session)** — nơi các thực thể cấu hình được
   "kích hoạt" thành instance đang chạy trong một phiên live cụ
   thể. Live Session là Runtime Aggregate Root (ADR-0007) — sở hữu
   toàn bộ trạng thái runtime của một phiên, không phải Aggregate
   Root của toàn bộ Workspace.
3. **Vùng Tích hợp (Platform Adapter)** — nơi hệ thống giao tiếp
   với thế giới bên ngoài (TikTok, Facebook, YouTube, Shopee, TTS,
   LLM). Platform Adapter không quyết định hành vi của hệ thống;
   nó chỉ chuyển đổi giao thức, dữ liệu, và sự kiện giữa hệ thống
   và nền tảng bên ngoài.

Ba vùng này giao tiếp với nhau qua một kênh duy nhất: Event Bus —
các module nghiệp vụ không gọi trực tiếp qua function call, mà
phát/nhận event qua Event Bus (ADR-0004; chi tiết luồng ở chương
04 và mô hình tiến trình ở chương 05).

## Sơ đồ tổng thể

```mermaid
flowchart TB
    subgraph Workspace["Vùng Cấu hình (Workspace)"]
        Scene[Scene]
        Persona[AI Host Persona]
        AutomationRule[Automation Rule]
        MediaAsset[Media Asset]
    end

    subgraph Runtime["Vùng Runtime (Live Session)"]
        LS[Live Session\n(Runtime Aggregate Root)]
        AIHost[AI Host]
        CommentEngine[Comment Engine]
        Voice[Voice]
        Automation[Automation Engine]
        HealthMonitor[Health Monitor]
        Analytics[Analytics]
        MediaRuntime[Media]
    end

    subgraph Integration["Vùng Tích hợp (Platform Adapter)"]
        OBS[OBS Adapter]
        TikTok[TikTok Adapter]
        Facebook[Facebook Adapter]
        YouTube[YouTube Adapter]
        Shopee[Shopee Adapter]
        TTS[TTS Adapter]
        LLM[LLM Adapter]
    end

    EventBus{{Event Bus}}

    Workspace -- "kích hoạt cấu hình vào" --> LS
    LS <--> EventBus
    AIHost <--> EventBus
    CommentEngine <--> EventBus
    Voice <--> EventBus
    Automation <--> EventBus
    HealthMonitor <--> EventBus
    Analytics <--> EventBus
    MediaRuntime <--> EventBus
    EventBus <--> Integration
```

**Ghi chú đọc sơ đồ:** mũi tên qua Event Bus là hai chiều vì mỗi
module vừa phát (emit) vừa nhận (consume) event; sơ đồ này không
thay thế chương 04 (Data Flow), chỉ minh hoạ hình dạng tổng thể.

## Cách đọc các chương tiếp theo

Đọc theo đúng thứ tự 01 → 08. Mỗi chương giả định người đọc đã
nắm nội dung chương trước đó (xem quy tắc tham chiếu ở README.md),
bắt đầu từ bối cảnh hệ thống (01), đi vào domain lõi (02), sau đó
ra ranh giới các module xung quanh domain lõi đó (03), luồng dữ
liệu giữa chúng (04), mô hình tiến trình thực thi (05), rồi đến
khả năng mở rộng (06), phục hồi lỗi (07), và giới hạn nền tảng (08).

## Vì sao không phải kiến trúc kiểu chatbot/agent framework

Nhiều hệ thống AI hiện nay coi "agent" là trung tâm điều phối:
agent quyết định gọi công cụ gì, khi nào. Ở đây thì ngược lại —
Live Session điều phối AI Host, không phải AI Host điều phối hệ
thống. AI Host là một module tạo nội dung khi được gọi trong ngữ
cảnh của một Live Session đang chạy, không phải bộ não trung tâm
ra quyết định vận hành. Ranh giới này được giữ xuyên suốt từ Vision
(không phải Agent Framework) tới Mission 2.

## Thứ tự đọc các chương còn lại

1. 01 — System Context: hệ thống đứng ở đâu (máy người dùng,
   không phải cloud), giao tiếp với những nhóm bên ngoài nào.
2. 02 — Core Domain: Live Session: mô hình hoá chi tiết state
   machine + aggregate boundary.
3. 03 — Module Boundaries: ranh giới trách nhiệm của 10 module,
   ai không được làm gì.
4. 04 — Data Flow: sơ đồ luồng dữ liệu ở mức hệ thống qua Event Bus.
5. 05 — Runtime Model: mô hình tiến trình (UI process tách Engine
   process).
6. 06 — Extensibility: cách thêm module/nền tảng/AI Host mới.
7. 07 — Failure Recovery: hệ thống phản ứng thế nào khi một phần lỗi.
8. 08 — Platform Constraints: giới hạn từ việc chạy trên desktop
   cá nhân, không phải server.

## Điều chương này không định nghĩa

Chương 00 không định nghĩa chi tiết kỹ thuật của bất kỳ module,
luồng dữ liệu, hay runtime cụ thể nào — những nội dung đó thuộc
các chương tương ứng phía trên, theo đúng quy tắc tham chiếu ở
02_Architecture/README.md.
