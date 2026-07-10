# Vision

[SOURCE OF TRUTH]
Status: Frozen

## Vì sao dự án này tồn tại

Livestream bán hàng tại Việt Nam đang phụ thuộc hoàn toàn vào
con người: một người vừa nói, vừa canh bình luận, vừa chốt đơn,
vừa xử lý sự cố kỹ thuật, liên tục nhiều giờ mỗi ngày. Mô hình
này không mở rộng được — muốn tăng số phiên live, phải tăng số
người, và chất lượng phụ thuộc vào thể trạng, tâm trạng người dẫn.

VA AI Livestream Studio tồn tại để giải quyết đúng vấn đề đó:
biến việc vận hành một buổi livestream chuyên nghiệp thành một
quy trình có thể **vận hành bởi AI, giám sát bởi con người** —
không phải ngược lại.

## Tầm nhìn 10 năm

Trong 10 năm tới, VA AI Livestream Studio trở thành phần mềm
desktop tiêu chuẩn cho vận hành livestream bằng AI tại thị trường
nói tiếng Việt trước, sau đó mở rộng khu vực — theo cách OBS Studio
đã trở thành tiêu chuẩn cho phát trực tiếp thủ công.

Phần mềm này được đo bằng một câu hỏi duy nhất: **một buổi
livestream có thể chạy ổn định, đúng kịch bản, đúng phản xạ
với khách hàng, mà không cần con người can thiệp liên tục hay không?**

## Ranh giới với các sản phẩm khác

- Đây **không phải** chatbot — chatbot trả lời từng tin nhắn rời rạc,
  không có khái niệm "phiên live" đang chạy.
- Đây **không phải** AI Agent Framework — framework là công cụ cho
  lập trình viên xây ứng dụng khác; đây là một ứng dụng hoàn chỉnh
  cho người vận hành livestream, không cần biết lập trình.
- Đây **không phải** wrapper gọi API LLM — LLM chỉ là một thành
  phần bên trong, không phải toàn bộ giá trị sản phẩm.

## Điều không thay đổi qua các phiên bản

Dù kiến trúc kỹ thuật, mô hình AI, hay nền tảng tích hợp (TikTok,
Facebook, YouTube...) có thay đổi theo thời gian, một điều giữ
nguyên: **Live Session luôn là trung tâm của hệ thống.** Mọi
module — AI Host, Automation, Comment Engine, Analytics — tồn tại
để phục vụ một phiên livestream đang diễn ra, không tồn tại độc lập.
