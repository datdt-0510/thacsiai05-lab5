# Break task — từ detail design LAB-4 thành backlog

**Nguồn:** bộ thiết kế LAB-4 đã nộp, cộng/trừ những gì khai ở [`PREMISE.md`](PREMISE.md).
**Kết quả:** 7 epic · 35 issue (trong đó **20 là story màn hình**) · 131 sub-issue = **173 item** trên
[GitHub Project](https://github.com/users/datdt-0510/projects/2).

---

## 1. Nguyên tắc chia task

### Độ hạt — chia tới đâu thì dừng

Dừng khi **một người làm xong trong 1–3 ngày** và **kiểm được bằng một câu**. Hai điều kiện, phải
đủ cả hai: việc 1 ngày mà không nói được "xong là khi nào" thì vẫn chưa đủ nhỏ.

Hệ quả thực tế: một màn hình thường là **một issue**, không phải một epic. Màn phức tạp thì tách
sub-issue, không phình issue.

**Một ngoại lệ, khai thẳng ra.** [#51](https://github.com/datdt-0510/thacsiai05-lab5/issues/51) khoá ngày nghiệp vụ được chốt **15 SWP ≈ 70 giờ**,
tức khoảng 10 ngày công — vượt hẳn luật 1–3 ngày ở trên. Phát hiện khi tự soát lại backlog.

Giữ nguyên, không tách, vì tách là **hỏng chính thứ issue này tồn tại để bảo đảm**: luật nằm ở hai
tầng (trigger dưới database, kiểm ở tầng API), tách đôi thì một nửa đóng được trong khi nửa kia
chưa xong, và lúc đó ngày đã khoá vẫn ghi được qua tầng còn hở. Đây đúng là lỗ hổng mà #51 sinh ra
để bịt. Đánh đổi: issue này sẽ khó theo dõi hơn các issue khác, và nếu làm thật thì nên chia theo
**bài kiểm** chứ không theo tầng — mỗi bài kiểm một sub-issue, cả hai tầng vẫn phải xanh cùng lúc.

Ngoài #51, issue lá nặng nhất là 6 SWP ≈ 4 ngày; phần lớn nằm ở 3–4 SWP ≈ 2–3 ngày, đúng luật.

### Hai kiểu tách sub-issue

Backlog này dùng **hai kiểu cho hai loại việc khác nhau**. Lẫn chúng vào nhau là chỗ dễ sai nhất.

#### Kiểu 1 — story màn hình: tách theo giai đoạn vòng đời

Mỗi màn là một **story**, dưới nó đủ **sáu** sub-issue theo đúng quy trình quản lý yêu cầu:

| Giai đoạn | Sub-issue | Đầu ra |
|---|---|---|
| 1 · Phân tích yêu cầu | Tạo tài liệu Requirement | Tài liệu yêu cầu được duyệt |
| 2 · Đặc tả yêu cầu | Tạo tài liệu Screen Specification | Đặc tả màn hình được duyệt |
| 2 · Đặc tả yêu cầu | Tạo tài liệu API Design | Đặc tả API được duyệt |
| 3 · Thực thi | Tạo testcase | Bộ testcase |
| 3 · Thực thi | Coding | Mã nguồn |
| 3 · Thực thi | Testing | Kết quả kiểm thử |

**Mỗi tài liệu một sub-issue riêng.** Testcase, coding và testing bám vào đúng cặp đặc tả của màn đó.

Ba sub-issue giai đoạn 1–2 mang **0 SWP và trạng thái `Done`**: tài liệu của chúng **chính là bản
thiết kế chi tiết LAB-4 đã nộp**, việc đã làm xong nên không còn công để ước. Chúng vẫn tồn tại
thành issue để vòng đời đọc được đủ sáu bước, chứ không phải để tính điểm. Toàn bộ điểm của story
nằm ở ba sub-issue giai đoạn 3.

#### Kiểu 2 — hạng mục kỹ thuật và ràng buộc nghiệp vụ: tách theo khối việc

Tách khi issue rơi vào một trong hai:

- **Đụng từ hai tầng trở lên** — giao diện + API + database. Ví dụ [#10](https://github.com/datdt-0510/thacsiai05-lab5/issues/10) dựng 17 bảng tách theo ba nhóm bảng.
- **Có nhánh luật riêng cần kiểm riêng** — ví dụ [#25](https://github.com/datdt-0510/thacsiai05-lab5/issues/25) tách 許可 và 承認 thành ba nhánh: cấp 許可, cấp 承認, và kiểm phân nhánh lúc giao dịch.

#### Vì sao không áp một kiểu cho tất cả

**Không áp kiểu 1 cho hạng mục kỹ thuật.** "Tạo tài liệu Screen Specification" vô nghĩa với một
issue như *Bật RLS deny-all trên 17 bảng* — nó không có màn nào. Chính luật của quy trình nói *mỗi
**tài liệu** một sub-issue*, nên không có tài liệu thì không sinh sub-issue. Áp máy móc sẽ đẻ ra
hàng chục issue rỗng và burndown đầy mảnh vụn.

**Không áp kiểu 2 cho story.** Chia một màn theo khối việc thì mất dấu vết giai đoạn: nhìn board
không biết màn đó đã có đặc tả chưa, đã có testcase chưa, hay chỉ mới code xong. Vòng đời sáu bước
làm được đúng việc đó.

### Định nghĩa xong

Mỗi issue có mục "Định nghĩa xong" dạng danh sách kiểm được. Quy ước theo loại:

| Loại issue | Xong nghĩa là |
|---|---|
| Màn hình | Trường · kiểm tra nhập liệu · trạng thái màn · phân quyền hiển thị đều khớp screen spec |
| API | Tham số, mã lỗi và mã HTTP khớp tài liệu API; mã lỗi tra được ở bảng mã lỗi dùng chung |
| Luật nghiệp vụ | Có bài kiểm chạy được cho từng nhánh, kể cả ca biên |
| **Yêu cầu phủ định** | Có bài kiểm **quét mã nguồn** xác nhận đường đó **không tồn tại** |

Dòng cuối là loại khác hẳn. Hai issue trong backlog nghiệm thu bằng *sự vắng mặt* của tính năng
(xem §3) — không viết định nghĩa xong theo kiểu "dựng được X", vì không có X nào để dựng.

### Epic → issue → sub-issue trên Project

Dùng **quan hệ cha–con thật của GitHub** (`addSubIssue`), không dùng ô đánh dấu trong thân issue.
Lý do: ô đánh dấu không đồng bộ với trạng thái issue con, nên tiến độ đọc trên Project sẽ sai.

- **Epic** — issue gắn nhãn `epic`, tiêu đề mở đầu `[EPIC]`. Không tự làm được, chỉ gom.
- **Story / issue** — con trực tiếp của epic. Hai mươi màn hình là **story**, mỗi story có đủ sáu
  sub-issue vòng đời. Phần còn lại là hạng mục kỹ thuật và ràng buộc nghiệp vụ, chỉ tách sub-issue
  khi cần (kiểu 2 ở trên).
- **Sub-issue** — con của story hoặc của issue. Chấm SWP riêng; điểm của cha là **tổng** của con,
  nên story không mang điểm của riêng nó.

Đã thử quan hệ cha–con trên một cặp issue nháp trước khi tạo hàng loạt, xác nhận giao diện hiện
đúng mục Sub-issues rồi mới chạy.

---

## 2. Spec → issue

| Spec LAB-4 | Epic | Issue / sub-issue | Phần spec của dòng này **chưa** thành issue, và vì sao |
|---|---|---|---|
| 8 ADR · `database-design/` · `architecture-design/` | [#3](https://github.com/datdt-0510/thacsiai05-lab5/issues/3) Nền tảng | #10 (+3 con) · #14 · #15 · #16 (+2 con) · #19 · #20 | Bảng `notification_logs` — có trong lược đồ, dựng trong #13, nhưng **không issue nào ghi vào** vì thuộc F-025 ngoài phạm vi |
| `SCR-003/005/006/008` · 8 endpoint · `PARTY_DB` | [#4](https://github.com/datdt-0510/thacsiai05-lab5/issues/4) Tư cách tham gia | #21 · #22 · #23 · #24 · #25 (+3 con) · #29 | `SCR-004/007/009/010/011` — thuộc chức năng LAB-4 khai ngoài phạm vi, không có detail design để break |
| `SCR-012/013/015` · 3 endpoint · `LOT_DB` | [#5](https://github.com/datdt-0510/thacsiai05-lab5/issues/5) Tiếp nhận lô | #30 · #31 · #32 · #33 | `SCR-014` (vòng đời trạng thái lô) — ngoài phạm vi |
| `SCR-016/017/018` · 2 endpoint · `TRADE_DB` · `TRADE_CREATE_TECH` | [#6](https://github.com/datdt-0510/thacsiai05-lab5/issues/6) Giao dịch | #34 · #35 (+2 con) · #38 · #39 · #40 | `SCR-019/020/021` ngoài phạm vi · `ADR-008` (相対取引 là luồng chính) là **quyết định định hướng**, không sinh việc dựng — đã phản ánh vào nội dung #35 và #38 |
| `SCR-022/023` · 2 endpoint · `DELIVERY_DB` | [#7](https://github.com/datdt-0510/thacsiai05-lab5/issues/7) Giao hàng | #41 · #42 · #43 | `SCR-024/026/027` ngoài phạm vi, trong đó `SCR-026/027` là luồng sửa sau chốt sổ (F-017) — xem `PREMISE.md` §2 |
| `SCR-025/028/030` · 5 endpoint · `SETTLE_DB` · `SETTLE_*_TECH` | [#8](https://github.com/datdt-0510/thacsiai05-lab5/issues/8) Chốt sổ & thưởng | #44 · #45 · #46 · #47 (+3 con) · #51 | `SCR-029/031/032/033/034/035` ngoài phạm vi |
| `SCR-001/002/040/041/042` · 10 endpoint · `ADMIN_DB` | [#9](https://github.com/datdt-0510/thacsiai05-lab5/issues/9) Quản trị | #52 · #53 · #54 · #55 · #56 · #57 | `SCR-043/044/045` ngoài phạm vi · toàn bộ `FG-06` (thông báo, `SCR-036→039`) ngoài phạm vi |

### Phần spec chưa thành issue, và vì sao

| Phần spec | Vì sao chưa thành issue |
|---|---|
| `requirements/` — 16 trong 31 tài liệu yêu cầu | Thuộc các chức năng LAB-4 đã khai là **ngoài phạm vi** (F-004, F-005, F-008, F-011, F-012, F-017, F-020…F-027, F-030, F-031). Có tài liệu yêu cầu nhưng **không có detail design** để break. Break chúng là bịa việc |
| `specs/_templates/` — 7 khuôn tài liệu | Là biểu mẫu, không phải nội dung sản phẩm. Không sinh việc dựng |
| 6 file `*_FUNC.md` Function Specifications | Là bản liệt kê các đường vào của từng module, nội dung đã nằm trong issue màn và issue API tương ứng. Tạo issue riêng là **chép việc hai lần** |
| `ADR-008` — 相対取引 là luồng chính, セリ là luồng phụ | Là quyết định định hướng, không phải việc dựng. Đã phản ánh vào nội dung [#35](https://github.com/datdt-0510/thacsiai05-lab5/issues/35) và [#38](https://github.com/datdt-0510/thacsiai05-lab5/issues/38) |
| Bảng `notification_logs` | Có trong lược đồ nhưng không chức năng nào trong phạm vi ghi vào. Thuộc F-025, ngoài phạm vi. Dựng bảng nằm trong #13, không có issue sinh dữ liệu cho nó |

---

## 3. Hạng mục chịu ràng buộc nghiệp vụ đặc thù

Bảy issue gắn nhãn
[`ràng-buộc-đặc-thù`](https://github.com/datdt-0510/thacsiai05-lab5/issues?q=label%3A%22r%C3%A0ng-bu%E1%BB%99c-%C4%91%E1%BA%B7c-th%C3%B9%22).
Tách riêng khỏi issue màn, không gộp.

| Ràng buộc | Issue | Vì sao tách riêng |
|---|---|---|
| **完納奨励金** — 5 tham số của quy tắc tính thưởng | [#47](https://github.com/datdt-0510/thacsiai05-lab5/issues/47) (+3 con) | Năm tham số độc lập: hệ số thuế, cắt lẻ bằng làm tròn xuống, không ân hạn, dùng ngày nghiệp vụ theo múi giờ quy định, điều chỉnh sinh ở kỳ sau. **Mỗi tham số là một cách sai riêng**, và sai thì sai tiền trả cho khách hàng của chợ. Gộp vào issue màn SCR-028 thì năm luật này biến thành một dòng mô tả |
| **許可 vs 承認** — hai cơ chế độc lập | [#25](https://github.com/datdt-0510/thacsiai05-lab5/issues/25) (+3 con) | Hồ sơ mời thầu coi việc gộp hai thứ này thành **cùng một quyền** là điều kiện loại tư cách đề xuất. Không phải chi tiết kỹ thuật mà là điều kiện dự thầu — phải có issue riêng kiểm được, không thể lẫn vào issue màn |
| **業務日** — khoá ngày nghiệp vụ | [#51](https://github.com/datdt-0510/thacsiai05-lab5/issues/51) | Luật nằm ở **hai nơi**: trigger ở tầng database cho một số bảng, kiểm ở tầng API cho phần còn lại. Một issue màn không kiểm được cả hai tầng |
| **目利き** — phán đoán là việc của người | [#33](https://github.com/datdt-0510/thacsiai05-lab5/issues/33) | *Yêu cầu phủ định.* Nghiệm thu bằng việc **không có** chấm điểm tự động, không gợi ý hạng, không suy luận từ ảnh |
| **セリ** — không nhận dạng ảnh | [#39](https://github.com/datdt-0510/thacsiai05-lab5/issues/39) | *Yêu cầu phủ định.* Nghiệm thu bằng **sự vắng mặt** của tính năng tải ảnh và nhận dạng ký hiệu tay |
| **Lô chưa thẩm định thì không bán được** | [#40](https://github.com/datdt-0510/thacsiai05-lab5/issues/40) | Luật từng chỉ nằm trong hướng dẫn thao tác, đi vòng được. Phải cưỡng chế ở nơi không đi vòng, và phải kiểm theo **bản ghi thẩm định** chứ không theo cột trạng thái — vì cột trạng thái sửa thẳng được |
| **Sổ giao hàng cộng theo hướng** | [#43](https://github.com/datdt-0510/thacsiai05-lab5/issues/43) | Trả lại là hàng **quay về** nên trừ vào lượng đã giao; huỷ một phần giảm nghĩa vụ chứ không đụng lượng đã giao. Cộng dồn mọi dòng vừa sai số liệu vừa **chặn nhầm nghiệp vụ có thật** (ghi trả lại sau khi đã giao đủ) |

Hai issue đánh dấu *yêu cầu phủ định* ([#33](https://github.com/datdt-0510/thacsiai05-lab5/issues/33) và [#39](https://github.com/datdt-0510/thacsiai05-lab5/issues/39)) có cách nghiệm thu khác hẳn phần còn lại của backlog: xong
nghĩa là chứng minh được một thứ **không tồn tại**. Định nghĩa xong của chúng là bài kiểm quét mã
nguồn, không phải bài kiểm chạy tính năng.

**Việc tách riêng này đo được.** So theo epic với dự toán LAB-1 ([`estimate.md`](estimate.md) §4)
thì hai epic **tăng** giờ lại đúng là hai epic mang ràng buộc đặc thù — `完納奨励金` ở epic #8
(**+59%**) và `許可` vs `承認` ở epic #4 (**+45%**) — trong khi năm epic còn lại đều giảm. Dự toán
đếm theo số màn không thấy được khối việc đó, vì nó đếm `SCR-028` ngang một form thường.

---

## 4. Chỗ hở và mâu thuẫn của spec LAB-4 phát hiện khi break

| Spec | Chỗ hở / mâu thuẫn | Xử lý |
|---|---|---|
| `PARTICIPANTS_ELIGIBLE_GET_API.md` · `PARTY_FUNC.md` | 買出人 (`ACT-04`) nằm trong danh sách bên mua nhưng **không qua kiểm tư cách nào**. Không rõ cố ý hay bỏ sót | **Tạo issue làm rõ** [#29](https://github.com/datdt-0510/thacsiai05-lab5/issues/29) — cần khách trả lời |
| `SETTLE_INCENTIVE_TECH.md` §4.4 | Tính lại **ghi đè** bản ghi kỳ hiện tại, trái tham số thứ năm của quy tắc tính thưởng (điều chỉnh sinh ở kỳ sau) | **Tạo issue** [#49](https://github.com/datdt-0510/thacsiai05-lab5/issues/49) — trỏ [`PREMISE.md`](PREMISE.md) §1: LAB-4 tự khai đây là 既知の限界, nên là **việc đã hẹn làm**, không phải thiết kế bị sửa |
| `SETTLE_INCENTIVE_TECH.md` §4.5 | Vết kiểm toán chỉ lưu giá trị **sau**, mà bước trước là ghi đè → không truy được giá trị cũ | **Tạo issue** [#50](https://github.com/datdt-0510/thacsiai05-lab5/issues/50) — cùng lý do, trỏ [`PREMISE.md`](PREMISE.md) §1 |
| `_common/error-codes.md` · `api-conventions.md` | Mã lỗi do tầng database ném ra phải có đủ ba mắt xích: ánh xạ HTTP, câu thông điệp, và bài kiểm đối chiếu cả lớp. Thiếu một mắt xích thì người dùng nhận lỗi hệ thống chung chung | **Tạo issue** [#57](https://github.com/datdt-0510/thacsiai05-lab5/issues/57) — chỗ hở tự phát hiện, tự sửa được nên **không** phải issue làm rõ |
| Hồ sơ LAB-1 (`Effort Estimation` ↔ `Q&A List`) | **Tự mâu thuẫn**: `ASM-04` được dẫn là §07-03 ở file này, §07-04 ở file kia, cùng một chủ đề | **Để mở** — chủ đề này thuộc luồng F-017 nằm ngoài phạm vi backlog, chưa sinh việc. Đã ghi thành giả định ở [`PREMISE.md`](PREMISE.md) §2, cần khách xác nhận trước khi làm F-017 |

Bốn dòng đầu thành issue thật; dòng cuối để mở vì sửa nó bây giờ là quyết hộ khách một chuyện chưa
sinh việc trong phạm vi này.

**Đối chiếu với bốn cách xử lý mà đề quy định:** dòng 1 là *tạo issue làm rõ (#29)*; dòng 5 là *để
mở và vì sao*, đồng thời đã khai thành *giả định* ở [`PREMISE.md`](PREMISE.md) §2. Ba dòng giữa
không rơi gọn vào ô nào trong bốn ô: chúng **không** phải thiết kế bị sửa (`PREMISE.md` §1 để trống
có chủ ý), cũng **không** phải câu hỏi cho khách. Chúng là chỗ hở tự phát hiện rồi tự sửa được, nên
tôi ghi thẳng là *tạo issue (#số)* kèm con trỏ sang dòng `PREMISE.md` giải thích, thay vì gán bừa
vào một nhãn không đúng.

---

## Lịch sử sửa đổi

| Ngày | Nội dung |
|---|---|
| 2026-10-21 | Dựng lại theo vòng đời story: mỗi màn 6 sub-issue, 55 → 173 item |
| 2026-10-18 | Break lần đầu: 7 epic · 35 issue · 13 sub-issue |
| 2026-10-18 | Thêm cột "phần spec chưa thành issue" vào mục 2 · đối chiếu cột Xử lý với 4 nhãn đề quy định · khai #51 là ngoại lệ độ hạt · sửa "ba" thành "hai" yêu cầu phủ định |
