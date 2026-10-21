# Ước lượng — SWP và quy ra giờ

> **SWP = Standard Work Point** — điểm công chuẩn. Mỗi work item nhận một điểm theo **một bảng
> chuẩn cố định**, nên cùng một mô tả công việc luôn ra cùng một điểm. Điểm là đầu ra của hai trục
> **Scope** (độ rộng — đụng bao nhiêu chỗ) và **Complexity** (độ khó), tra ma trận `Scope × Complexity`.
> Hai trục **nhân** chứ không cộng, vì độ khó áp lên từng đơn vị khối lượng. Issue cha không chấm
> trực tiếp: điểm của nó là tổng điểm các con.

**Báo cáo HTML của skill:** [`swp-report/sun-estimate-product-a.html`](swp-report/sun-estimate-product-a.html)

Mở trực tiếp bằng trình duyệt, tự chứa, không cần mạng.

---

## 1. Issue mẫu skill chọn để kiểm

> **Phạm vi lần chấm này — khai trước để không gây hiểu nhầm.** Skill chấm **43 work item** của cấu
> trúc backlog lúc đó. Sau này backlog được dựng lại theo vòng đời story, 20 màn tách thành 120
> sub-issue giai đoạn; **những sub-issue đó không qua một lần chấm ma trận riêng** mà **thừa hưởng
> điểm của chính story cha** theo luật chia ở mục 2. Nói cách khác: điểm gốc vẫn là điểm skill chấm
> và người đã kiểm, phần tách ra chỉ **phân bổ lại trong nội bộ từng story** chứ không sinh thêm
> điểm — nên tổng vẫn đúng 173 SWP.

Skill `tkm:sun-estimate` chấm 43 work item lá bằng 5 worker cô lập — mỗi worker chỉ đọc **tiêu đề,
thân và nhãn issue**, không có công cụ, không đọc được spec LAB-4. Bộ chọn xác định rút ra 5 issue
mẫu theo vai trò: thấp nhất, trung vị dưới, cao nhất, ngoại lai, độ tin cậy thấp nhất.

| Issue | Skill chấm (Scope × Complexity → SWP) | Chốt | Lý do |
|---|---|---|---|
| [#11](https://github.com/datdt-0510/thacsiai05-lab5/issues/11) Nhóm bảng tư cách — 7 bảng | 3 × 1 → **4** | **4** *(giữ)* | 7 bảng trong một lớp dữ liệu, ràng buộc thông thường. Skill tự ghi gap *"không đọc được nội dung spec"* thay vì đoán — đúng luật thiếu thông tin thì hạ độ tin cậy chứ không nâng điểm. Đối chiếu thiết kế LAB-4: 7 bảng này đúng là khoá ngoại và unique thường, không có migration hay backfill |
| [#22](https://github.com/datdt-0510/thacsiai05-lab5/issues/22) SCR-005 tạo/sửa hồ sơ | 3 × 1 → **4** | **4** *(giữ)* | Màn form nối một endpoint, kiểm trường bắt buộc. Ngang các màn form khác nên nhất quán. Skill nêu gap đúng (chỉ thấy POST, chưa thấy đường sửa) nhưng điều đó không đổi dải điểm |
| [#26](https://github.com/datdt-0510/thacsiai05-lab5/issues/26) Cấp 許可 chỉ cho 仲卸 | 1 × 1 → **1** | **2** | RFP §12-06 coi việc gộp 許可 với 承認 là **điều kiện loại tư cách đề xuất**. Việc thật không chỉ là một phép kiểm loại: phải có chốt chặn phía máy chủ, thông điệp nêu đúng mã và loại thực tế, và bài kiểm chứng minh cấp nhầm bị từ chối. Mức sàn 1 điểm dành cho thay đổi hằng số, không dành cho chốt chặn có bài kiểm |
| [#27](https://github.com/datdt-0510/thacsiai05-lab5/issues/27) Cấp 承認 chỉ cho 売買参加者 | 1 × 1 → **1** | **2** | Đối xứng với #26 cho `ACT-03`, cùng stratum nên cùng mức |
| [#51](https://github.com/datdt-0510/thacsiai05-lab5/issues/51) Khoá ngày nghiệp vụ | 5 × 3 → **10** | **15** | Skill nhận **đúng** độ rộng (hai tầng, Scope 5) nhưng đánh giá thấp **rủi ro**. Chốt sổ không có đường hoàn tác, và một mục trong định nghĩa xong — *xoá phải bị từ chối chứ không bị huỷ trong im lặng* — là cái bẫy prototype LAB-3 của tôi đã dính thật và phải ra một bản vá riêng. *(Kho LAB-3 là repo riêng tư nên không dẫn link ở đây được — đây là lời khai của tôi, không phải bằng chứng người chấm kiểm được.)* Đó đúng mô tả Complexity 5: thao tác nhạy cảm với việc không hoàn tác |

### Hai chỗ sửa ngoài danh sách mẫu

Skill chỉ nhận chỗ sửa trên **issue mẫu**. Hai chỗ dưới đây người tự soi ra khi đọc toàn bảng, nằm
ngoài cơ chế đó — nên **không có trong báo cáo HTML**, nhưng có trong trường point trên Project.

| Issue | Skill chấm | Chốt | Lý do |
|---|---|---|---|
| [#46](https://github.com/datdt-0510/thacsiai05-lab5/issues/46) SCR-030 phiên bản biểu phí | 3 × 0 → **3** | **4** | Skill cho Complexity 0 vì *"không có nhánh trạng thái đáng kể"*. Sai: màn phải xác định phiên bản nào **đang** áp dụng theo ngày hiệu lực, và tách người đề xuất với người duyệt |
| [#53](https://github.com/datdt-0510/thacsiai05-lab5/issues/53) SCR-002 trang chủ theo vai trò | 1 × 1 → **1** | **3** | Skill cho Scope 1 vì chỉ thấy một file spec. Nhưng màn đọc ma trận quyền để lọc thẻ chức năng và phải xử lý ca vai trò không có quyền xem màn nào — ngang khối lượng màn danh sách #21 vốn được 3. Chấm 1 là **không nhất quán nội bộ** |

### Một chỗ khuyến nghị ban đầu là bất khả

Vòng đầu tôi đề xuất nâng #51 lên **12 điểm**. Áp vào ma trận mới thấy không được: chỉ có **một** ô
bằng 12 là `Scope 8 × Complexity 2`, mà Scope 8 theo rúbric nghĩa là *"nhiều hệ thống con, nhiều
kho, nhiều môi trường"* — sai với một issue nằm ở hai tầng. Chốt lại 15 điểm bằng cách **giữ Scope 5
và nâng Complexity**, đúng phần skill đánh giá thấp.

**Tổng:** 163 SWP (skill chấm) → **173 SWP** (sau khi người kiểm).

---

## 2. Cách quy ra giờ

### Đã chọn: hệ số hiệu chuẩn từ dự toán LAB-1

Không ước tay từng issue. Lý do: 43 issue ước tay là 43 lần phán đoán chủ quan, khó nhất quán, và
**không giải trình được** khi đem so với LAB-1. Hệ số lấy gốc từ chính dự toán LAB-1 nên mục 5 dưới
đây là so sánh cùng một gốc, không phải hai thang khác nhau.

### Quy tắc

```
giờ/SWP = (giờ của đúng phạm vi backlog trong dự toán LAB-1) ÷ (tổng SWP sau review)
```

### Chốt tử số — đọc thẳng từ file, không lấy từ trí nhớ

Mở bảng dự toán của LAB-1, phần liệt kê 45 màn và phần khai hệ số:

| Thành phần | Implement | Design | Cộng |
|---|---:|---:|---:|
| 20 màn trong phạm vi backlog | 50,00 | 16,50 | **66,50** người-ngày |
| 4 dòng xuyên suốt thuộc epic Nền tảng¹ | 17,00 | 3,00 | **20,00** người-ngày |
| *(ngoài phạm vi: 25 màn + 6 dòng xuyên suốt khác)* | *100,00* | *24,00* | *124,00* |

¹ Bốn dòng đó là: kiến trúc tổng thể và thiết kế lược đồ cơ sở dữ liệu · xác thực, phân quyền và
tầng trung gian kiểm quyền · dịch vụ ghi vết kiểm toán · hệ thống thành phần giao diện dùng chung.
Sáu dòng còn lại (batch CSV, thông báo, di trú dữ liệu, CI/CD, đồng bộ offline) thuộc chức năng
**ngoài phạm vi** backlog nên không vào gốc.

Rồi áp **chính bộ hệ số mà LAB-1 đã khai**, không tự đặt hệ số mới:

| Bước | Hệ số | Kết quả |
|---|---|---:|
| Gốc implement + design | — | 86,50 MD |
| + kiểm thử đơn vị `0,3 × implement` | | |
| + sửa lỗi `0,3 × implement` | | 126,70 MD |
| + dự phòng `30%` | | 164,71 MD |
| − phần giảm nhờ dùng AI `30%` | | **115,30 MD** |

Quy đổi giờ theo đúng định mức LAB-1 đã khai: **20 ngày/tháng × 7 giờ/ngày**.

```
115,30 MD × 7 giờ = 807 giờ
807 giờ ÷ 173 SWP = 4,665 → làm tròn hệ số: 4,7 giờ/SWP
```

**Vì sao trừ hệ số AI:** con số tổng mà LAB-1 công bố (31,80 người-tháng) đã trừ nó. Không trừ thì
mục 5 so hai thang khác nhau. Bản không trừ AI cho **6,66 giờ/SWP** — ghi lại để đối chiếu.

### Cách áp nhất quán

- `giờ issue = SWP × 4,7`, làm tròn tới **0,5 giờ** — áp cho mọi work item **không phải** sub-issue
  giai đoạn
- **Sub-issue giai đoạn của story dùng luật riêng** — xem khối ngay dưới
- `giờ epic = tổng giờ issue con` — **không** tính lại từ SWP của epic, để tổng khớp đúng
- Epic và issue cha là **container**: SWP là tổng con, không tra ma trận lần hai

### Chia điểm cho sáu giai đoạn của một story

Ba sub-issue giai đoạn 1–2 mang **0 SWP và trạng thái `Done`**: tài liệu của chúng chính là bản
thiết kế LAB-4 đã nộp, việc đã xong nên không còn công để ước. Toàn bộ điểm của story chia cho ba
giai đoạn thực thi.

**Chia theo bằng chứng, không theo công thức cứng.** Với mỗi màn, skill chấm *Tạo testcase*,
*Coding* và *Testing* **kèm nội dung ticket cha làm bối cảnh** — nhờ cha, nó biết màn ấy có bao
nhiêu luật chặn, bao nhiêu trạng thái, có thao tác không hoàn tác hay không. Ba điểm thu được dùng
làm **trọng số tương đối**, rồi quy về đúng điểm của story bằng **phương pháp dư lớn nhất**, mỗi
giai đoạn tối thiểu 1 điểm, hoà thì ưu tiên *Coding*. Giờ chia theo tỷ lệ điểm mới, làm tròn 0,5,
dồn phần dư vào giai đoạn nặng nhất.

Vì sao phải quy về điểm cha thay vì lấy thẳng điểm skill chấm: ma trận đo **một khối việc độc lập
to đến đâu**, không đo **phần này chiếm bao nhiêu của cha**. Hỏi sáu câu riêng về cùng một màn thì
cộng lại thành đếm màn đó sáu lần — đo thử trên hai màn cho hệ số phình **5,3 và 6,0 lần**. Quy về
điểm cha giữ đúng nghĩa "tổng backlog là tổng khối lượng thật".

**Kết quả: 5/20 màn lệch khỏi công thức cứng `testcase 1 · testing 1 · coding = S−2`**, và lệch
đúng chỗ có lý do:

| Màn | Công thức cứng | Theo bằng chứng | Bằng chứng ở ticket cha |
|---|---|---|---|
| `SCR-012` | 1 · 3 · 1 | **2 · 2 · 1** | Màn duy nhất phân quyền tới **từng nút**, cộng ẩn nút theo ngày đã chốt sổ — ma trận ca lớn hơn hẳn |
| `SCR-023` | 1 · 2 · 1 | **2 · 1 · 1** | Nhiều luật chặn ở biên: số lượng ≤ 0, và giao vượt nghĩa vụ phải chọn lý do |
| `SCR-025` | 1 · 4 · 1 | **2 · 2 · 2** | Thao tác **không hoàn tác**: mỗi lần chạy lại phải dựng lại dữ liệu ngày nghiệp vụ, nên *Testing* nặng bất thường |
| `SCR-001` | 1 · 3 · 1 | **2 · 2 · 1** | Vừa phải giấu phân biệt *sai mật khẩu* / *email không tồn tại*, vừa trả lý do riêng cho tài khoản bị chặn |
| `SCR-041` | 1 · 3 · 1 | **2 · 2 · 1** | Ma trận vai trò × chức năng × mức quyền, cộng ràng buộc *sửa/duyệt cần quyền xem* — sinh rất nhiều tổ hợp ca |

Mười lăm màn còn lại trùng công thức cứng. Đó là tin tốt chứ không phải công cốc: công thức cứng
đúng cho màn thường, và chỉ sai ở đúng năm màn có đặc thù.

### Kiểm tỉnh táo

Dải kỳ vọng đặt **trước** khi tính: 3–8 giờ/SWP. Kết quả 4,7 nằm trong dải. Kiểm chéo bằng độ hạt
đã chốt ở đợt 3 (*một người làm xong trong 1–3 ngày*): issue 3 SWP ≈ 14 giờ ≈ 2 ngày. Khớp.

---

## 3. Tổng theo epic

| Epic | Issue mức 1 | Tổng SWP | Tổng giờ | Ghi chú |
|---|---:|---:|---:|---|
| [#3](https://github.com/datdt-0510/thacsiai05-lab5/issues/3) Nền tảng | 6 | 35 | 165,0 | Không có nó thì không màn nào dựng được |
| [#4](https://github.com/datdt-0510/thacsiai05-lab5/issues/4) Tư cách tham gia | 6 | 26 | 123,0 | Gồm chốt chặn 許可/承認 |
| [#5](https://github.com/datdt-0510/thacsiai05-lab5/issues/5) Tiếp nhận lô | 4 | 16 | 75,5 | Gồm yêu cầu phủ định 目利き |
| [#6](https://github.com/datdt-0510/thacsiai05-lab5/issues/6) Giao dịch | 5 | 19 | 89,5 | Gồm yêu cầu phủ định セリ |
| [#7](https://github.com/datdt-0510/thacsiai05-lab5/issues/7) Giao hàng | 3 | 11 | 52,0 | Nhỏ nhất — 2 màn |
| [#8](https://github.com/datdt-0510/thacsiai05-lab5/issues/8) Chốt sổ & thưởng | 5 | **40** | 188,0 | **Nặng nhất** — 5 tham số `BR-INC-01` + khoá ngày |
| [#9](https://github.com/datdt-0510/thacsiai05-lab5/issues/9) Quản trị | 6 | 26 | 122,5 | 5 màn, nhiều nhất |
| **Tổng** | **35** | **173** | **815,5** | ≈ 5,83 người-tháng |

Cột *Issue mức 1* đếm **con trực tiếp của epic**, không đếm sub-issue bên dưới. Hai mươi trong số
35 issue đó là **story màn hình**, mỗi story có thêm 6 sub-issue giai đoạn — nên toàn Project có
**142 work item mang điểm**. Điểm của story là tổng của sáu con nên tổng cột SWP không đổi.

Hai epic nặng nhất — Nền tảng và Chốt sổ — chiếm **43%** tổng điểm. Hợp lý: một bên là móng, một
bên mang toàn bộ ràng buộc nghiệp vụ đặc thù của Product A.

---

## 4. So với dự toán LAB-1

So theo **epic**, không chỉ so tổng. Số LAB-1 lấy từ phần liệt kê theo màn, cộng theo đúng nhóm màn mà
mỗi epic bám, rồi áp **cùng chuỗi hệ số ở mục 2** để hai cột về cùng một đơn vị.

| Epic | LAB-1 | LAB-5 | Chênh | Vì sao |
|---|---:|---:|---:|---|
| [#3](https://github.com/datdt-0510/thacsiai05-lab5/issues/3) Nền tảng | 192,4 h | 165,0 h | **−14%** | LAB-1 gộp 4 dòng xuyên suốt thành khối 20 MD ước thô. Break ra thì phần thật sự phải dựng ở giai đoạn này nhỏ hơn: MFA và một phần giám sát thuộc chức năng ngoài phạm vi backlog |
| [#4](https://github.com/datdt-0510/thacsiai05-lab5/issues/4) Tư cách tham gia | 84,7 h | 123,0 h | **+45%** | **Chênh vì ràng buộc đặc thù.** LAB-1 đếm 4 màn. Break ra lộ `許可` và `承認` là **hai cơ chế độc lập** cần chốt chặn riêng, bài kiểm riêng ([#25](https://github.com/datdt-0510/thacsiai05-lab5/issues/25) +3 con) — gộp hai thứ này là điều kiện loại tư cách đề xuất, không phải chi tiết kỹ thuật |
| [#5](https://github.com/datdt-0510/thacsiai05-lab5/issues/5) Tiếp nhận lô | 97,5 h | 75,5 h | **−23%** | LAB-1 cho SCR-013 tới 5,5 MD implement. Thiết kế LAB-4 chốt màn này là form nhập một bảng, phần nặng nằm ở `目利き` mà `目利き` lại là **yêu cầu phủ định** — nghiệm thu bằng sự vắng mặt của chấm điểm tự động, rẻ hơn dựng tính năng |
| [#6](https://github.com/datdt-0510/thacsiai05-lab5/issues/6) Giao dịch | 118,5 h | 89,5 h | **−24%** | Cùng lý do: `セリ` không nhận dạng ảnh ([#39](https://github.com/datdt-0510/thacsiai05-lab5/issues/39)) là yêu cầu phủ định. LAB-1 ước SCR-016 như một bảng điều khiển đầy đủ; LAB-4 chốt phạm vi hẹp hơn |
| [#7](https://github.com/datdt-0510/thacsiai05-lab5/issues/7) Giao hàng | 52,9 h | 52,0 h | **−2%** | Khớp. Hai màn thẳng, không ràng buộc nào ngoài luật cộng theo hướng — mà luật đó đã tách thành [#43](https://github.com/datdt-0510/thacsiai05-lab5/issues/43) và vẫn nằm trong epic |
| [#8](https://github.com/datdt-0510/thacsiai05-lab5/issues/8) Chốt sổ & thưởng | 118,5 h | 188,0 h | **+59%** | **Chênh lớn nhất, và là chênh vì ràng buộc đặc thù.** LAB-1 không có dòng riêng cho `BR-INC-01`: 5 tham số tính thưởng nằm lẫn trong SCR-028 (4,00 MD). Break ra thành [#47](https://github.com/datdt-0510/thacsiai05-lab5/issues/47) +3 con = 11 SWP, cộng [#49](https://github.com/datdt-0510/thacsiai05-lab5/issues/49) và [#50](https://github.com/datdt-0510/thacsiai05-lab5/issues/50) là hai chỗ hở LAB-4 tự khai |
| [#9](https://github.com/datdt-0510/thacsiai05-lab5/issues/9) Quản trị hệ thống | 142,7 h | 122,5 h | **−14%** | LAB-1 gộp cả di trú dữ liệu và UAT vào nhóm này; backlog chỉ nhận 5 màn quản trị, phần còn lại ngoài phạm vi |
| **Tổng phạm vi backlog** | **807,1 h** | **815,5 h** | **+1,0%** | Chênh do **làm tròn** hệ số 4,665 → 4,7 và làm tròn giờ từng issue tới 0,5. Không phải chênh về phạm vi |

> **Về ô tổng 807,1 h.** Cộng bảy số đã làm tròn ở cột LAB-1 ra **807,2**, lệch 0,1 so với ô tổng.
> Ô tổng tính từ số **chưa làm tròn** (807,079) để khớp đúng với mục 2, nên chênh này là do hiển
> thị một chữ số thập phân, không phải sai phép cộng.

Không chênh nào bắt nguồn từ thay đổi khai ở [`PREMISE.md`](PREMISE.md) — bảng "spec thêm/bỏ/sửa"
của file đó trống.

### Chỗ đáng đọc nhất của bảng trên

Tổng gần khớp là **theo thiết kế**, vì hệ số 4,7 lấy gốc từ chính LAB-1. Nên con số +1,0% không nói
gì cả. Thông tin nằm ở **phân bổ**:

> Hai epic **tăng** là đúng hai epic mang ràng buộc nghiệp vụ đặc thù — `完納奨励金` ở #8 (+59%) và
> `許可` vs `承認` ở #4 (+45%). Năm epic còn lại đều **giảm**.

Đây là thứ dự toán theo số màn không thấy được: nó đếm `SCR-028` là một màn, ngang `SCR-005` là một
màn. Break task mới lộ ra rằng sau `SCR-028` có **năm luật tính tiền độc lập**, mỗi luật một cách
sai riêng, còn sau `SCR-005` chỉ có một form.

Chiều ngược lại cũng có thật và ít ai nói tới: hai epic giảm mạnh nhất (#5 −23%, #6 −24%) giảm **vì
yêu cầu phủ định**. `目利き` và `セリ` là chỗ hệ thống bị cấm làm thay người, nên việc phải dựng ít
hơn LAB-1 tưởng. Dự toán theo màn không phân biệt được "màn này phải làm nhiều" với "màn này bị cấm
làm nhiều".

Một mức so khác cho cùng kết luận:

| Quan sát | LAB-1 | LAB-5 |
|---|---|---|
| Tỷ trọng epic Nền tảng | 20,00 / 86,50 MD = **23%** | 35 / 173 SWP = **20%** — gần khớp |
| Chốt sổ (`SCR-025`) so với form hồ sơ (`SCR-005`) | 4,00 MD ↔ 4,00 MD — **bằng nhau** | [#44](https://github.com/datdt-0510/thacsiai05-lab5/issues/44) 6 SWP ↔ [#22](https://github.com/datdt-0510/thacsiai05-lab5/issues/22) 4 SWP. Backlog nhận ra chốt sổ nặng hơn vì **không hoàn tác được**; LAB-1 chấm bằng nhau vì đếm theo số màn |

### Một chỗ LAB-1 và giả định ban đầu của tôi lệch nhau

Bản đầu của [`PREMISE.md`](PREMISE.md) đặt giả định **1 người-tháng = 160 giờ** (20 ngày × 8 giờ).
Mở phần khai định mức của LAB-1 thì thấy **7 giờ/ngày**, tức **140 giờ**. Giả định của tôi
sai và đã sửa. Nếu giữ 160 thì hệ số thành 5,3 giờ/SWP và tổng cao hơn thực tế **14%**.

Đây đúng là lý do kế hoạch ghi "mở file ra đọc, không quyết bằng trí nhớ".

---

## 5. Ghi SWP lên Project

Skill **cố ý không ghi** lên Project (`externally read-only`), và đề cũng tách hai bước. Sau khi
kiểm xong, Agent ghi bằng `gh project item-edit` — cách làm kể ở [`AI-report.md`](AI-report.md).

| Kiểm sau khi ghi | Kết quả |
|---|---|
| 173 item có trường `SWP` | đủ, 0 thiếu |
| 173 item có trường `Giờ ước lượng` | đủ, 0 thiếu |
| Đối chiếu từng item với bảng đã chốt | 0 lệch |
| Tổng SWP của 7 epic | 173 — khớp tổng 142 lá |
| Tổng giờ của 7 epic | 815,5 — khớp tổng 142 lá |
| Mỗi story có điểm cha bằng tổng 6 con | 20/20 khớp, cả SWP lẫn giờ |

**Trường point chỉ ghi SWP**, đúng quy định của đề. Giờ nằm ở trường riêng.

> **Hai chỗ báo cáo HTML lệch với Project, đều có chủ ý.**
>
> **Về điểm:** báo cáo mang **170 SWP** — đã gồm 3 chỗ sửa trên issue mẫu nhưng chưa gồm #46 và #53
> vì hai chỗ đó nằm ngoài cơ chế hiệu chuẩn theo mẫu. Trường point trên Project mang **173 SWP**,
> là con số người đã chốt.
>
> **Về số dòng:** báo cáo chấm **43 work item** — đó là ảnh chụp cấu trúc **trước khi dựng lại theo
> vòng đời story**. Sau khi dựng lại, Project có **142 lá** vì 20 màn hình tách thành 120 sub-issue
> giai đoạn. Tổng điểm **không đổi**: điểm của mỗi màn được chia cho các giai đoạn của chính nó chứ
> không sinh thêm. Skill read-only với nguồn bên ngoài nên không ghi ngược lại báo cáo cũ được; giữ
> nguyên bản chấm gốc và khai chênh ở đây, thay vì sinh lại báo cáo rồi mất dấu vết lần chấm đầu.

---

## Lịch sử sửa đổi

| Ngày | Nội dung |
|---|---|
| 2026-10-21 | Chia điểm sáu giai đoạn theo bằng chứng ticket cha thay vì công thức cứng; 5/20 màn đổi, tổng không đổi |
| 2026-10-21 | Cập nhật số item sau khi dựng lại theo vòng đời story; tổng SWP và giờ không đổi |
| 2026-10-18 | Chấm SWP lần đầu, người kiểm 5 mẫu + 2 chỗ ngoài mẫu, chốt hệ số 4,7 giờ/SWP |
| 2026-10-18 | Mục 4 so theo từng epic thay vì chỉ so tổng — số LAB-1 tách theo nhóm màn |
| 2026-10-20 | Dẫn nguồn dự toán LAB-1 theo cách mô tả thay vì theo địa chỉ ô; giữ nguyên hệ số và phép tính |
| 2026-10-18 | Ghi chú chênh 0,1 do làm tròn ở mục 4 · nêu rõ sự cố LAB-3 là lời khai, không dẫn link được |
