# PREMISE — Tiền đề của backlog so với bản LAB-4 đã nộp

Backlog này break từ **bản LAB-4 đã nộp**, bản đã bao gồm hai lượt sửa sau review (23 chỗ ở đợt
review chéo, 4 chỗ cập nhật sau khi prototype chuyển sang tiếng Nhật). Những chỗ đó **không** khai
ở đây vì chúng đã nằm trong bản nộp, không phải khác biệt.

File này chỉ ghi ba thứ: thay đổi thiết kế phát sinh khi break, giả định mới đặt thêm, và hướng
đã chốt ở chỗ LAB-4 còn để ngỏ.

---

## 1. Spec thêm / bỏ / sửa so với LAB-4

| Spec (mã/file) | Thêm/Bỏ/Sửa | Trước → sau | Vì sao | Issue bị ảnh hưởng |
|---|---|---|---|---|
| — | — | — | — | — |

**Không có.** Break hết 20 màn, 30 endpoint, 17 bảng và 8 ADR mà không chỗ nào buộc phải sửa
thiết kế. Ghi lại điều này thay vì bỏ trống bảng, vì "không có thay đổi" cũng là một kết quả cần
khai — và nó chỉ đúng nhờ bản nộp đã qua hai lượt review.

Hai chỗ hở về quyết toán (tính lại ghi đè kỳ hiện tại; vết kiểm toán thiếu giá trị trước) **không**
nằm ở bảng này: chính LAB-4 đã ghi chúng là *既知の限界 — LAB-5 の対応項目* trong
`SETTLE_INCENTIVE_TECH.md` §4.4 và §4.5. Chúng là việc đã hẹn làm, không phải thiết kế bị sửa.
Thành issue [#49](https://github.com/datdt-0510/thacsiai05-lab5/issues/49) và
[#50](https://github.com/datdt-0510/thacsiai05-lab5/issues/50).

---

## 2. Giả định bổ sung

| Giả định | Chỗ spec/RFP để ngỏ | Ảnh hưởng tới issue/estimate |
|---|---|---|
| ~~Một người-tháng = 160 giờ~~ → **140 giờ** (20 ngày × 7 giờ) | **Không còn là giả định.** Mở `Effort Estimation` của LAB-1 thì thấy phần khai định mức ghi rõ **20 ngày/tháng** và **7 giờ/ngày** | Bản đầu file này đặt 160 giờ theo thói quen. **Giả định đó sai**: giữ 160 thì hệ số thành 5,3 giờ/SWP và tổng cao hơn thực tế 14%. Đã sửa — xem `estimate.md` §2 |
| Đội giả định **2 người**, mỗi người **6 giờ hiệu quả/ngày** | Đề yêu cầu "người phụ trách giả định" nhưng không nêu quy mô đội | Quyết định sức chứa một sprint, tức số issue nhét vào Sprint 1 — xem `progress-report.md` |
| `ASM-04` (ma trận duyệt sửa sau chốt sổ) theo **§07-03** | **Hồ sơ LAB-1 tự mâu thuẫn**: `Effort Estimation` dẫn §07-03, `Q&A List` dẫn §07-04 cho cùng chủ đề | Không sinh issue trong phạm vi này vì luồng sửa sau chốt sổ (F-017) nằm ngoài backlog. Cần khách xác nhận trước khi làm F-017 |
| Luồng đề nghị sửa sau chốt sổ (F-017) **ngoài phạm vi** backlog | LAB-4 để `SCR-025` nhắc tới F-017 nhưng không có thiết kế chi tiết cho nó | [#44](https://github.com/datdt-0510/thacsiai05-lab5/issues/44) chỉ dựng màn chốt sổ, không dựng luồng sửa |

Mọi giả định ở trên **lấp chỗ để ngỏ**, không chồng lên khối nào RFP đã nêu rõ.

Một chỗ **chưa** thành giả định vì không được phép đoán: 買出人 (`ACT-04`) cần loại tư cách nào.
LAB-4 ghi thẳng *"LAB-5 で顧客に確認すること（本書で決めない）"*. Đã thành issue
[#29](https://github.com/datdt-0510/thacsiai05-lab5/issues/29) gắn nhãn `cần-khách-trả-lời`,
và là một trong các rủi ro trễ ở `progress-report.md`.

---

## 3. Hướng đã chốt

| Vấn đề | Các phương án ở LAB-4 | Phương án chốt | Vì sao |
|---|---|---|---|
| LAB-4 không gom ADR, thiết kế CSDL và tài liệu kiến trúc vào thư mục feature nào | (a) để rải trong `system-design/`, mỗi màn tự gánh phần móng của mình · (b) gom thành một epic riêng. LAB-4 **không chọn** vì nó không bàn tới backlog | Thêm **epic thứ 7: Nền tảng** ([#3](https://github.com/datdt-0510/thacsiai05-lab5/issues/3)) | Không có lược đồ dữ liệu, cổng phiên, ma trận quyền và lớp từ điển thì không màn nào dựng được. Để chúng rải ra là backlog có 20 màn mà không ai nhận phần móng |
| Phạm vi backlog: break bao nhiêu chức năng | (a) break cả 31 chức năng nêu trong hồ sơ · (b) chỉ break 15 chức năng có detail design. LAB-4 `PREMISE.md` đã thu còn 15 nhưng **không nói** backlog phải theo con số nào | Bám đúng 15 chức năng / 20 màn mà LAB-4 có thiết kế chi tiết | 25 màn còn lại **không có detail design để break**. Break chúng là bịa việc, và đề nói nguồn break là detail design LAB-4 |
| Độ hạt khi chia | (a) một issue mỗi màn · (b) một issue mỗi endpoint · (c) một issue mỗi màn, tách sub-issue khi màn gánh ràng buộc riêng. LAB-4 **không nói** về độ hạt backlog | 7 epic · 35 issue · 13 sub-issue = **55 item** | Xem `task-breakdown.md` §1 |

---

## Lịch sử sửa đổi

| Ngày | Nội dung |
|---|---|
| 2026-10-18 | Khởi tạo khi break backlog từ bản LAB-4 đã nộp |
| 2026-10-20 | Bỏ tham chiếu sheet/ô của bảng dự toán nội bộ |
| 2026-10-18 | Mục 3 nêu rõ các phương án đã cân nhắc thay vì chỉ nêu hiện trạng — theo review chéo |
