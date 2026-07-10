# Technical Roadmap

[CHỈ THAM CHIẾU]
Status: Frozen

Thứ tự triển khai kỹ thuật, phản ánh đúng thứ tự phụ thuộc đã có ở 04_Modules/README.md (sơ đồ Upstream/Downstream) — không tự sắp xếp lại.

## Thứ tự đề xuất
1. Live Session (Aggregate Root, nền tảng runtime) + Event Bus.
2. Platform Adapter (một nền tảng đầu tiên) + Comment Engine (đầu vào cần có trước để test luồng thật).
3. Scene + Media (hiển thị cơ bản).
4. Automation + AI Host + Voice (luồng phản hồi hoàn chỉnh).
5. Health Monitor (giám sát toàn bộ luồng đã có).
6. Analytics (không ảnh hưởng luồng chính, làm sau cùng trong nhóm MVP).

## Điều file này không định nghĩa
Không định nghĩa timeline cụ thể — đó là RELEASES.md khi có cam kết thời gian thật.
