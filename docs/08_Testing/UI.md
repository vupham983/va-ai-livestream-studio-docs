# UI Testing

[SOURCE OF TRUTH]
Status: Frozen

## Phạm vi
Kiểm thử Workspace và Dock (03_UI_UX) — hiện 03_UI_UX vẫn đang skeleton, file này chỉ định nguyên tắc, chưa thể liệt kê kịch bản UI cụ thể.

## Nguyên tắc
1. UI test phải xác nhận UI Process không tự giữ trạng thái độc lập ngoài bản mirror nhận qua IPC (chương 05 — Runtime Model), tức là test phải phát hiện được nếu UI hiển thị sai lệch so với State thật ở Engine Process.
2. UI test không thay thế Integration Test — UI chỉ kiểm tra hiển thị đúng, không kiểm tra logic nghiệp vụ bên dưới.

## Điều file này không định nghĩa
Không định nghĩa kịch bản test cụ thể theo từng màn hình — chờ 03_UI_UX có nội dung thật.
