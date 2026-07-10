# 08 — Platform Constraints

[SOURCE OF TRUTH]
Status: Frozen

Giới hạn phát sinh từ việc chạy trên desktop cá nhân, không phải server — ràng buộc thực tế mà mọi chương trước phải tôn trọng.

## Giới hạn tài nguyên máy người dùng

Hệ thống chạy trên máy của người vận hành (chương 01), không có quyền giả định phần cứng mạnh hay ổn định như server. Cụ thể:
- Không giả định có GPU rời — AI inference cục bộ (nếu có) là tuỳ chọn, không bắt buộc để chạy MVP (MVP_DEFINITION.md dựa vào dịch vụ AI bên thứ ba qua Platform Adapter/Integration, không phụ thuộc phần cứng AI cục bộ).
- Không giả định mạng ổn định liên tục — mất kết nối tạm thời là tình huống bình thường phải xử lý được (chương 07), không phải trường hợp ngoại lệ hiếm gặp.
- Tài nguyên bị chia sẻ với các phần mềm khác người dùng đang chạy song song (trình duyệt, OBS nếu dùng kèm...) — Health Monitor phải tính đến việc CPU/RAM không dành riêng 100% cho ứng dụng (NON_FUNCTIONAL_SCOPE.md — Observability).

## Giới hạn từ việc là một tiến trình desktop, không phải multi-tenant

Không có khái niệm "nhiều người dùng cùng lúc trên một instance" như ứng dụng web nhiều người dùng — mỗi máy chạy một instance phục vụ một người vận hành (hoặc một đội dùng chung một máy). Điều này đơn giản hoá đáng kể mô hình bảo mật (NON_FUNCTIONAL_SCOPE.md — Security) so với hệ thống multi-tenant, nhưng đồng nghĩa không có cơ chế đồng bộ nhiều máy tích hợp sẵn ở phạm vi kiến trúc hiện tại.

## Giới hạn từ hệ điều hành

Ứng dụng phải hoạt động ổn định trên hệ điều hành desktop phổ biến người dùng mục tiêu sử dụng. Quyết định cụ thể hỗ trợ hệ điều hành nào (Windows-only hay đa nền tảng) chưa được chốt ở giai đoạn tài liệu này — sẽ ghi ADR khi bắt đầu triển khai kỹ thuật, không suy đoán trước ở đây.

## Hệ quả cho toàn bộ Architecture Bible

Mọi quyết định ở các chương trước (runtime multi-process ở chương 05, chiến lược phục hồi ở chương 07) đều phải khả thi trong giới hạn một máy desktop cá nhân — không chương nào được giả định hạ tầng cấp server (load balancer, nhiều instance chạy song song, orchestration) mà không có ADR riêng giải thích vì sao cần vượt ra ngoài giới hạn desktop-only này.

## Điều chương này không định nghĩa

Không liệt kê danh sách hệ điều hành/phiên bản cụ thể được hỗ trợ chính thức — đó là quyết định triển khai, ghi ở ADR riêng khi có đủ thông tin, không phải nội dung Architecture Bible.
