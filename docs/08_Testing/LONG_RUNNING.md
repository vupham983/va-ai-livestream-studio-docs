# Long Running Testing

[SOURCE OF TRUTH]
Status: Frozen

## Phạm vi
Kiểm thử một Live Session chạy liên tục nhiều giờ mà không khởi động lại (PRODUCT_GOALS.md Goal 2).

## Nguyên tắc
1. Test phải chạy đủ dài để phát hiện rò rỉ bộ nhớ (memory leak) tích luỹ trong Runtime Data (05_Data/RUNTIME_DATA.md) qua thời gian.
2. Test phải xác nhận autosave/Snapshot định kỳ (05_Data/SNAPSHOT.md) hoạt động đúng trong suốt phiên dài, không chỉ lúc bắt đầu/kết thúc.

## Điều file này không định nghĩa
Không định nghĩa thời lượng test cụ thể (bao nhiêu giờ) — chọn theo kịch bản sử dụng thực tế khi triển khai.
