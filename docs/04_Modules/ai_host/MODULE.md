# AI Host — MODULE

[SOURCE OF TRUTH]
Status: Frozen

## Purpose

Một Content Generation Component — sinh nội dung nói không xác định trước (lời thoại, phản ứng) theo persona đã cấu hình, khi được gọi trong ngữ cảnh một Live Session đang chạy. AI Host KHÔNG phải một Decision Engine — nó không tự quyết định khi nào hành động, chỉ tạo nội dung khi được yêu cầu (Philosophy #1, chương 00).

## Public Contract

- Sinh một đoạn nội dung nói theo persona đã cấu hình, khi nhận yêu cầu hợp lệ kèm ngữ cảnh cần thiết.
- Duy trì phong cách/persona nhất quán trong suốt một phiên đang chạy.
- Phát nội dung đã sinh ra dưới dạng event để module khác (Voice) tiêu thụ — không tự phát âm thanh, không tự hiển thị.

## Responsibilities

- Sinh nội dung không xác định trước (lời thoại, phản ứng) đúng persona đang active trong phiên.
- Giữ ngữ cảnh hội thoại cần thiết trong phạm vi một phiên để nội dung sinh ra mạch lạc.
- Phát nội dung đã sinh dưới dạng event, không tự thực thi bất kỳ hành động nào khác ngoài sinh nội dung.

## Boundaries (không làm gì)

- Không tự trigger hành động ngoài phạm vi được gọi (ranh giới Automation vs AI Host, ADR-0002).
- Không sở hữu workflow của Live Session — không được chuyển Scene, không phát Media, không kết thúc phiên, không mở livestream. Chỉ sinh nội dung, đề xuất, và phát event.
- Không tự đọc trực tiếp dữ liệu từ Platform Adapter — chỉ nhận ngữ cảnh đã qua Comment Engine/Automation chuẩn hoá (Trust Boundary, chương 01).

## Inputs

- Yêu cầu sinh nội dung kèm ngữ cảnh cần thiết (từ Automation, theo rule đã trigger; hoặc từ Human Override, khi người vận hành yêu cầu trực tiếp — VD: bấm "đọc lời chào").
- Ngữ cảnh hội thoại liên quan (câu hỏi đã được Comment Engine phân loại và Automation chuyển tiếp, nếu có).

## Outputs

- Nội dung đã sinh (văn bản) — emit ra Event Bus để Voice tiêu thụ và chuyển thành giọng nói.

## Upstream Dependencies

- Automation — nguồn kích hoạt mặc định, theo rule (if-trigger). Mọi phản ứng theo trạng thái phiên (chào khi bắt đầu, tạm biệt khi kết thúc) đi qua Automation lắng nghe event trạng thái phiên, không phải Live Session gọi AI Host trực tiếp — tránh hai đường kích hoạt trùng chức năng, vi phạm nguyên tắc "một event một nguồn phát" (chương 04).
- Human Override — kích hoạt trực tiếp qua Event Bus khi người vận hành yêu cầu AI Host phát biểu ngay lập tức.

## Downstream Consumers

- Voice — tiêu thụ nội dung đã sinh để chuyển thành giọng nói. AI Host không biết Voice dùng TTS provider nào (Edge/XTTS/ElevenLabs/...) — Voice là một abstraction, đổi provider không ảnh hưởng AI Host (chương 06).

## Configuration State

- Persona definition: giọng, phong cách, prompt nền — thuộc sở hữu Workspace, tái sử dụng giữa nhiều Live Session (Mission 6, Philosophy #3).
- Knowledge Reference: tham chiếu tới voice, style, prompt, knowledge set liên quan — AI Host chỉ tham chiếu, không sở hữu nội dung Knowledge thật (hiện chưa có module Knowledge riêng trong 10 module).
- LLM provider và API key — quản lý bởi module AI Host, không lộ ra module khác (NON_FUNCTIONAL_SCOPE.md — Security).

## Runtime State

Thuộc sở hữu Live Session theo Aggregate Boundary (chương 02, ADR-0007) — mất khi phiên Ended:
- Current speaking state (đang sinh nội dung / đang rảnh).
- Current conversation context (ngữ cảnh hội thoại hiện tại trong phiên).
- Pending response (nếu có nội dung đang chờ hoàn tất/phát ra).
- Active persona instance (persona đang được nạp cho phiên hiện tại).

## Invariants

- Không đọc trực tiếp dữ liệu từ Platform (Trust Boundary, chương 01).
- Không điều khiển OBS/Scene.
- Không tự phát Voice (chỉ emit nội dung, Voice mới là nơi phát âm thanh).
- Không tự quyết định workflow (không tự trigger hành động ngoài phạm vi được gọi).
- Không ghi trực tiếp vào Live Session State — chỉ sinh nội dung hoặc phát event theo yêu cầu.

## Failure Modes

- Lỗi kỹ thuật (LLM API timeout/lỗi phản hồi) — Critical nếu xảy ra khi đang cần nội dung cho luồng chính đang Live, theo phân loại chương 07.
- Low Confidence — AI không đủ tự tin vào nội dung sinh ra (khác với lỗi kỹ thuật) — Non-critical.

## Recovery Strategy

- Lỗi kỹ thuật: retry theo cơ chế chung ở chương 07, cảnh báo Health Monitor nếu lặp lại.
- Low Confidence: cảnh báo → chờ Human Override quyết định, hoặc tự động dùng lại nội dung định trước từ Automation rule (không phát minh cơ chế "Fallback Script" mới ngoài hệ thống — dùng đúng các bước đã có ở chương 07).

## Extension Points

- Thêm AI Host persona mới không phải thêm module — persona là cấu hình (Workspace), AI Host load persona như dữ liệu, không cần sửa code (chương 06).

## Related ADRs

- ADR-0002 — Ranh giới Automation vs AI Host.
- ADR-0004 — Event Bus bắt buộc.
- ADR-0007 — Live Session = Runtime Aggregate Root (Runtime State của AI Host thuộc sở hữu Live Session).
