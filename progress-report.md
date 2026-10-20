# Báo cáo tiến độ — Sprint 1

| | |
|---|---|
| **Iteration trên Project** | `Sprint 1` |
| **Khoảng thời gian (giả lập)** | 2026-10-05 → 2026-10-18 · 14 ngày lịch · **10 ngày làm việc** |
| **Nguồn số liệu** | Trạng thái, SWP, giờ, người phụ trách: trích từ [GitHub Project](https://github.com/users/datdt-0510/projects/2) bằng `gh project item-list --format json`, không gõ tay. **Cột "ngày nào đóng issue nào" trong burndown là tái dựng** — xem ghi chú ở mục 2 |
| **Đội giả định** | Tên hư cấu, xem bảng dưới. Sprint 1 chỉ dùng **2 lập trình viên + 1 BrSE** · sức chứa lập trình **120 giờ** = 2 × 6 giờ hiệu quả × 10 ngày, theo giả định đã khai ở `PREMISE.md` |

---

## 1. Trạng thái

Sprint 1 nhận **trọn epic [#3](https://github.com/datdt-0510/thacsiai05-lab5/issues/3) Nền
tảng** (35 SWP) cộng [#29](https://github.com/datdt-0510/thacsiai05-lab5/issues/29) (2 SWP) —
đúng thứ tự dựng thật, vì không có lược đồ dữ liệu, phiên đăng nhập và ma trận quyền thì không màn
nào chạy được. Tổng 37 SWP, chia thành 10 work item lá cộng 2 issue cha là container (container
không mang điểm riêng, chỉ cộng của con).

| Trạng thái | Số issue | Tổng SWP | Tổng giờ | Ghi chú |
|---|---:|---:|---:|---|
| **Done** | 6 | **22** | 104,0 | Toàn bộ lược đồ dữ liệu, cổng phiên, ma trận quyền |
| **In Progress** | 1 | 5 | 23,5 | Lớp từ điển giao diện — trôi sang Sprint 2 |
| **Blocked** | 2 | 7 | 33,0 | Xem mục 3 |
| **Todo** | 1 | 3 | 14,0 | Chưa chạm |
| **Tổng cam kết** | **10** | **37** | **174,5** | |

**Hoàn thành 22/37 SWP = 59%.**

### Đội giả định

Mọi tên dưới đây là **tên hư cấu**, đặt theo kiểu họ Việt + từ chỉ vai trò để không trùng người
thật. GitHub chỉ gán được tài khoản có thật nên các tên này nằm ở trường chữ riêng
*Người phụ trách (giả định)* trên Project, không phải trường assignee.

| Người | Vai trò | Trong Sprint 1 |
|---|---|---|
| **Nguyễn Văn Dev** | Lập trình viên 1 — lược đồ dữ liệu, quy tắc nghiệp vụ, tầng API | Có |
| **Trần Thị Dev** | Lập trình viên 2 — màn hình và luồng giao diện | Có |
| **Lê Văn BrSE** | Cầu nối khách hàng — câu hỏi làm rõ, chốt phạm vi | Có |
| **Nguyễn Thị QA** | Kiểm thử — nhận các issue nghiệm thu bằng bài kiểm | Chưa, từ Sprint 2 |
| **Phạm Thị PM** | Quản lý — theo dõi epic, xếp sprint | Chưa, theo dõi epic |

### Phân bổ Sprint 1

| Người | Issue | SWP | Giờ nhận | Sức chứa | Tỷ lệ |
|---|---:|---:|---:|---:|---:|
| Nguyễn Văn Dev | 4 | 17 | 80,5 | 60 | **134%** |
| Trần Thị Dev | 5 | 18 | 84,5 | 60 | **141%** |
| Lê Văn BrSE | 1 | 2 | 9,5 | — | điều phối, không tính vào sức chứa lập trình |

Cả hai lập trình viên đều nhận quá sức chứa. Đây là nguyên nhân thứ nhất của phần lệch bên dưới.

---

## 2. Burndown

Đơn vị: SWP còn lại. Kế hoạch là đường thẳng từ 37 về 0 trong 10 ngày làm việc.

> **Nguồn của bảng này.** GitHub Project chỉ giữ trạng thái **hiện tại**, không giữ lịch sử đổi
> trạng thái theo ngày. Nên cột cuối cùng — ngày nào đóng issue nào — là **tái dựng theo thứ tự phụ
> thuộc giữa các issue**, không phải số đo. Điểm đầu (37) và điểm cuối (15) thì khớp đúng Project.

| Ngày làm việc | Kế hoạch còn lại | Thực tế còn lại | Việc đóng trong ngày |
|---:|---:|---:|---|
| 0 | 37,0 | 37 | — |
| 1 | 33,3 | 37 | dựng môi trường |
| 2 | 29,6 | 33 | #11 nhóm bảng tư cách |
| 3 | 25,9 | 29 | #12 nhóm bảng hàng & giao dịch |
| 4 | 22,2 | 25 | #13 nhóm bảng quyết toán → #10 đóng |
| 5 | 18,5 | 22 | #15 cổng phiên ở proxy |
| 6 | 14,8 | 19 | #17 gieo ma trận quyền |
| 7 | 11,1 | **15** | #18 hàm kiểm quyền → #16 đóng |
| 8 | 7,4 | **15** | — |
| 9 | 3,7 | **15** | — |
| 10 | 0,0 | **15** | — |

### Thực tế lệch kế hoạch ở đâu

Hai điều, và chúng khác nhau về bản chất.

**Lệch đều từ đầu sprint: cam kết vượt sức chứa.** Phần việc giao cho hai lập trình viên là 165
giờ, trong khi sức chứa là 120 — **138%**. Nói cách khác, đường kế hoạch về 0 **không bao giờ đạt
được**, kể cả khi mọi thứ trôi chảy. Đường thực tế vì vậy nằm trên đường kế hoạch ngay từ ngày 1,
trước khi có bất kỳ vấn đề nào. Đây là lỗi lập kế hoạch, không phải lỗi thực thi.

**Phẳng hẳn từ ngày 7.** Ba ngày cuối không đóng được việc nào. Không phải vì đội chậm: hai issue
dừng vì **thiếu một quyết định chưa ai chốt được**, còn issue thứ ba
([#19](https://github.com/datdt-0510/thacsiai05-lab5/issues/19) lớp từ điển) hoá ra nặng hơn
ước — ràng buộc khoá giữa hai bản ngôn ngữ ở mức kiểu cần dựng thêm một tầng kiểu riêng mà ước
lượng ban đầu không tính tới.

Hai chỗ chặn khác hẳn nhau, và điều đó quyết định cách xử lý:

- [#29](https://github.com/datdt-0510/thacsiai05-lab5/issues/29) là **chỗ hở spec** — đội không
  có quyền tự quyết 買出人 cần loại tư cách nào, phải chờ khách. Tự đoán là làm sai luật chợ.
- [#14](https://github.com/datdt-0510/thacsiai05-lab5/issues/14) là **chỗ hở thiết kế** — đội
  tự sửa được, chỉ là chưa chốt cách kiểm. Chặn này nằm trong tầm tay.

Đoạn phẳng là thông tin, nhưng phải đọc đúng tỷ lệ — và tỷ lệ ở đây nhỏ hơn cảm giác: trong 15 SWP
còn lại, **chỉ 2 SWP thật sự chờ khách** (#29). Mười ba SWP còn lại nằm trong tay đội, trong đó
5 SWP (#14) dừng vì **đội chưa tự chốt được một quyết định kỹ thuật**, không phải vì thiếu người.

Nói thẳng: sprint này trượt chủ yếu vì **cam kết quá sức**, thứ đến là một quyết định bị treo. Phần
chờ khách tuy nhỏ về điểm nhưng nguy hiểm theo cách khác — nó chặn đường vào epic giao dịch, nên
hậu quả của nó rơi vào sprint sau chứ không phải sprint này.

---

## 3. Rủi ro trễ

| Rủi ro | Issue | Ảnh hưởng tới mốc nào | Hành động | Ai làm, khi nào |
|---|---|---|---|---|
| **Khách chưa trả lời: 買出人 (`ACT-04`) cần loại tư cách nào.** Hỏi từ ngày 2, chưa có đáp | [#29](https://github.com/datdt-0510/thacsiai05-lab5/issues/29) | Chặn [#28](https://github.com/datdt-0510/thacsiai05-lab5/issues/28) kiểm tư cách phân nhánh → chặn [#35](https://github.com/datdt-0510/thacsiai05-lab5/issues/35) màn tạo 相対取引. Đây là **luồng chính của chợ**, nên trễ ở đây trễ cả epic Giao dịch | Gửi lại câu hỏi kèm **hai phương án đề xuất** và hạn trả lời. Quá hạn thì đi theo phương án mặc định (`ACT-04` không cần tư cách riêng) và ghi thành giả định ở `PREMISE.md`, nêu rõ chi phí nếu đoán sai | Lê Văn BrSE · gửi lại 2026-10-20, hạn khách trả lời 2026-10-24 |
| **Chưa chốt cách kiểm "khoá `service_role` không lọt vào gói trình duyệt".** Phát hiện khi làm: định nghĩa xong đòi điều này nhưng không nêu kiểm bằng gì | [#14](https://github.com/datdt-0510/thacsiai05-lab5/issues/14) | Chặn phần còn lại của epic [#3](https://github.com/datdt-0510/thacsiai05-lab5/issues/3) Nền tảng. Mọi epic khác dựng trên nó | Chốt một cách kiểm cụ thể (quét gói sau khi build, bài kiểm đỏ nếu thấy chuỗi khoá) rồi bổ sung vào định nghĩa xong của issue | Trần Thị Dev · 2026-10-21 |
| **Cam kết vượt sức chứa 38%.** Lỗi lập kế hoạch, lặp lại thì mọi sprint đều trễ | — | Mọi mốc phía sau trượt dần, và burndown mất giá trị cảnh báo vì luôn đỏ | Sprint 2 cam kết theo **vận tốc quan sát được ở Sprint 1 là 22 SWP**, không theo sức chứa lý thuyết. Giữ 15 SWP trôi sang cộng tối đa 7 SWP mới | Phạm Thị PM · khi lập Sprint 2, 2026-10-19 |
| **Epic nặng nhất chưa chạm.** [#8](https://github.com/datdt-0510/thacsiai05-lab5/issues/8) Chốt sổ & thưởng nặng 40 SWP, chứa toàn bộ ràng buộc đặc thù `BR-INC-01` (5 tham số) | [#47](https://github.com/datdt-0510/thacsiai05-lab5/issues/47) và 3 issue con | Để dồn về cuối thì rủi ro dồn về cuối, đúng lúc không còn thời gian xoay | Đưa [#48](https://github.com/datdt-0510/thacsiai05-lab5/issues/48) (tách ba nhóm khoản thanh toán) vào Sprint 2 để chạm sớm phần khó nhất, kể cả khi epic chưa tới lượt | Phạm Thị PM · Sprint 2 |

---

## 4. Dự phóng

Vận tốc quan sát ở sprint đầu: **22 SWP/sprint**. Đây là sprint **giả lập**, nên con số này là một
quan sát chứ chưa phải vận tốc đo từ công việc thật. Một sprint cũng không đủ để nói vận tốc ổn
định. Nhưng nó đủ để thấy khoảng cách.

| | |
|---|---|
| Đã xong | 22 / 173 SWP — **13%** |
| Còn lại | 151 SWP |
| Theo vận tốc hiện tại | ≈ **7 sprint nữa** ≈ 14 tuần |

Con số này **giả định không có gì bị chặn**. Hai issue đang `Blocked` chiếm 7 SWP nhưng chặn phía
sau nhiều hơn thế — riêng `ACT-04` chặn đường vào luồng giao dịch chính. Nếu khách không trả lời
trong Sprint 2, dự phóng này phải tính lại.

---

## Lịch sử sửa đổi

| Ngày | Nội dung |
|---|---|
| 2026-10-18 | Báo cáo Sprint 1 (giả lập) |
| 2026-10-18 | Điền người phụ trách giả định cho cả 55 item, đổi sang tên hư cấu kiểu Việt |
| 2026-10-18 | Nêu rõ cột ngày trong burndown là tái dựng · sửa "vận tốc thật đo được" thành quan sát ở sprint giả lập · sửa lại tỷ lệ phần việc chờ khách |
