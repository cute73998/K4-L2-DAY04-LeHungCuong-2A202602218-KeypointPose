# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Hùng Cường   Nhóm: T015  Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 325 / 130 / 38 |
| Thời gian trung bình mỗi ảnh | 4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear`: 59% (17/29 occluded)
2. `right_ear`: 52% (15/29 occluded)
3. `left_hip`: 41% (12/29 occluded)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Cả 3 khớp trên đúng là những vị trí dễ gây tranh cãi và khó xác định nhất. Tai (`left_ear`, `right_ear`) thường xuyên bị tóc, mũ hoặc góc nghiêng của đầu che khuất một phần, đòi hỏi phải suy luận vị trí giải phẫu từ mắt và mũi. Trong khi đó, hông (`left_hip`) là khớp thuộc nhóm thân hay bị che bởi trang phục (vạt áo thụng, váy dài), làm mất bề mặt nhìn thấy trực tiếp nên phải ước lượng dựa trên trục thắt lưng và đùi.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9370 | 0.9504 |
| OKS@0.50 | 0.9310 | 1.0000 |
| OKS@0.75 | 0.9310 | 1.0000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 3 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_13.jpg` người #1, #2: Bổ sung 2 skeleton người bị thiếu, chấm đầy đủ 17 keypoints theo ảnh/gold annotation với cờ `v=2` và `v=1`.
- `train_10.jpg` người #1: Sửa 2 khớp hông `left_hip` và `right_hip` từ `v=0` thành `v=1` (`left_hip`: 0.664000 0.880000 1, `right_hip`: 0.378000 0.861333 1) tại vị trí ước lượng bị áo che.
- `train_04.jpg` người #1: Sửa khớp cổ tay trái `left_wrist` từ `v=0` thành `v=1` (`left_wrist`: 0.504687 0.789934 1) tại vị trí ước lượng.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: Ngô Xuân Nam

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| `left_hip` | 41% | 25% | 16% | Guideline chưa rõ trường hợp hông bị vạt áo phông thụng/váy che |
| `left_ear` | 59% | 45% | 14% | Guideline chưa thống nhất khi tai bị tóc che một nửa |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Khi khớp hông (`left_hip`, `right_hip`) bị trang phục che khuất nhưng toàn bộ cơ thể người vẫn nằm trong khung hình, bắt buộc phải chọn `v=1` và chấm ở vị trí ước lượng dựa theo trục vai và đùi, tuyệt đối không được đánh `v=0`.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   `pose_mAP50-95` tăng nhẹ +0.0055 (từ 0.6853 lên 0.6908). Dù tập dữ liệu fine-tune chỉ gồm 20 ảnh, mAP pose không bị giảm mà còn cải thiện nhẹ nhờ model thích ứng tốt hơn với các góc chụp và bối cảnh đặc thù của tập bài tập.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) là 0.1133 (11.33%). Model tìm *người* dễ hơn tìm *khớp*. Lý do: Bounding box người có diện tích lớn, đặc trưng tổng thể về hình dáng cơ thể rất rõ ràng và ít bị biến dạng cực đoan; trong khi keypoints là các vị trí pixel rất nhỏ, dễ bị che khuất (occluded) và đòi hỏi độ chính xác không gian cao theo dung sai OKS.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Ở ảnh test `test_03.jpg`, model mắc lỗi **lệch nhẹ** ở khớp cổ tay (`left_wrist`) do hai tay của đối tượng bắt chéo gần nhau, làm tâm chấm keypoint bị dịch chuyển nhẹ khoảng vài pixel so với khớp giải phẫu chuẩn.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh `train_13.jpg` (người #1 và #2) ban đầu có OKS thấp nhất giữa nhãn và model do nhãn cũ thiếu 2 skeleton. Sau khi đối chiếu với gold annotation và xem ảnh gốc, nhãn của tôi (sau khi bổ sung) là đúng vì 2 người ở xa rõ ràng có mặt trong bức ảnh và cần được gán đầy đủ keypoints.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   Có, ảnh `train_13.jpg` vừa là bức ảnh nhãn ban đầu dễ bỏ sót vừa là ảnh model gặp khó khăn trong dự đoán. Điều này phản ánh bức ảnh có độ phức tạp cao: các đối tượng nằm ở hậu cảnh, quy mô nhỏ, tỉ lệ che khuất cao và có nhiều vật thể gây nhiễu xung quanh.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

Trong ảnh `train_10.jpg`, người thứ 1, tôi đã quyết định chọn trạng thái `v=1` cho khớp hông trái (`left_hip`). Căn cứ thị giác cho thấy toàn bộ thân người vẫn nằm gọn trong khung hình, hai vai và hai chân hiển thị rõ ràng, nhưng phần hông bị phủ bởi vạt áo phông thụng. Dựa vào vị trí vai trái và đùi trái liền kề, vị trí giải phẫu của khớp hông trái hoàn toàn xác định được bên dưới lớp áo. Vì khớp vẫn nằm trong khung ảnh và chỉ bị trang phục che khuất, việc chọn `v=1` (Occluded - đặt chấm tại vị trí ước lượng) là hoàn toàn chính xác theo guideline.
