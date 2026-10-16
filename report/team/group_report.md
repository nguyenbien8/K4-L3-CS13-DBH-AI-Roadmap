# Báo cáo nhóm DBH: CS13 Lộ trình sản phẩm AI

## 1. Thông tin nhóm

- Tên nhóm: DBH
- Đề CS13: Đề 1 · SổSạch: trợ lý đọc hóa đơn đầu vào cho văn phòng kế toán dịch vụ
- Thành viên: xem [team/TEAM.md](../../team/TEAM.md).

## 2. Mục tiêu và cách tiếp cận

Mục tiêu là một lộ trình 12 tháng trả lời ba câu hỏi của ban lãnh đạo: làm gì trước, khi nào cho máy tự nhập nhiều hơn, khi nào dừng. Mọi chỉ số dùng đúng đơn vị "1 việc = 1 hóa đơn được đọc, đối chiếu và nhập vào phần mềm kế toán".

Căn cứ lấy từ hồ sơ Đề 1:

- **Tiêu chí của chuyên gia (chị Hạnh):** sai mã số thuế hay sai tiền thì không được lần nào. Gắn cờ thừa một chút thì chấp nhận, nhưng nếu một nửa số hóa đơn bị cờ thì không ai dùng. Từ đó nhóm đặt hai chỉ số chính cho mọi cổng: **0 lỗi mã số thuế/tiền** và **tỷ lệ gắn cờ** (ngưỡng ≤ 20%, báo động khi > 30%).
- **Bằng chứng đánh giá:** bản 2 đã qua cổng phát hành (đạt 97,3%, 0 lỗi nghiêm trọng, 0 tầng tụt, kappa 0,90), nên tầng Bây giờ là vận hành và đo trên hóa đơn thật, không phải xây lại.
- **Công cụ sẵn có:** phần mềm kế toán đã có tính năng nhập hóa đơn điện tử từ tệp gốc, và đã có dịch vụ tra cứu mã số thuế để tích hợp. Vì vậy nhóm chọn **Mua/dùng sẵn** cho hai phần này, không tự xây.
- **Dữ liệu:** chưa có dữ liệu hạch toán của các doanh nghiệp khách hàng, nên việc "gợi ý tài khoản" được xếp **Chờ**.
- **Slide 81 (hỏi AI việc nào bị mô hình miễn phí làm thay):** gợi ý của AI được đối chiếu với hồ sơ để chấm cột L. Hỏi đáp thuế chung (0,9) và nhập tệp gốc (0,9) dễ bị nuốt nhất. Gắn cờ (0,2) và kiểm chứng doanh thu (0,1) gắn với quy trình và dữ liệu riêng nên bền hơn.

## 3. Giải pháp

- **Ưu tiên** (`1_Danh sách việc`): 8 việc, gồm 6 việc gợi ý và 2 việc nhóm thêm.
  - Hạng 1 là đối chiếu mã số thuế (14,25 điểm), hạng 2 là gắn cờ (8,1 điểm).
  - Việc 7 "kiểm chứng doanh thu trên ≥ 10 văn phòng" đứng hạng 6 nhưng vẫn đưa vào tầng Bây giờ. Lý do: thử nghiệm hiện chỉ có 1 văn phòng, không đo được điều kiện dừng của đề (cần ít nhất 10 văn phòng), và kịch bản xấu là hết tiền ở tháng 7.
- **Mức tự động** (`2_Mức tự động`):
  - Mức 0: duyệt 100%.
  - Mức 1: duyệt 20%. Cổng là 0 lỗi trên ≥ 1.000 hóa đơn; theo quy tắc 3/n, giới hạn trên của tỷ lệ lỗi là 0,3%.
  - Mức 2: duyệt 10%. Cổng là 0 lỗi trên ≥ 3.000 hóa đơn (giới hạn 0,1%) và đạt ≥ 97% trên bộ đề 100 ca.
  - Mức 3: duyệt 5%. Cổng là 0 lỗi trên ≥ 10.000 hóa đơn ở ≥ 3 văn phòng.
  - Người duyệt luôn là kế toán viên của công ty (Người của mình).
- **Lộ trình và rà soát** (`3_Bảng lộ trình`, `4_Rà soát`):
  - Điều kiện dừng tầng Bây giờ đo được ngay trên quy mô thử nghiệm (~6.000 hóa đơn trong 2 tuần).
  - Điều kiện dừng theo doanh thu của đề (< 3,71 triệu/văn phòng, trên ≥ 10 văn phòng) dùng từ tầng Tiếp theo, khi đã đủ số văn phòng.

## 4. Kết quả

- File: `DBH_CS13.xlsx`. Trang `6_Tự kiểm tra` đạt 8/8 khi mở bằng Excel.
- Chi phí và biên gộp theo mức, tính từ số liệu của đề (doanh thu ≈ 2.256 đ/hóa đơn = 3,97 triệu ÷ 1.760):

| Mức | % có người duyệt | Chi phí/hóa đơn | Biên gộp | Đạt ≥ 60%? |
|---|---|---|---|---|
| 0 · Gợi ý | 100% | 1.704 đ | 24,5% | Không |
| 1 · Hỗ trợ có duyệt | 20% | 704 đ | 68,8% | Đạt |
| 2 · Tự động một phần | 10% | 579 đ | 74,3% | Đạt |
| 3 · Tự động có giám sát | 5% | 517 đ | 77,1% | Đạt |

- Slide lãnh đạo: `DBH_CS13_Slide.pptx` (kèm bản PDF). Nội dung trùng với 5 ô B4:B8 của trang 5; mỗi ô từ 36 đến 47 chữ.

## 5. Khó khăn và cách xử lý

- **Điều kiện dừng của đề cần ≥ 10 văn phòng, nhưng thử nghiệm chỉ có 1.** Nhóm tách thành hai lớp. Tầng Bây giờ dừng theo chất lượng (lỗi, tỷ lệ gắn cờ), đo được ngay. Tầng Tiếp theo dừng theo doanh thu. Ký 10 văn phòng trước ngày 90 trở thành một việc chính của tầng Bây giờ.
- **Mức 0 có biên 24,5%, dưới ngưỡng 60%.** Nhóm không hạ yêu cầu an toàn. Thay vào đó, Mức 0 giới hạn tối đa 4 tuần và phải qua cổng lên Mức 1 trước ngày 60.
- **Giờ chuyên gia có hạn:** chị Hạnh chỉ có 3 giờ/tuần. Thêm 60 ca vào bộ đề tốn khoảng 13 giờ (theo tỷ lệ 8,5 giờ/40 ca), nên cần chia việc với anh Tùng, mất khoảng 3 tuần.

## 6. Tổng hợp đóng góp

Điền đóng góp trong [team/TEAM.md](../../team/TEAM.md).

## 7. Hạn chế và hướng cải thiện

- Các ngưỡng nhóm tự đề xuất là giả định cần kiểm chứng, chưa phải kết quả đo: gắn cờ ≤ 20%, giảm 50% giờ nhập hóa đơn ảnh, gợi ý tài khoản đúng ≥ 85%. Mốc xem lại là các ngày 30/60/90.
- Chưa biết tỷ lệ hóa đơn ảnh so với hóa đơn điện tử; cần đo trong 2 tuần đầu để chấm lại độ phủ của việc 1.
- Đến ngày 90 chưa đo được giá trị vòng đời của khách, vì khách mới dùng được 3 tháng.

## 8. Tài liệu tham khảo

- Hồ sơ tình huống CS13 (`CS13_HoSoTinhHuong.pdf`), Đề 1; slide bài giảng CS13 (slide 74: quy tắc 3/n; slide 81: rủi ro bị nuốt).
- Luật Trí tuệ nhân tạo số 134/2025/QH15; Luật Bảo vệ dữ liệu cá nhân số 91/2025/QH15 (theo trích dẫn trong tài liệu lớp).
