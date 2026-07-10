# VA AI Livestream Studio — Documentation Foundation

> Vai trò tài liệu này: đặt nền móng cho toàn bộ hệ thống tài liệu dự án, trước khi bước vào Architecture Bible. Không chứa nội dung kiến trúc, không chứa code.

---

## 0. Nguyên tắc chỉ đạo (áp dụng xuyên suốt)

1. **Single Source of Truth (SoT):** mỗi sự thật chỉ được định nghĩa ở đúng một file. Mọi nơi khác chỉ **tham chiếu** (link), không sao chép nội dung.
2. **Tách "Vì sao" khỏi "Là gì" khỏi "Làm sao":** Vision/Philosophy (Why) → Product (What) → Architecture/Modules (How it's built) → Development (How we build it).
3. **Module = đơn vị bảo trì:** mỗi module trong `04_Modules` có template giống hệt nhau để dự án 10 năm không "mục nát" theo phong cách người viết.
4. **ADR là nhật ký, không phải tài liệu sống:** quyết định kiến trúc không được sửa lại sau khi ban hành — chỉ có thể bị "superseded" bởi ADR mới.
5. **Không tài liệu nào được mô tả UI chi tiết ngoài `03_UI_UX`**, không tài liệu nào mô tả hành vi module ngoài `04_Modules`, không tài liệu nào mô tả tích hợp bên thứ ba ngoài `06_Integrations`.

---

## 1. Cây thư mục tổng thể

```

docs/
├── 00_Project/
│   ├── README.md
│   ├── VISION.md
│   ├── MISSION.md
│   ├── PHILOSOPHY.md
│   ├── PRINCIPLES.md
│   └── GLOSSARY.md
│
├── 01_Product/
│   ├── README.md
│   ├── PRODUCT_VISION.md
│   ├── PRODUCT_GOALS.md
│   ├── PERSONAS.md
│   ├── USER_JOURNEYS.md
│   ├── USE_CASES.md
│   ├── FUNCTIONAL_SCOPE.md
│   ├── NON_FUNCTIONAL_SCOPE.md
│   ├── MVP_DEFINITION.md
│   └── FUTURE_VISION.md
│
├── 02_Architecture/
│   ├── README.md                  (mục lục + quy tắc tham chiếu)
│   └── chapters/
│       ├── 00_overview.md
│       ├── 01_system_context.md
│       ├── 02_core_domain_live_session.md
│       ├── 03_module_boundaries.md
│       ├── 04_data_flow.md
│       ├── 05_runtime_model.md
│       ├── 06_extensibility.md
│       ├── 07_failure_recovery.md
│       └── 08_platform_constraints.md
│
├── 03_UI_UX/
│   ├── README.md
│   ├── LAYOUT.md
│   ├── WORKSPACE.md
│   ├── DOCK_SYSTEM.md
│   ├── TOOLBAR.md
│   ├── INSPECTOR.md
│   ├── THEME.md
│   ├── INTERACTION_PATTERNS.md
│   ├── SHORTCUTS.md
│   ├── NOTIFICATIONS.md
│   └── DIALOGS.md
│
├── 04_Modules/
│   ├── README.md                  (danh sách module + template chuẩn)
│   ├── _MODULE_TEMPLATE.md
│   ├── live_session/MODULE.md
│   ├── scene/MODULE.md
│   ├── ai_host/MODULE.md
│   ├── comment_engine/MODULE.md
│   ├── voice/MODULE.md
│   ├── automation/MODULE.md
│   ├── media/MODULE.md
│   ├── analytics/MODULE.md
│   ├── health_monitor/MODULE.md
│   └── platform_adapter/MODULE.md
│
├── 05_Data/
│   ├── README.md
│   ├── RUNTIME_DATA.md
│   ├── CONFIGURATION.md
│   ├── STATE.md
│   ├── SNAPSHOT.md
│   ├── EVENT.md
│   ├── QUEUE.md
│   └── CACHE.md
│
├── 06_Integrations/
│   ├── README.md
│   ├── _INTEGRATION_TEMPLATE.md
│   ├── obs/INTEGRATION.md
│   ├── tiktok/INTEGRATION.md
│   ├── facebook/INTEGRATION.md
│   ├── youtube/INTEGRATION.md
│   ├── shopee/INTEGRATION.md
│   ├── tts/INTEGRATION.md
│   └── llm/INTEGRATION.md
│
├── 07_Development/
│   ├── README.md
│   ├── CODING_STANDARDS.md
│   ├── NAMING_CONVENTIONS.md
│   ├── FOLDER_RULES.md
│   ├── DEPENDENCY_RULES.md
│   └── REVIEW_CHECKLIST.md
│
├── 08_Testing/
│   ├── README.md
│   ├── UNIT.md
│   ├── INTEGRATION.md
│   ├── UI.md
│   ├── PERFORMANCE.md
│   ├── RECOVERY.md
│   ├── STRESS.md
│   └── LONG_RUNNING.md
│
├── 09_Roadmap/
│   ├── README.md
│   ├── MILESTONES.md
│   ├── RELEASES.md
│   ├── VERSION_STRATEGY.md
│   ├── TECHNICAL_ROADMAP.md
│   └── PRODUCT_ROADMAP.md
│
└── 10_ADR/
    ├── README.md
    ├── _ADR_TEMPLATE.md
    └── ADR-0001-...md, ADR-0002-...md, ...

```

---

## 2. Vai trò từng thư mục

| Thư mục | Trả lời câu hỏi | Đối tượng đọc chính |
|---|---|---|
| `00_Project` | Dự án này tồn tại để làm gì, tin vào điều gì | Toàn team, founder |
| `01_Product` | Sản phẩm làm gì cho ai, phạm vi tới đâu | Product, founder, dev lead |
| `02_Architecture` | Hệ thống được ghép bằng gì, ranh giới ở đâu | Kiến trúc sư, dev senior |
| `03_UI_UX` | Người dùng thao tác qua giao diện nào | UI/UX, dev frontend |
| `04_Modules` | Từng module làm gì, input/output/trách nhiệm | Dev phụ trách module |
| `05_Data` | Dữ liệu sống ở đâu, hình dạng ra sao, vòng đời thế nào | Toàn bộ dev |
| `06_Integrations` | Kết nối với bên thứ ba như thế nào, giới hạn gì | Dev tích hợp |
| `07_Development` | Quy tắc viết code, review, tổ chức thư mục source | Toàn bộ dev |
| `08_Testing` | Kiểm chứng chất lượng bằng cách nào | QA, dev |
| `09_Roadmap` | Khi nào, theo thứ tự nào | Founder, PM |
| `10_ADR` | Vì sao một quyết định kiến trúc được chọn, tại thời điểm nào | Kiến trúc sư tương lai |

---

## 3. Vai trò từng file — theo nhóm

### 00_Project
- **README.md** — bản đồ tài liệu, cách điều hướng docs/. *Chỉ tham chiếu.*
- **VISION.md** — SoT: lý do dự án tồn tại, tầm nhìn dài hạn (10 năm).
- **MISSION.md** — SoT: điều dự án phải làm để đạt Vision.
- **PHILOSOPHY.md** — SoT: các niềm tin nền tảng chi phối quyết định (VD: "Live Session là trung tâm, AI là công cụ phục vụ Live Session, không phải ngược lại").
- **PRINCIPLES.md** — SoT: nguyên tắc ra quyết định khi có mâu thuẫn (VD: ổn định > tính năng mới).
- **GLOSSARY.md** — SoT duy nhất cho **định nghĩa thuật ngữ** dùng xuyên suốt toàn dự án (Scene, Session, Dock...). Mọi file khác dùng thuật ngữ phải khớp định nghĩa ở đây, không được định nghĩa lại.

### 01_Product
- **PRODUCT_VISION.md** — SoT: sản phẩm này là gì trong mắt người dùng (khác với VISION.md ở 00, vốn nói về *dự án*, không phải *sản phẩm*).
- **PRODUCT_GOALS.md** — SoT: mục tiêu đo lường được của sản phẩm.
- **PERSONAS.md** — SoT: chân dung người dùng.
- **USER_JOURNEYS.md** — SoT: hành trình sử dụng, tham chiếu Personas.
- **USE_CASES.md** — SoT: các tình huống sử dụng cụ thể, tham chiếu Journeys.
- **FUNCTIONAL_SCOPE.md** / **NON_FUNCTIONAL_SCOPE.md** — SoT: ranh giới chức năng và phi chức năng (hiệu năng, độ ổn định, khả năng phục hồi...).
- **MVP_DEFINITION.md** — SoT: tập con tối thiểu của Functional Scope cho bản đầu tiên.
- **FUTURE_VISION.md** — *chỉ tham chiếu định hướng*, không cam kết — tránh nhầm với Roadmap (09), vốn có deadline/thứ tự thật.

### 02_Architecture
- **README.md** — mục lục Architecture Bible + **quy tắc tham chiếu giữa chương**: mỗi chương chỉ được tham chiếu chương có số nhỏ hơn hoặc bằng (tránh phụ thuộc vòng), thuật ngữ phải khớp `GLOSSARY.md`.
- `chapters/00–08` — đã viết nội dung thật, Frozen từ Sprint 3 (xem docs/02_Architecture/). Thứ tự chương đã được sắp theo nguyên tắc: bối cảnh → domain lõi (Live Session) → ranh giới module → luồng dữ liệu → runtime → khả năng mở rộng → phục hồi lỗi → giới hạn nền tảng.

### 03_UI_UX
Mỗi file mô tả **cấu trúc và hành vi** của một vùng UI, không mô tả pixel/màu cụ thể (đó là design system, không phải doc kiến trúc). `THEME.md` là SoT cho design tokens ở mức khái niệm; file thiết kế chi tiết (Figma...) chỉ được link, không copy.

### 04_Modules
- **README.md** — danh sách 10 module + bản đồ phụ thuộc giữa chúng (ai gọi ai), không lặp nội dung từng module.
- **_MODULE_TEMPLATE.md** — SoT cấu trúc bắt buộc cho mọi `MODULE.md`, gồm các mục cố định:
  `Purpose → Public Contract → Responsibilities → Boundaries (không làm gì) → Inputs → Outputs → Upstream Dependencies → Downstream Consumers → Configuration State → Runtime State → Invariants → Failure Modes → Recovery Strategy → Extension Points → Related ADRs` (cấu trúc 15 mục, cập nhật tại ADR-0008 sau khi có Architecture Bible, thay thế bản 10 mục ban đầu).
- Mỗi `module/MODULE.md` **chỉ được sở hữu duy nhất** phần mô tả trách nhiệm của module đó. Nếu hai module cần mô tả cùng một luồng dữ liệu → luồng đó thuộc về `05_Data` hoặc `02_Architecture/chapters/04_data_flow.md`, hai module chỉ link tới đó.

### 05_Data
Mỗi file trả lời "loại dữ liệu này sống bao lâu, ai ghi, ai đọc, được persist hay không". Đây là SoT về **vòng đời dữ liệu**, để `04_Modules` không phải lặp lại mô tả state ở từng module.

### 06_Integrations
- **_INTEGRATION_TEMPLATE.md** — SoT cấu trúc: `Purpose → Auth model → Rate limits/constraints → Failure behavior → Data exchanged → Related module → Related ADRs`.
- Mỗi tích hợp **không được mô tả logic nghiệp vụ** (đó là việc của `04_Modules/platform_adapter`), chỉ mô tả đặc tính của bên thứ ba.

### 07_Development — 08_Testing — 09_Roadmap — 10_ADR
Chuẩn hoá quy trình, không chứa quyết định sản phẩm/kiến trúc (những cái đó thuộc 01/02/10).
- **10_ADR/_ADR_TEMPLATE.md** — SoT format: `Context → Decision → Alternatives considered → Consequences → Status (Proposed/Accepted/Superseded)`.
- ADR **bất biến** sau khi Accepted — sửa quyết định = tạo ADR mới, đánh dấu ADR cũ là Superseded.

---

## 4. Source of Truth vs. Chỉ tham chiếu

**SoT (được định nghĩa đúng một lần):**
`GLOSSARY.md`, `VISION.md`, `MISSION.md`, `PHILOSOPHY.md`, `PRINCIPLES.md`, `PRODUCT_VISION.md`, `FUNCTIONAL_SCOPE.md`, `NON_FUNCTIONAL_SCOPE.md`, `MVP_DEFINITION.md`, mỗi `chapters/*.md` trong Architecture, mỗi `MODULE.md`, mỗi file trong `05_Data`, mỗi `INTEGRATION.md`, `CODING_STANDARDS.md`, mỗi `ADR-xxxx.md`.

**Chỉ tham chiếu (tổng hợp/điều hướng, không tạo sự thật mới):**
Mọi `README.md` ở mọi cấp, `FUTURE_VISION.md` (định hướng, không cam kết), `PRODUCT_ROADMAP.md`/`TECHNICAL_ROADMAP.md` (tổng hợp từ Milestones + Architecture, không tự định nghĩa tính năng), `04_Modules/README.md`, `06_Integrations/README.md`.

---

## 5. Thứ tự nên viết

1. `00_Project` (toàn bộ) — nền tư tưởng, bắt buộc trước tiên.
2. `01_Product` — cần Vision/Philosophy để xác định scope đúng.
3. `10_ADR/_ADR_TEMPLATE.md` — chuẩn hoá cách ra quyết định *trước khi* có quyết định kiến trúc đầu tiên.
4. `02_Architecture/README.md` + khung chương (chỉ mục lục, chưa nội dung).
5. `04_Modules/_MODULE_TEMPLATE.md` + `README.md` (danh sách module + phụ thuộc) — cần trước khi viết nội dung Architecture chương 2–3 vì hai bên phải khớp ranh giới.
6. `05_Data/README.md` — khung, để Architecture chương 4 (data flow) có chỗ tham chiếu.
7. `03_UI_UX` — sau khi module đã rõ ranh giới, UI mới có "vật liệu" để mô tả workspace/dock.
8. `06_Integrations` — sau khi `platform_adapter` module đã có khung.
9. `07_Development`, `08_Testing` — song song, sau khi có Coding/Module chuẩn.
10. `09_Roadmap` — cuối cùng, vì roadmap phải phản ánh scope đã chốt, không dẫn dắt scope.

---

## 6. Quyết định kiến trúc đã chốt (nền cho Architecture Bible)

Các điểm này trước đây là "quyết định treo", nay đã chốt để Architecture Bible và `04_Modules` có căn cứ viết ngay, không đoán mò. Mỗi quyết định dưới đây **phải được ghi thành ADR riêng** trong `10_ADR` khi bắt đầu viết Architecture Bible (không chỉ nằm ở đây).

1. **Live Session = state machine.** Domain lõi có trạng thái tường minh: `Idle → Preparing → Live → Paused → Ended` (+ `Error/Recovering`). Mọi module khác subscribe theo state qua Event Bus, không tự suy luận trạng thái. → nền cho `chapters/02_core_domain_live_session.md`.
2. **Ranh giới `automation` vs `ai_host`:**
   - `automation` = engine chạy rule/trigger xác định trước (if X → do Y), **không tự sinh nội dung**.
   - `ai_host` = agent sinh nội dung không xác định trước (lời thoại, phản ứng).
   - Automation chỉ được **gọi** AI Host như một hành động (action) trong rule của nó, không được chứa logic sinh nội dung.
3. **Runtime: multi-process ngay từ đầu** — UI process tách khỏi Engine process (mô hình OBS). Lý do: UI không được đứng hình khi Engine tải nặng (AI inference, encode). Đổi sau này rất đắt cho dự án 10 năm. → nền cho `chapters/05_runtime_model.md`, ảnh hưởng `05_Data` (State/Cache phải qua cơ chế IPC, không shared memory ngầm định).
4. **Event Bus bắt buộc** — mọi module giao tiếp qua event, **không gọi trực tiếp module khác qua function call**. Định nghĩa nằm ở `05_Data/EVENT.md` + `QUEUE.md`, viết **trước** khi bất kỳ `MODULE.md` nào được viết nội dung thật.
5. **Health Monitor** giám sát cả hai domain — kết nối nền tảng (TikTok/FB API...) và tài nguyên máy (CPU/RAM/mạng) — nhưng gộp trong **một module duy nhất** với 2 sub-domain, vì cùng phục vụ một mục đích: cảnh báo sớm nguy cơ gãy Live Session. Không tách module riêng.
6. **Docs versioning:** mỗi ADR gắn header `Applies-to: v0.x`. Không tạo changelog riêng cho docs/ — ADR tự đóng vai trò lịch sử thay đổi kiến trúc.

---

## 7. Phản biện — rủi ro nợ kỹ thuật trong chính cấu trúc được yêu cầu

Hai điểm trong yêu cầu gốc, nếu làm đúng như liệt kê, sẽ tạo trùng lặp:

1. **`01_Product/FUNCTIONAL_SCOPE.md` vs `04_Modules/README.md`** dễ trùng nhau nếu không phân vai rõ: Product Scope liệt kê *tính năng theo góc nhìn người dùng* (VD: "AI tự động trả lời comment"), còn Module Boundaries liệt kê *đơn vị kỹ thuật* (VD: `comment_engine` nhận event, `ai_host` sinh nội dung, `voice` đọc ra). Đề xuất: `FUNCTIONAL_SCOPE.md` không được nêu tên module, chỉ nêu năng lực sản phẩm; bảng ánh xạ "năng lực → module nào phụ trách" đặt trong `04_Modules/README.md`, một chiều duy nhất.
2. **`02_Architecture/chapters/04_data_flow.md` vs `05_Data/EVENT.md`/`QUEUE.md`** có nguy cơ mô tả trùng luồng dữ liệu ở hai nơi. Đề xuất: chương Architecture chỉ mô tả **sơ đồ luồng ở mức hệ thống** (module A → module B qua kênh gì), còn `05_Data` mô tả **hình dạng và vòng đời của dữ liệu** trong từng kênh đó — một bên là "đường đi", một bên là "hàng hoá trên đường đi", không chồng lấn nếu tuân thủ nghiêm.

Nếu không áp đặt ranh giới này ngay từ đầu, đến năm thứ 2–3 của dự án hai cặp tài liệu trên chắc chắn sẽ lệch nhau (một bên cập nhật, bên kia quên) — đây là dạng nợ tài liệu phổ biến nhất trong các dự án sống lâu.

---

## 8. Bước tiếp theo

6 quyết định ở mục 6 đã chốt. Việc còn lại khi mở Architecture Bible: chuyển mỗi quyết định thành một ADR độc lập trong `10_ADR` (ADR-0001 → ADR-0006, theo đúng thứ tự liệt kê ở mục 6), rồi mới viết nội dung thật cho các chương Architecture tương ứng.
