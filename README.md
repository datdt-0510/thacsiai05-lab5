# Product A — Backlog và quản lý tiến độ trên GitHub Project

| | |
|---|---|
| **Dự án** | Product A — Hệ thống hỗ trợ vận hành chợ đầu mối thủy sản |
| **Mã học viên** | `DatDT_ThacSiAI05` |
| **Đề** | LAB-5 — Break task, ước lượng SWP, quản lý tiến độ |

---

## 1. Tổng quan

Backlog này break từ **bản thiết kế chi tiết LAB-4 nộp ngày 2026-10-09** (`product-a-management/`,
133 file tài liệu). Bản đó đã qua hai lượt sửa sau review — 23 chỗ ở đợt review chéo bằng mô hình
thứ hai, 4 chỗ cập nhật sau khi prototype LAB-3 chuyển sang tiếng Nhật — nên những chỗ đó **không**
tính là khác biệt ở đây.

| | |
|---|---|
| **Thay đổi thiết kế so với LAB-4** | **Không có.** Break hết 20 màn, 30 endpoint, 17 bảng và 8 ADR mà không chỗ nào buộc phải sửa thiết kế. Chi tiết ở [`PREMISE.md`](PREMISE.md) §1 |
| **Giả định bổ sung** | 4, khai ở [`PREMISE.md`](PREMISE.md) §2. Một trong đó là **giả định sai đã tự sửa** — xem `estimate.md` §4 |
| **Chỗ không được phép đoán** | 1 — tư cách của 買出人 (`ACT-04`). LAB-4 ghi thẳng là phải hỏi khách, nên thành issue [#29](https://github.com/datdt-0510/thacsiai05-lab5/issues/29) gắn nhãn `cần-khách-trả-lời` chứ không thành giả định |
| **Quy mô backlog** | **7 epic · 35 issue · 131 sub-issue = 173 item** trên Project. Hai mươi màn hình là **story**, mỗi story có đủ 6 sub-issue theo vòng đời (requirement → đặc tả màn → đặc tả API → testcase → coding → testing) |
| **Tổng ước lượng** | **173 SWP ≈ 815,5 giờ ≈ 5,83 người-tháng** (hệ số 4,7 giờ/SWP, dẫn xuất từ dự toán LAB-1). *SWP = Standard Work Point — điểm công chuẩn, chấm theo ma trận `Scope × Complexity`; xem [`estimate.md`](estimate.md)* |
| **Sprint đã chạy** | `Sprint 1` — 2026-10-05 → 2026-10-18, giả lập. Hoàn thành 22/37 SWP = 59%, có 2 issue `Blocked` với nguyên nhân nối về chỗ hở spec có thật. Báo cáo ở [`progress-report.md`](progress-report.md) |

Epic thứ 7 — **Nền tảng** — là thứ LAB-4 không có: ADR, thiết kế cơ sở dữ liệu và tài liệu kiến trúc
nằm rải trong `system-design/`, không thuộc feature nào. Gom chúng thành epic riêng vì không có
lược đồ dữ liệu, cổng phiên, ma trận quyền và lớp từ điển thì không màn nào dựng được.

---

## 2. Link

| | |
|---|---|
| **Repo** | https://github.com/datdt-0510/thacsiai05-lab5 |
| **GitHub Project** | https://github.com/users/datdt-0510/projects/2 |
| **Tiến độ Sprint 1** | Ghim ngay đầu trang Project dưới dạng *status update* — trạng thái `At risk`, khoảng 2026-10-05 → 2026-10-18, kèm hai chỗ chặn |

### View trên Project

| View | Xem được gì |
|---|---|
| [#2 Sprint 1 — theo trạng thái](https://github.com/users/datdt-0510/projects/2/views/2) | Bảng thẻ, lọc `sprint:"Sprint 1"` — 12 item của sprint xếp theo trạng thái. **Đây là chỗ xem tiến độ sprint** |
| [#5 Tiến độ theo sprint](https://github.com/users/datdt-0510/projects/2/views/5) | Bảng, lọc `-no:sprint` — mọi item đã xếp sprint, kèm cột Sprint, Trạng thái, SWP, Giờ. Sprint sau thêm vào là tự hiện |
| [#3 Backlog theo epic](https://github.com/users/datdt-0510/projects/2/views/3) | Toàn bộ backlog kèm nhãn epic |
| [#1 Tất cả item](https://github.com/users/datdt-0510/projects/2/views/1) | Bảng trần, không lọc |

Biểu đồ dựng sẵn ở tab [Insights](https://github.com/users/datdt-0510/projects/2/insights). Lưu ý:
Insights chỉ vẽ từ **trạng thái hiện tại**, GitHub Project không lưu lịch sử trạng thái theo ngày —
nên burndown theo ngày vẫn phải đọc ở [`progress-report.md`](progress-report.md) §2.
| **Báo cáo SWP (HTML)** | [`swp-report/sun-estimate-product-a.html`](swp-report/sun-estimate-product-a.html) — tự chứa, mở thẳng bằng trình duyệt, không cần máy chủ |

### Đã kiểm mở được mà không cần đăng nhập

| Kiểm | Kết quả |
|---|---|
| `GET api.github.com/repos/.../thacsiai05-lab5` không mang chứng danh | `200` · `"private": false` · `"visibility": "public"` |
| `GET github.com/users/datdt-0510/projects/2` không mang cookie | `200`, và **thân trang trả về đúng tên cả 7 epic** — tức là nội dung thật, không phải trang mời đăng nhập |
| `GET raw.githubusercontent.com/.../swp-report/sun-estimate-product-a.html` | `200` |

Trang Project render bằng JavaScript nên mã `200` một mình không chứng minh gì. Bằng chứng thật là
**tên cả 7 epic nằm trong thân trang mà máy chủ trả về cho một client không mang cookie nào** — nếu
Project còn riêng tư thì chỗ đó là trang mời đăng nhập, không phải nội dung.

**Đã mở lại cả hai link bằng cửa sổ ẩn danh và xác nhận bằng mắt:** repo và Project đều xem được mà
không cần đăng nhập.

---

## 3. Cấu trúc repo

Mọi mục ở gốc:

| File / thư mục | Nội dung |
|---|---|
| [`README.md`](README.md) | File này — tổng quan, link, cấu trúc, bảng spec → epic |
| [`PREMISE.md`](PREMISE.md) | Tiền đề so với LAB-4: thay đổi thiết kế (không có), 4 giả định bổ sung, 3 hướng đã chốt ở chỗ LAB-4 để ngỏ |
| [`task-breakdown.md`](task-breakdown.md) | Nguyên tắc chia task và độ hạt · bảng spec → issue · 7 hạng mục chịu ràng buộc nghiệp vụ đặc thù · chỗ hở spec phát hiện khi break |
| [`estimate.md`](estimate.md) | Chấm SWP bằng ma trận Scope × Complexity · cách dẫn hệ số 4,7 giờ/SWP từ dự toán LAB-1 · tổng theo epic · đối chiếu ngược với LAB-1 · cách ghi điểm lên Project |
| [`progress-report.md`](progress-report.md) | Báo cáo Sprint 1: bảng trạng thái · burndown theo ngày kèm giải thích chỗ lệch · bảng rủi ro trễ · dự phóng |
| [`AI-report.md`](AI-report.md) | Đã dùng skill gì và ra lệnh thế nào cho từng việc · người review ở đâu và phát hiện gì · điểm đặc biệt và điểm chưa tốt |
| [`swp-report/`](swp-report/) | Báo cáo HTML do skill `tkm:sun-estimate` sinh ra — bảng chấm từng issue, ma trận SWP, dữ liệu kiểm toán nhúng sẵn. Một file duy nhất 207 KB, không phụ thuộc mạng |
| `.gitignore` | Chặn sẵn `*.xlsx` `*.xls` `*.pdf` `*.docx` và `.env*`. Repo này public nên **không được chứa hồ sơ gốc do khách cấp hay bất kỳ khoá nào** — chặn ở mức công cụ, không trông vào trí nhớ |

**Không có thư mục `skills/`.** Bài này không viết skill mới: việc chấm SWP dùng `tkm:sun-estimate`
đã có sẵn trong bộ công cụ, còn phần break task và dựng backlog làm bằng script Python gọi thẳng
`gh` và GraphQL — những script đó là công cụ thao tác một lần, không phải sản phẩm nộp, nên để ở
repo làm việc riêng chứ không đưa vào đây.

---

## 4. Spec LAB-4 → epic trên Project

| Spec LAB-4 | Epic trên Project | Ghi chú |
|---|---|---|
| 8 ADR · `system-design/database-design/` · `architecture-design/` | [#3](https://github.com/datdt-0510/thacsiai05-lab5/issues/3) **Nền tảng** — 35 SWP | **Không có trong LAB-4 dưới dạng feature** — xem `PREMISE.md` §3. Chứa `業務日` ở mức thiết kế (`ADR-005`, khoá ngày nghiệp vụ bằng trigger) |
| `SCR-003/005/006/008` · 8 endpoint · `PARTY_DB` | [#4](https://github.com/datdt-0510/thacsiai05-lab5/issues/4) **Tư cách tham gia thị trường** — 26 SWP | Chứa **`許可` vs `承認`** ([#25](https://github.com/datdt-0510/thacsiai05-lab5/issues/25) +3 con): hai cơ chế độc lập, gộp thành cùng một quyền là **điều kiện loại tư cách đề xuất** theo hồ sơ mời thầu. Cũng chứa [#29](https://github.com/datdt-0510/thacsiai05-lab5/issues/29) `cần-khách-trả-lời` |
| `SCR-012/013/015` · 3 endpoint · `LOT_DB` | [#5](https://github.com/datdt-0510/thacsiai05-lab5/issues/5) **Tiếp nhận lô và thẩm định** — 16 SWP | Chứa **`目利き`** ([#33](https://github.com/datdt-0510/thacsiai05-lab5/issues/33)) — *yêu cầu phủ định*: nghiệm thu bằng việc **không có** chấm điểm tự động hay suy luận hạng từ ảnh |
| `SCR-016/017/018` · 2 endpoint · `TRADE_DB` · `TRADE_CREATE_TECH` | [#6](https://github.com/datdt-0510/thacsiai05-lab5/issues/6) **Giao dịch — 相対取引 và セリ** — 19 SWP | Chứa **`セリ` không nhận dạng ảnh** ([#39](https://github.com/datdt-0510/thacsiai05-lab5/issues/39), *yêu cầu phủ định*) và **lô chưa thẩm định thì không bán được** ([#40](https://github.com/datdt-0510/thacsiai05-lab5/issues/40)) |
| `SCR-022/023` · 2 endpoint · `DELIVERY_DB` | [#7](https://github.com/datdt-0510/thacsiai05-lab5/issues/7) **Giao hàng và ngoại lệ** — 11 SWP | Chứa **sổ giao hàng cộng theo hướng di chuyển của hàng** ([#43](https://github.com/datdt-0510/thacsiai05-lab5/issues/43)): trả lại trừ vào lượng đã giao, huỷ một phần giảm nghĩa vụ |
| `SCR-025/028/030` · 5 endpoint · `SETTLE_DB` · `SETTLE_*_TECH` | [#8](https://github.com/datdt-0510/thacsiai05-lab5/issues/8) **Chốt sổ và tiền thưởng** — 40 SWP | Epic nặng nhất. Chứa **`完納奨励金` / `BR-INC-01`** ([#47](https://github.com/datdt-0510/thacsiai05-lab5/issues/47) +3 con) — 5 tham số, mỗi tham số là một cách sai riêng, và sai thì sai tiền. Cộng **`業務日`** ở mức cưỡng chế ([#51](https://github.com/datdt-0510/thacsiai05-lab5/issues/51)) |
| `SCR-001/002/040/041/042` · 10 endpoint · `ADMIN_DB` | [#9](https://github.com/datdt-0510/thacsiai05-lab5/issues/9) **Quản trị hệ thống** — 26 SWP | Vết kiểm toán chỉ thêm, không sửa không xoá được từ trong ứng dụng ([#56](https://github.com/datdt-0510/thacsiai05-lab5/issues/56)) |

Bảy issue mang nhãn
[`ràng-buộc-đặc-thù`](https://github.com/datdt-0510/thacsiai05-lab5/issues?q=label%3A%22r%C3%A0ng-bu%E1%BB%99c-%C4%91%E1%BA%B7c-th%C3%B9%22)
được **tách khỏi issue màn**, không gộp. Lý do từng cái ở [`task-breakdown.md`](task-breakdown.md) §3.

Phần spec **chưa** thành issue (16/31 tài liệu yêu cầu, 7 khuôn tài liệu, 6 file `*_FUNC.md`,
`ADR-008`, bảng `notification_logs`) và lý do từng phần: [`task-breakdown.md`](task-breakdown.md) §2.

---

## Lịch sử sửa đổi

| Ngày | Nội dung |
|---|---|
| 2026-10-21 | Cập nhật quy mô backlog sau khi dựng lại theo vòng đời story |
| 2026-10-18 | Khởi tạo bộ nộp LAB-5 |
| 2026-10-19 | Thêm status update Sprint 1 và 2 view trên Project |
