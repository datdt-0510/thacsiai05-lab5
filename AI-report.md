# AI-report — LAB-5

Ghi lại cách dùng AI cho bài này: ra lệnh thế nào cho từng việc, người kiểm ở đâu và bắt được gì,
chỗ nào tin là hiệu quả và chỗ nào chưa.

---

## 1. Đã dùng skill gì, ra lệnh thế nào

### 1.1 Break task — không dùng skill, dùng script

Không có skill nào trong bộ công cụ làm đúng việc "đọc thiết kế chi tiết rồi sinh backlog". Nên
phần này làm bằng tay, có AI hỗ trợ, theo một quy ước: **backlog là dữ liệu, không phải lời gọi API**.

**Nguồn đọc:** `lab4/product-a-management/specs/` — 7 thư mục theo feature cộng `system-design/`.
**Không đọc `requirements/`**: 31 file đó giữ nguyên văn hồ sơ khách, mà repo nộp là public.

**Cách ra lệnh.** Không bảo AI "tạo issue trên GitHub". Bảo nó viết ra hai file:

| File | Vai trò |
|---|---|
| `backlog-data.py` | Toàn bộ backlog dưới dạng dữ liệu Python thuần: `EPICS`, `ISSUES`, `LABELS`, cộng một hàm `body()` dựng thân issue theo khuôn cố định (mục tiêu · phạm vi · định nghĩa xong · phụ thuộc) |
| `create-backlog.py` | Phần gọi `gh`, có 4 lệnh con `--dry-run` / `labels` / `issues` / `project` |

Tách đôi như vậy vì ba lý do cụ thể:

1. **`--dry-run` in cả cây ra màn hình mà không gọi API lần nào.** Đọc 55 item trong terminal rẻ
   hơn nhiều so với tạo 55 issue rồi phát hiện khuôn sai.
2. **Chạy lại được.** `existing_issues()` lấy cả issue đã đóng, tiêu đề trùng thì bỏ qua. Lần chạy
   thứ hai không sinh bản trùng — và đã dùng thật, vì lần đầu đứt giữa chừng.
3. **Sửa nội dung không phải sửa code.** Đổi một dòng định nghĩa xong là sửa dữ liệu, không đụng
   phần gọi API.

**Chia việc máy / việc người:**

| Máy làm | Người làm |
|---|---|
| Soạn tiêu đề và thân issue theo khuôn, từ tên file spec | Quyết **độ hạt**: dừng chia ở đâu, cái gì tách sub-issue |
| Trải đều khuôn cho 55 item không sót mục nào | Quyết issue nào là **ràng buộc đặc thù** và vì sao tách khỏi issue màn |
| Gọi `gh` lặp lại, chờ, bắt lỗi | Quyết phần spec nào **không** thành issue, và viết lý do |

**Một luật tự đặt, ghi thẳng vào đầu `backlog-data.py`:** nội dung issue **tự viết, không chép từ
spec**. Issue trỏ tới spec bằng **tên file**, không dán nội dung. Vừa giữ luật repo public, vừa
tránh có hai bản mô tả lệch nhau.

### 1.2 Chấm SWP — `tkm:sun-estimate`

```
/tkm:sun-estimate https://github.com/users/datdt-0510/projects/2
```

Một dòng. Skill tự đọc Project, tự lọc ra 43 work item lá (bỏ 12 container), chia 5 lô cho 5
worker cô lập, rồi gom lại.

**Vì sao không tự viết skill chấm điểm.** Đã cân nhắc và bỏ, vì phần khó của việc chấm điểm không
phải code mà là **thang đo và kỷ luật áp thang**:

- Ma trận `Scope × Complexity → SWP` là **nhân, không cộng** — độ khó áp lên từng đơn vị khối lượng.
  Tự viết thì rất dễ cộng hai trục rồi ra thang sai ở hai đầu.
- Container (epic, issue cha) **cộng thẳng điểm con**, không tra ma trận lần hai. Tự viết dễ quên,
  và sai ở đây làm tổng phình lên.
- Worker **không có công cụ** — cố ý. Chúng chỉ đọc tiêu đề, thân, nhãn, nên không thể đi lục
  codebase rồi chấm theo thứ nó tưởng tượng.

Viết lại ba thứ đó cho bằng công sức hơn nhiều so với học cách dùng thang đã có.

**Chỗ tôi ra lệnh sai, và nó lộ ra ngay.** Lô đầu tôi đưa worker **đường dẫn file** thay vì nội dung.
Cả 5 worker **từ chối chấm** và báo thiếu dữ liệu — đúng, vì chúng không có công cụ đọc file. Đây
là lỗi của tôi, không phải của chúng, và đáng ghi lại: **mô hình từ chối bịa khi thiếu dữ liệu là
hành vi đúng**, nếu nó chấm bừa thì tôi đã không biết prompt mình hỏng. Dispatch lại với rúbric đầy
đủ và JSON của cả lô **nhúng thẳng trong prompt** thì chạy.

### 1.3 Quy giờ — chọn đường hiệu chuẩn, không ước tay

Hai đường:

| | Đường A — ước tay từng issue | Đường B — hệ số hiệu chuẩn từ LAB-1 |
|---|---|---|
| Cách làm | 43 lần phán đoán "cái này mấy giờ" | Lấy giờ của đúng phạm vi backlog trong dự toán LAB-1 chia cho tổng SWP |
| Nhất quán | Kém — cùng một người, chấm issue thứ 40 khác issue thứ 3 | Một hệ số, áp đều |
| Giải trình | Không so được với LAB-1 vì hai thang khác gốc | So được, vì **cùng một gốc** |

**Chọn đường B.** Nhờ vậy mục 4 của `estimate.md` mới so ngược được với LAB-1 một cách có nghĩa —
chênh 1,0% và chênh đó truy được về đúng hai chỗ làm tròn, không phải chênh về phạm vi.

**Cách ra lệnh.** Không hỏi AI "dự án này mất bao nhiêu giờ" — hỏi thế là nhận một con số không
truy được nguồn. Lệnh chia làm ba bước, mỗi bước buộc phải dẫn nguồn:

1. *"Mở bảng dự toán LAB-1. Liệt kê đúng những dòng thuộc phạm vi backlog — 20 màn và 4 dòng xuyên
   suốt. **Trích số từ file**, không nhớ, không ước."*
2. *"Áp đúng bộ hệ số mà chính LAB-1 đã khai, không tự đặt hệ số mới. In ra từng bước trung gian
   để tôi kiểm."*
3. *"Chia cho tổng SWP. Trước khi tính, hãy nói dải giờ/SWP mà anh cho là hợp lý — **đặt dải trước
   rồi mới tính**, để không tự bẻ dải cho vừa kết quả."*

Bước 3 là chỗ quan trọng nhất: ép khai dải kỳ vọng **trước** khi biết kết quả.

**Chốt tử số bằng cách mở file ra đọc.** Lệnh tự đặt cho mình ở bước này là *"không quyết bằng trí
nhớ"*, và đó không phải lời sáo — xem mục 2.3, hai con số tôi nhớ đều sai.

Mở bảng dự toán của LAB-1, lấy 86,50 người-ngày cho đúng
phạm vi backlog, rồi áp **chính các hệ số của LAB-1** (kiểm thử đơn vị 0,3 · sửa lỗi 0,3 · dự phòng 0,3 ·
trừ phần giảm nhờ AI 0,3) ra **115,30 người-ngày** → × 7 giờ/ngày = 807 giờ → **4,7 giờ/SWP**.

**Kiểm tỉnh táo đặt TRƯỚC khi tính:** dải kỳ vọng 3–8 giờ/SWP. Đặt trước để không tự bẻ dải cho vừa
kết quả. 4,7 nằm trong dải.

### 1.4 Ghi SWP lên trường point bằng Agent

Skill `tkm:sun-estimate` **cố ý không ghi** lên Project — nó read-only với nguồn bên ngoài. Đề cũng
tách hai bước. Nên sau khi người chốt xong 173 SWP, Agent ghi bằng `gh`.

**Chuẩn bị:** lấy id của Project và của từng trường một lần, đặt vào biến.

```bash
PID=PVT_kwHOAhsd5M4BniI3          # Project
SWP_FID=PVTF_lAHOAhsd5M4BniI3zhmHklk   # trường số "SWP"  → đây là trường point
GIO_FID=PVTF_lAHOAhsd5M4BniI3zhmHkmo   # trường số "Giờ ước lượng"
```

**Ghi:** lặp qua bảng đã chốt, mỗi item một lệnh.

```bash
gh project item-edit --project-id "$PID" --id "<ITEM_ID>" \
  --field-id "$SWP_FID" --number <SWP>
gh project item-edit --project-id "$PID" --id "<ITEM_ID>" \
  --field-id "$GIO_FID" --number <GIO>
```

**Trường point chỉ mang SWP**, đúng quy định của đề. Giờ nằm ở trường riêng — nếu nhét giờ vào
point thì mọi phép cộng của Project trả ra con số không còn là điểm.

**Kiểm lại sau khi ghi** — đây mới là phần đáng nói, vì ghi 110 lệnh thì sai một lệnh không ai thấy
bằng mắt. Đọc ngược toàn bộ Project ra JSON rồi đối chiếu bằng script:

```bash
gh project item-list 2 --owner datdt-0510 --format json --limit 300
```

| Kiểm | Kết quả |
|---|---|
| 173 item có `SWP`, 173 item có `Giờ ước lượng` | đủ, 0 thiếu |
| Đối chiếu từng item với bảng đã chốt | 0 lệch |
| Tổng SWP của 7 epic = tổng của 43 lá | 173 = 173 |
| Tổng giờ của 7 epic = tổng của 43 lá | 815,5 = 815,5 |

Phép kiểm cuối bắt được một lỗi thật — xem mục 2.4.

### 1.5 Báo cáo tiến độ — trích số, không gõ số

**Cách ra lệnh.** Lệnh có một mệnh lệnh cấm đi kèm: *"Viết báo cáo tiến độ Sprint 1. **Mọi con số
phải trích từ Project bằng lệnh, không được gõ tay và không được lấy từ phần đã viết ở trên.** Chỗ
nào Project không có dữ liệu thì nói thẳng là không có, đừng dựng số cho đủ bảng."*

Chính vế cấm thứ hai bắt được chỗ hở: bảng burndown cần biết **ngày nào đóng issue nào**, mà Project
chỉ giữ trạng thái hiện tại. Nếu không có lệnh cấm đó, tôi đã viết cột ngày ra như thể nó là số đo.

Sau khi xếp Sprint 1 và cập nhật trạng thái, **không** viết báo cáo từ trí nhớ. Đọc ngược từ Project:

```bash
gh project item-list 2 --owner datdt-0510 --format json --limit 300 > p2.json
```

rồi gom bằng Python: lọc `sprint.title == "Sprint 1"`, bỏ 2 container, nhóm theo `status`, cộng
`sWP` và `giờ ước lượng`. Bảng trạng thái, phân bổ theo người, 59% hoàn thành, 174,5 giờ — tất cả
ra từ đó.

**Một chỗ KHÔNG trích được, và phải nói rõ thay vì để người đọc tưởng.** Cột "ngày nào đóng issue
nào" trong bảng burndown: Project chỉ giữ trạng thái **hiện tại**, không giữ lịch sử đổi trạng thái
theo ngày. Thứ tự ngày đó là tôi **tái dựng theo thứ tự phụ thuộc** giữa các issue. Hai đầu mút của
đường (37 và 15) khớp Project; đường đi giữa hai mút là dựng lại. Đã ghi chú ngay trong
`progress-report.md` mục 2.

Trước khi commit còn chạy lại một lượt nữa để đối chiếu với bản đã viết. Khớp cả 12 con số.

---

## 2. Người review ở đâu, bắt được gì

### 2.1 Sáu chặng

| Chặng | Người kiểm cái gì |
|---|---|
| Dựng backlog | `--dry-run` in cây 55 item, đọc trước khi gọi API |
| Chấm SWP | 5 issue mẫu do bộ chọn xác định rút ra, cộng một lượt đọc toàn bảng 43 dòng |
| Ghi lên Project | Đọc ngược JSON, đối chiếu máy với máy |
| Viết báo cáo | Trích lại số từ Project lần hai, so với bản đã viết |
| Review chéo | Đưa cả bộ nộp cho một mô hình khác đọc với lệnh chỉ tìm lỗi — xem §2.7 |
| Đối chiếu quy trình | So cấu trúc ticket với sơ đồ vòng đời story của khoá — xem §2.8 |

### 2.2 Chấm SWP — kiểm 7 issue, đổi điểm 5, cả 5 đều do AI chấm thấp

Chi tiết từng chỗ ở `estimate.md` §1. Điểm chung đáng rút ra:

**Nhóm bị chấm thấp nhất là nhóm "một luật nhỏ nhưng hỏng thì hỏng nặng".** [#26](https://github.com/datdt-0510/thacsiai05-lab5/issues/26)
và [#27](https://github.com/datdt-0510/thacsiai05-lab5/issues/27) — cấp `許可` chỉ cho `仲卸`,
cấp `承認` chỉ cho `売買参加者` — bị chấm 1 điểm, mức sàn. Đọc thân issue thì đúng là trông như một
phép kiểm loại. Nhưng hồ sơ mời thầu coi việc **gộp hai thứ này thành cùng một quyền** là điều kiện
loại tư cách đề xuất. Việc thật gồm chốt chặn phía máy chủ, thông điệp nêu đúng mã và loại, và bài
kiểm chứng minh cấp nhầm bị từ chối. Nâng lên 2.

**AI nhận đúng độ rộng nhưng đánh giá thấp rủi ro.** [#51](https://github.com/datdt-0510/thacsiai05-lab5/issues/51)
khoá ngày nghiệp vụ: nó chấm `Scope 5` — đúng, hai tầng. Nhưng `Complexity 3`. Trong khi một mục
của định nghĩa xong — *xoá phải bị từ chối chứ không bị huỷ trong im lặng* — là cái bẫy prototype
LAB-3 **đã dính thật** và phải ra một bản vá riêng. Có bằng chứng cụ thể thì đó là `Complexity 5`.

**Khuyến nghị đầu của tôi là bất khả, và ma trận bắt được.** Vòng đầu tôi đề xuất nâng #51 lên **12
điểm**. Áp vào ma trận mới thấy chỉ có **một** ô bằng 12 là `Scope 8 × Complexity 2`, mà `Scope 8`
nghĩa là *"nhiều hệ thống con, nhiều kho, nhiều môi trường"* — sai hẳn với một issue nằm ở hai tầng
trong cùng một kho. Chốt lại **15 = Scope 5 × Complexity 5**: giữ nguyên độ rộng, nâng đúng phần AI
đánh giá thấp.

Đây là chỗ thang đo **cãi lại người**, và nó đúng. Nếu tự viết skill thì tôi đã ghi 12 vào và không
ai biết con số đó không nằm trong thang nào cả.

**Hai chỗ ngoài danh sách mẫu.** Skill chỉ nhận sửa trên 5 issue mẫu. [#46](https://github.com/datdt-0510/thacsiai05-lab5/issues/46)
và [#53](https://github.com/datdt-0510/thacsiai05-lab5/issues/53) là người tự soi ra khi đọc
toàn bảng — #53 bị chấm `Scope 1` chỉ vì thân issue dẫn **một** file spec, trong khi khối lượng
ngang màn danh sách #21 vốn được 3. Đó là **không nhất quán nội bộ**, loại lỗi chỉ thấy khi đọc cả
bảng cạnh nhau, không thấy khi đọc từng dòng.

**Một chi tiết về quy trình:** bước hiệu chuẩn đòi phản hồi cho **cả 5** issue mẫu, kể cả hai cái
tôi giữ nguyên. Lúc đầu thấy phiền. Nghĩ lại thì đúng: *giữ nguyên vì sao* cũng là một quyết định
phải ghi, không phải chỗ trống.

### 2.3 Hai con số tôi nhớ sai

Cả hai chỉ lộ ra vì mở file gốc:

| Tôi nhớ | Thực tế | Hậu quả nếu không kiểm |
|---|---|---|
| Tổng dự toán LAB-1 = 14,14 người-tháng | **31,80** người-tháng (đọc ở sheet lịch sử) | Mục so sánh ngược sai gốc hoàn toàn |
| 1 người-tháng = 160 giờ (20 ngày × 8 giờ) | **140 giờ** — hồ sơ LAB-1 khai định mức **7 giờ/ngày** | Hệ số thành 5,3 giờ/SWP, tổng cao hơn thực tế **14%** |

Chỗ nguy hiểm là cả hai con số đều **trông rất hợp lý**. 160 giờ/tháng là con số ai cũng dùng. Nó
sai ở đây chỉ vì dự án này khai 7 giờ/ngày. Không có cách nào phát hiện bằng cảm giác — chỉ có cách
mở file.

### 2.4 Ghi lên Project — bắt được lỗi cộng dồn

Phép kiểm *"tổng giờ của 7 epic = tổng giờ của 43 lá"* **trượt** ở lần chạy đầu, lệch 3 giờ.

Nguyên nhân: tôi tính giờ của epic bằng `SWP_epic × 4,7` thay vì cộng giờ của các issue con. Mỗi
issue con đã làm tròn tới 0,5 giờ rồi, nên cộng 43 chỗ làm tròn ra khác với nhân một lần. Sửa bằng
cách bỏ hẳn đường nhân cho container: **giờ của epic = tổng giờ con**, không tính lại.

Luật này giờ ghi thẳng vào `estimate.md` §2 để lần sau không lặp lại.

### 2.5 Dựng backlog — `addSubIssue` chạy, không phải lùi về checkbox

Lo trước: GitHub có thể không cho tạo quan hệ cha-con bằng dòng lệnh, phải lùi về danh sách
checkbox trong thân epic — tức là mất quan hệ thật, chỉ còn chữ.

Kết quả: `gh issue edit` **không** có cờ nào cho sub-issue, nhưng GraphQL có:

```graphql
mutation($p:ID!,$c:ID!){ addSubIssue(input:{issueId:$p, subIssueId:$c}){ issue{ number } } }
```

Chạy được cho cả 48 quan hệ. Quan hệ cha-con trên Project là **thật**, không phải checkbox.

Vài chỗ tương tự, cùng một bài học — *dòng lệnh thiếu thì xuống GraphQL xem trước khi chấp nhận
phương án tệ hơn*:

| Chỗ | `gh` có | GraphQL có |
|---|---|---|
| Quan hệ cha-con | `gh issue edit` không có cờ nào cho sub-issue | `addSubIssue` |
| Trường lặp (Sprint) | `gh project field-create --data-type` chỉ nhận `TEXT\|SINGLE_SELECT\|DATE\|NUMBER` | `createProjectV2Field` nhận `ITERATION` |
| Thêm trạng thái `Blocked` | — | `updateProjectV2Field` **thay cả danh sách** lựa chọn, nên phải liệt kê lại đủ 4 cái, không thêm được một cái |

*(Kiểm trên `gh` 2.92.0 — ba dòng trên phụ thuộc phiên bản, bản sau có thể đã bù.)*

### 2.6 Chỗ không sửa được và đã khai ra

Báo cáo HTML mang **170 SWP**, trường point trên Project mang **173**. Chênh 3 điểm là #46 và #53 —
hai chỗ người tự soi ra ngoài cơ chế hiệu chuẩn theo mẫu, mà skill thì read-only nên không ghi
ngược vào báo cáo của nó được.

Đã khai thành một khối chú ý trong `estimate.md` §5 thay vì lặng lẽ để hai con số khác nhau. Chọn
khai vì người chấm sẽ thấy, và thấy mà không có giải thích thì nó thành lỗi.


### 2.7 Đợt review chéo bằng mô hình thứ hai (Codex)

Sau khi coi như xong, đưa cả bộ nộp cộng đề bài cho một mô hình khác đọc, với lệnh **chỉ tìm lỗi,
cấm khen, cấm tóm tắt** và bắt mỗi phát hiện phải trích đúng chữ trong file làm bằng chứng.

**Chỗ làm sai lần đầu — và nó không tốn token nào.** Chạy nền thì `stdin` không phải terminal, mà
`codex exec` lại chờ đọc thêm từ `stdin`, nên nó đứng im ở `Reading additional input from stdin...`
suốt mười phút: `stdout` 0 byte, `stderr` 39 byte. Nhìn qua tưởng "đang đọc bài", thực ra treo.
Sửa bằng `< /dev/null`. Bài học: **im lặng không phải là đang chạy** — phải nhìn kích thước file
đầu ra, không nhìn đồng hồ.

**Kết quả: 12 phát hiện, tôi xác minh lại từng cái, cả 12 đều trỏ vào chỗ có thật.** Không có phát
hiện nào bịa. Bốn cái đáng kể nhất:

| Phát hiện | Vì sao đúng | Đã xử lý |
|---|---|---|
| `AI-report.md` ghi "7 chỗ người sửa điểm", trong đó "AI chấm thấp 5/7" | Đọc lại bảng mẫu: 7 issue **được kiểm**, nhưng #11 và #22 ghi rõ *(giữ)*. Chỉ **5** issue đổi điểm, và cả 5 đều tăng | Tách thành 4 dòng: kiểm 7 · đổi 5 · giữ 2 · hướng đổi 5/5 tăng |
| [#51](https://github.com/datdt-0510/thacsiai05-lab5/issues/51) chốt 15 SWP ≈ 10 ngày công, trái luật độ hạt "1–3 ngày" do chính tôi đặt ở `task-breakdown.md` §1 | Đúng. Và đây là issue **lá**, không phải container, nên luật áp thẳng vào nó | Khai thành ngoại lệ có lý do ở §1, kèm cái giá phải trả và cách chia đúng nếu làm thật |
| Báo cáo khẳng định mọi số trích từ Project, nhưng cột "ngày nào đóng issue nào" thì Project không giữ | Đúng. GitHub Project chỉ có trạng thái **hiện tại**, không có lịch sử theo ngày | Ghi chú thẳng vào `progress-report.md` §2: hai đầu mút khớp Project, đường đi giữa là tái dựng |
| "Phần việc còn lại **phần lớn** không nằm trong tay đội" | Sai tỷ lệ: trong 15 SWP còn lại chỉ **2 SWP** chờ khách, 13 SWP còn lại là việc của đội | Viết lại theo đúng con số |

Tám cái còn lại: thiếu một cột đề đòi ở bảng `Spec → issue`; cột "Xử lý" dùng nhãn ngoài bốn nhãn
đề quy định; cột "Các phương án ở LAB-4" không thật sự nêu phương án nào; cộng cột ra 807,2 trong
khi ô tổng ghi 807,1; "Ba issue yêu cầu phủ định" trong khi bảng chỉ đánh dấu hai; 1,1% còn sót lại
sau khi mục 4 đổi sang 1,0%; gọi 22 SWP là "vận tốc **thật đo được**" trong khi sprint là giả lập;
và viện dẫn một sự cố LAB-3 mà repo nộp không dẫn link được.

**Điều đáng rút ra.** Không lỗi nào trong 12 cái là lỗi *kiến thức* — tất cả đều là **lỗi không
nhất quán giữa các file**, loại sinh ra khi sửa một chỗ mà quên chỗ nhắc lại nó. Chín trong mười
hai cái chỉ lộ ra khi đọc **hai file cạnh nhau**, mà tôi thì viết từng file một. Đó chính là việc
mô hình thứ hai làm tốt hơn người viết: nó không nhớ mình đã định nói gì, nên nó đọc đúng chữ trên
giấy.

Hai lỗi tôi tự tìm ra trước khi Codex trả lời — đơn vị `115,30 người-tháng` lẽ ra là **người-ngày**,
và chuỗi hệ số — đều tìm được bằng cách **tự chạy lại phép tính** thay vì đọc lại câu chữ. Hai cách
soát bắt hai loại lỗi khác nhau; bỏ cách nào cũng sót.


### 2.8 Đối chiếu với quy trình của khoá — và chỗ tôi đi lệch

Khi đã coi là xong, người hướng dẫn đưa sơ đồ *"Vòng đời một Story qua các sub-issue"* để đối chiếu.
Hoá ra tôi hiểu sai **ý nghĩa của sub-issue**.

| | Sơ đồ quy định | Tôi đã dựng |
|---|---|---|
| Đơn vị cha | **Story** — một chức năng người dùng thấy được | Epic theo miền nghiệp vụ |
| Sub-issue là gì | **Giai đoạn vòng đời**: requirement → đặc tả màn → đặc tả API → testcase → coding → testing | **Khối việc** chia nhỏ cùng loại |
| Số lượng | 6 cho mỗi story | 0 với 30/35 issue; 2–3 với 5 issue còn lại |
| Testcase, testing | Mỗi thứ một sub-issue riêng | **Không có issue nào** |

Kiểm bằng số thay vì cảm giác: lọc 55 issue theo `testcase|kiểm thử|testing|tài liệu|đặc tả` →
**không khớp cái nào**. Bài kiểm chỉ nằm thành dòng trong mục "Định nghĩa xong" của 13 issue.

**Chỗ nặng nhất không phải thiếu, mà là tôi đã viết hẳn lý lẽ bác lại.** `task-breakdown.md` của
tôi có câu: *"**Không** tách sub-issue chỉ để chia theo tầng kỹ thuật… làm backlog phồng gấp đôi"*.
Tức là tôi không bỏ sót quy ước — tôi **chọn ngược lại và ghi lý do vào tài liệu**. Một quy ước của
tổ chức thì không phải chỗ để tôi tự quyết khác rồi biện minh; muốn khác thì phải hỏi trước.

**Đã dựng lại:** 20 màn thành **story**, mỗi story đủ 6 sub-issue. Backlog từ 55 lên **173 item**.

Hai quyết định phải chốt trước khi chạy, vì sai là phải làm lại từ đầu:

1. **Story lấy đến đâu.** Chỉ 20 màn. Mười lăm hạng mục kỹ thuật (RLS, lược đồ dữ liệu, 完納奨励金…)
   **không** áp khuôn 6 giai đoạn — chúng không có màn, nên không có "đặc tả màn hình" để sinh
   sub-issue. Chính luật *"mỗi tài liệu một sub-issue"* dẫn tới kết luận đó.
2. **Điểm xử thế nào.** Ba sub-issue giai đoạn 1–2 mang **0 SWP và `Done`**, vì tài liệu của chúng
   chính là LAB-4 đã nộp — việc đã xong, không còn công để ước. Điểm của story chia cho testcase ·
   coding · testing. Nhờ vậy **tổng giữ nguyên 173 SWP / 815,5 giờ**, không phải tính lại hệ số
   4,7 giờ/SWP hay viết lại mục so sánh với LAB-1. Cách chia ban đầu là một công thức cứng
   (`testcase 1 · testing 1 · coding = S−2`); sau đó đã thay bằng cách chia theo bằng chứng —
   xem §2.9.

Kiểm sau khi chạy: 20/20 story có điểm cha **đúng bằng** tổng con, cả SWP lẫn giờ; 0 trường thiếu
trên 173 item.

**Một chỗ vướng và cách gỡ.** [#35](https://github.com/datdt-0510/thacsiai05-lab5/issues/35) SCR-017
đã có sẵn 2 sub-issue, và 6 SWP của nó **chính là tổng hai cái đó** — màn tự nó không mang điểm.
Thêm 6 sub-issue vòng đời nữa thì hoặc đội tổng lên 179, hoặc bỏ đói testcase và testing. Đã gộp
nội dung hai issue đó thành hai dòng trong "Định nghĩa xong" của *Coding — SCR-017* rồi xoá chúng.
Giữ được cả luật nghiệp vụ lẫn tổng điểm.

**Một cái bẫy đáng ghi.** Bộ nạp Project báo *"thêm mới 0 · đã có sẵn 120"* rồi dừng, trong khi cả
120 item **không có trường nào**. Nguyên nhân: Project bật workflow `Auto-add sub-issues to project`,
nên `addSubIssue` vừa chạy là item tự vào board, và điều kiện "đã có trên board thì bỏ qua" của tôi
hiểu nhầm thành "đã xử lý xong". Bài học: **sự hiện diện trên board không phải dấu hiệu đã xử lý** —
phải kiểm chính giá trị trường.


### 2.9 Chạy lại skill trên backlog mới — và chỗ khuyến nghị của tôi sai

Sau khi dựng lại theo vòng đời story, tôi khuyến nghị **chạy lại skill chấm toàn bộ 142 lá**, vì đề
viết *"skill chấm từng issue"* mà 120 sub-issue mới thì chưa qua lần chấm nào. Chạy được hai lô thì
phải dừng, và dừng là đúng.

**Phát hiện 1 — hai worker chấm cùng một khuôn ra hai điểm khác nhau.** Các sub-issue vòng đời sinh
từ đúng một khuôn, thân giống hệt nhau chỉ khác mã màn. Vậy mà:

| Khuôn | Lô 1 | Lô 2 |
|---|---|---|
| Tạo tài liệu Requirement | S2·C0 → **2** | S1·C0 → **1** |
| Tạo tài liệu API Design | S2·C1 → **3** | S1·C0 → **1** |
| Testing | S2·C1 → **3** | S2·C0 → **2** |

Số học của cả hai lô đều đúng ma trận, **0 lỗi** — cái lệch nằm ở phán đoán dải. Nguyên nhân: thân
issue sinh từ khuôn **không có gì để bám vào**, nên worker phải đoán. Đây là thứ chỉ lộ ra khi
backlog có nhiều issue giống hệt nhau; bản chấm cũ (43 item, mỗi cái một nội dung riêng) không có
cách nào phát hiện.

**Phát hiện 2 — tổng phình gấp ~2,3 lần.** Hai lô đầu cho trung bình 2,79 điểm/issue, suy ra 142 lá
≈ **397 SWP** so với 173 hiện tại. Phần lớn là điểm chấm cho **60 issue tài liệu mà LAB-4 đã giao
xong**: skill chấm *việc được mô tả*, nó không có cách nào biết tài liệu ấy đã nằm trong bản nộp.

**Vì sao không thể vừa chạy lại vừa giữ tổng.** Tôi đo thử trên hai màn, cho worker đọc cả ticket
cha. Kết quả: thứ tự thì đúng tuyệt đối — **6/6 giai đoạn** của màn nặng đều cao hơn màn nhẹ, và
`Coding — SCR-017` được 15 điểm vì worker đọc ra hai luật tương tranh từ cha. Nhưng tổng con so với
điểm cha phình **5,3 lần** (SCR-003) và **6,0 lần** (SCR-017).

Lý do nằm ở bản chất thang đo: **ma trận đo "khối việc này to đến đâu", không đo "phần này chiếm
bao nhiêu của cha"**. Hỏi sáu câu riêng về cùng một màn là đếm màn đó sáu lần. Thang SWP thiết kế
cho work item **độc lập**; sáu giai đoạn vòng đời của một màn thì **không độc lập**.

Và chuẩn hoá ngược về điểm cha cũng không cứu được hết: sàn của ma trận là 1 điểm, sáu giai đoạn
cần tối thiểu 6, mà story nhẹ nhất chỉ có 3. **Một màn 3 SWP không chia nổi cho 6 giai đoạn** nếu
mọi giai đoạn đều phải mang điểm khác 0. Đây mới là lý do cứng khiến ba giai đoạn tài liệu phải
mang 0 — lúc đầu tôi chỉ lý giải bằng "LAB-4 đã giao xong".

**Kết luận: yêu cầu "skill chấm từng issue" và cấu trúc "6 sub-issue sinh từ khuôn mỗi màn" mâu
thuẫn nhau.** Chấm từng cái thì lệch nhau và phình tổng; chấm theo khuôn thì không phải chấm từng cái.

**Phần giữ lại được.** Ý tưởng cho worker đọc ticket cha thì đúng và đã dùng thật: thay vì công
thức cứng, mỗi màn được skill chấm ba giai đoạn thực thi **kèm bối cảnh cha**, lấy kết quả làm
trọng số tương đối rồi quy về đúng điểm story. **5/20 màn đổi** so với công thức cứng, và đổi đúng
chỗ có đặc thù — chi tiết ở [`estimate.md`](estimate.md) §2.

**Điều rút ra về cách tôi tư vấn.** Khi khuyến nghị chạy lại, tôi tính đến việc điểm sẽ đổi nhưng
**không lường rằng 60 issue tài liệu sẽ bị chấm như việc chưa làm** — dù chính tôi đặt chúng thành
0 điểm. Lẽ ra phải thấy trước. Thứ cứu được tình huống không phải trí nhớ mà là **dừng lại sau hai
lô để đo, thay vì chạy hết năm lô rồi mới nhìn tổng**.

---

## 3. Điểm đặc biệt

### 3.1 Tin là hiệu quả

**Backlog là dữ liệu, không phải lời gọi API.** Tách `backlog-data.py` khỏi `create-backlog.py` trả
công ngay trong bài: `--dry-run` cho đọc cả cây trước khi tạo gì, và khi lần chạy đầu đứt giữa
chừng thì chạy lại không sinh bản trùng. Làm thật với backlog vài trăm item thì khác biệt còn lớn hơn.

**Dùng thang đo có sẵn thay vì tự viết.** Giá trị không nằm ở chỗ đỡ code. Nó nằm ở chỗ **thang cãi
lại được người**: khuyến nghị 12 điểm của tôi bị chính ma trận bác bỏ vì không ô nào hợp lý cho ra
12. Thang tự viết thì chiều theo người viết.

**Đặt dải kỳ vọng trước khi tính.** Viết "3–8 giờ/SWP" xuống trước, rồi mới tính ra 4,7. Nếu tính
trước thì dải nào cũng vừa.

**Mô hình từ chối bịa là tín hiệu tốt.** 5 worker đồng loạt báo thiếu dữ liệu thay vì chấm bừa —
nhờ đó tôi biết prompt mình hỏng. Nếu chúng chấm bừa, tôi đã nhận 43 con số trông hợp lý và không
có cách nào biết.

### 3.2 Chưa tốt — làm thật sẽ phải khác

**Điểm chấm thực ra là chấm chất lượng viết issue, không phải độ khó công việc.** Worker cố ý không
có công cụ, chỉ đọc tiêu đề + thân + nhãn. Nên #53 bị `Scope 1` vì thân issue dẫn một file spec,
chứ không phải vì màn đó nhỏ. Nói cách khác: **viết issue sơ sài thì được điểm thấp, và điểm thấp
đó trông y hệt điểm thấp thật.** Trong bài này tôi bù bằng một lượt đọc toàn bảng. Với backlog 300
item thì cách đó không mở rộng được, mà tôi chưa có cách nào thay.

**Đối chiếu ngược với LAB-1 gần khớp quá, và điều đó không hoàn toàn là tin tốt.** Chênh 1,0%. Nghe
thì đẹp, nhưng hệ số 4,7 **dẫn xuất từ chính LAB-1**, nên hai con số gần nhau là chuyện đương nhiên,
không phải bằng chứng ước lượng đúng. Thông tin thật nằm ở chỗ **phân bổ lệch** — ví dụ `BR-INC-01`
được LAB-5 cho 11 SWP ≈ 52 giờ trong khi LAB-1 không có dòng riêng cho nó. Lần sau nên trình bày
phần phân bổ trước, tổng sau, để không ai đọc nhầm "khớp tổng" thành "ước đúng".

**Sprint là giả lập.** Mọi trạng thái `Done` / `Blocked` do tôi đặt. Tôi cố nối nguyên nhân trượt về
chỗ hở spec có thật trong LAB-4 để nó không thành bịa, nhưng vận tốc 22 SWP/sprint vẫn là con số
chưa đo được. Dự phóng "7 sprint nữa" phải đọc như một phép tính, không phải một lời hứa.

**Ghi hàng trăm lệnh `item-edit` là cách làm thô.** Chạy được vì backlog cỡ này. Nếu 500 item thì phải gom thành
mutation hàng loạt, và phải có đường chạy lại giữa chừng — thứ `create-backlog.py` có mà bước ghi
điểm thì không.

### 3.3 Chưa nghĩ ra giải pháp

**Làm sao kiểm được hệ số 4,7 giờ/SWP là đúng.** Toàn bộ phần quy giờ dựng trên dự toán LAB-1. Nó
**nhất quán bên trong** — cùng gốc, áp đều, truy được từng bước. Nhưng không có một giờ công thật
nào để đối chiếu. Nếu LAB-1 ước sai thì LAB-5 sai y hệt, và không phép kiểm nào trong bài này phát
hiện ra. Cách duy nhất tôi nghĩ ra là chạy thật vài sprint rồi hiệu chuẩn lại — tức là chưa có cách
nào trong phạm vi bài.

**Làm sao chấm điểm mà không phụ thuộc vào chất lượng viết issue.** Hai đường đều hỏng: cho worker
đọc spec thì phá thế cô lập (và với bài này còn phạm luật repo public, vì spec chứa nguyên văn hồ
sơ khách); không cho đọc thì điểm phản ánh cách viết. Có lẽ đường thứ ba là bắt buộc mỗi issue khai
đủ một số trường cấu trúc trước khi được chấm — nhưng như thế chỉ dời vấn đề sang chỗ khác, vì
người vẫn là người điền các trường đó.

**Làm sao gán người thật vào Project công khai.** GitHub chỉ gán được tài khoản có thật, mà đề đòi
"người phụ trách giả định" và luật dữ liệu cá nhân cấm dùng tên người thật. Tôi lách bằng một
trường chữ riêng tên *Người phụ trách (giả định)*, điền cho **cả 173 item** bằng năm tên hư cấu đặt
theo kiểu họ Việt cộng từ chỉ vai trò — *Nguyễn Văn Dev*, *Trần Thị Dev*, *Nguyễn Thị QA*,
*Lê Văn BrSE*, *Phạm Thị PM* — để nhìn là biết tên giả, không trùng người thật. Nó hiển thị được,
lọc được, nhưng **không phải** trường assignee nên mọi tính năng dựa trên assignee đều không dùng
được: không lọc "việc của tôi", không đếm tải theo người bằng công cụ sẵn có, không nhận thông báo.
Chưa có cách nào vừa đúng
luật vừa giữ được tính năng.

---

## 4. Số liệu bài làm

| | |
|---|---|
| Item trên Project | **173** — 7 epic · 35 issue (20 story màn hình) · 131 sub-issue |
| Quan hệ cha-con tạo bằng GraphQL | **166** |
| Work item mang điểm | **142 lá** (31 container cộng dồn từ con) |
| Tổng ước lượng | 173 SWP · 815,5 giờ · 5,83 người-tháng |
| Issue người kiểm lại điểm | **7** — 5 mẫu do skill chọn + 2 tự soi ngoài mẫu |
| Trong đó **đổi điểm** | **5** — #26, #27, #51, #46, #53 |
| Trong đó **giữ nguyên, có ghi lý do giữ** | 2 — #11, #22 |
| Hướng đổi | **5/5 đều tăng** — AI chấm thấp hơn người ở cả 5 chỗ, không chỗ nào chấm cao |
| Lỗi do người nhớ sai, bắt được nhờ mở file | 2 |
| Lỗi logic bắt được nhờ phép kiểm tổng | 1 (giờ container lệch 3 giờ) |
| File nộp | 6 markdown + 1 báo cáo HTML (207 KB, tự chứa) |

---

## Lịch sử sửa đổi

| Ngày | Nội dung |
|---|---|
| 2026-10-18 | Viết sau khi xong 7 đợt |
| 2026-10-18 | Điền người phụ trách giả định cho cả 55 item bằng tên hư cấu |
| 2026-10-18 | Sửa 7→5 chỗ đổi điểm · 115,30 là người-ngày không phải người-tháng · 1,1%→1,0% · thêm §2.7 đợt review chéo bằng Codex |
