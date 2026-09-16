# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: **Trương Văn Vượng-2A20262077** Nhóm: **\_\_** Ngày: **16/9/2026**

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số                       |       Giá trị |
| ---------------------------- | ------------: |
| Số ảnh đã gán                |            20 |
| Số skeleton                  |            29 |
| v=2 / v=1 / v=0              | 429 / 43 / 21 |
| Thời gian trung bình mỗi ảnh |             6 |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. Đầu gối trái (`left_knee`) - 9 lần
2. Đầu gối phải (`right_knee`) - 5 lần
3. Cổ tay (`left_wrist` / `right_wrist`) - 4 lần

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

Đúng, đây là những khớp khó gán nhất vì chúng rất hay bị che lấp trong các khung hình thực tế thay vì do khó xác định vị trí giải phẫu. Bằng chứng là khi quan sát các ảnh train, đầu gối thường xuyên bị che hoàn toàn bởi lớp quần áo dài rộng, không để lộ nếp gấp vải. Tương tự, phần cổ tay thường bị che lấp khi tay đang thao tác cầm nắm đồ vật (như cầm tay lái) hoặc bị khuất sau thân mình do góc máy, buộc phải nội suy dựa vào hướng cánh tay.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số                | Trước rework | Sau rework |
| --------------------- | -----------: | ---------: |
| OKS trung bình        |       0.9349 |            |
| OKS@0.50              |          1.0 |            |
| OKS@0.75              |          1.0 |            |
| Lỗi `dao_trai_phai`   |            1 |            |
| Lỗi `nham_nguoi`      |            0 |            |
| Lỗi `xoa_khop_bi_che` |            0 |            |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_13.jpg`, người thứ 3, các khớp bên trái/phải: Chỉnh lại cờ và toạ độ do đánh nhầm bên trái thành bên phải (lỗi đảo trái/phải).
<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_01.jpg`, người thứ 1, khớp `left_wrist`: Chỉnh lại toạ độ điểm do bị lệch nhẹ 25px so với nhãn gold.
- `train_01.jpg`, người thứ 2, khớp `right_wrist`: Chỉnh lại toạ độ do báo lỗi lệch nhẹ 27px.
- `train_13.jpg`, người thứ 2, khớp `left_knee` và `right_knee`: Di dời lại vị trí hai đầu gối do chấm lệch hơn 30px so với tiêu chuẩn.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải của tôi xảy ra ở ảnh `train_13.jpg`. Đây là một ảnh khó do chất lượng ảnh bị mờ, đồng thời người trong ảnh đang đứng quay lưng chéo nên rất dễ nhầm lẫn khi phân định các khớp trái/phải của cơ thể.

## 3. Kiểm chéo

Bạn cùng nhóm: **\_\_**

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn |  Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| ---- | --: | --: | ---: | ------------------------------------ |
|      |     |     |      |                                      |
|      |     |     |      |                                      |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số         | yolo26n-pose gốc | Sau fine-tune |   Chênh |
| -------------- | ---------------: | ------------: | ------: |
| pose_mAP50     |           0.8450 |        0.8450 |  0.0000 |
| pose_mAP50-95  |           0.6853 |        0.6908 |  0.0055 |
| pose_precision |           0.9734 |        0.9792 |  0.0058 |
| pose_recall    |           0.8462 |        0.8462 |  0.0000 |
| box_mAP50-95   |           0.8119 |        0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   - **Trả lời:** Chỉ số `pose_mAP50-95` **tăng nhẹ 0.0055** (từ 0.6853 lên 0.6908). Việc chỉ số tăng chứng tỏ 20 ảnh gán nhãn tay của tôi đã cung cấp thêm thông tin hữu ích, giúp mô hình học được cách định vị tọa độ các khớp chính xác hơn so với bộ trọng số COCO gốc.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm _người_ dễ hơn hay tìm
   _khớp_ dễ hơn? Vì sao?
   - **Trả lời:** `box_mAP50-95` (0.8041) chênh lệch cao hơn hẳn so với `pose_mAP50-95` (0.6908), mức chênh lên tới **0.1133**. Rõ ràng model tìm **người (box)** dễ hơn tìm **khớp (pose)**. Lý do là việc phát hiện vùng tổng thể bao quanh cơ thể đơn giản hơn nhiều so với việc định vị chi tiết 17 tọa độ nằm bên trong, đặc biệt là khi các khớp thường xuyên bị che khuất hoặc biến đổi góc nhìn linh hoạt.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   - **Trả lời:** Ở ảnh test (ví dụ `test_03.jpg`), model đoán sai vị trí cổ tay trái, đây là lỗi **"lệch nhẹ"**. Nguyên nhân do cổ tay bị khuất hoặc hòa lẫn vào màu nền, khiến model ước lượng sai điểm gấp khúc.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   - **Trả lời:** Ảnh `train_13.jpg` có OKS thấp nhất. Trong trường hợp này, **model đã dự đoán đúng**. Dựa vào bằng chứng thị giác, do ảnh mờ và người đứng quay lưng chéo, tôi đã gán nhãn sai (lỗi đảo trái/phải), trong khi model nhờ học được đặc trưng không gian tổng thể nên đã phân định đúng các khớp.

5. Ảnh bạn gán tệ nhất có _cũng_ là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   - **Trả lời:** **Có**, ảnh tôi gán tệ nhất (`train_13.jpg`) cũng là ảnh model và tôi có độ lệch OKS cao nhất. Điều này nói lên rằng bức ảnh đó có độ phức tạp về thị giác quá lớn (ảnh mờ, tư thế quay lưng chéo gây ảo giác). Khi dữ kiện hình ảnh bị thiếu hụt hoặc nhiễu, cả não người lẫn máy học đều gặp khó khăn và dễ dự đoán sai lệch.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

**(1)** Ở ảnh `train_01.jpg`, người thứ 1, đối với khớp háng trái (`left_hip`).
**(2)** Căn cứ thị giác là khớp này bị che khuất hoàn toàn bởi lớp tạp dề dài và rộng, không hề để lộ nếp gấp quần áo hay đường nét cơ thể tại vùng háng. Tuy nhiên, toàn bộ phần đùi và cẳng chân nối liền bên dưới vẫn hiển thị rõ nét bên trong bức ảnh.
**(3)** Do phần chi dưới nối với khớp háng vẫn nằm trong khung hình, theo nguyên tắc bảo toàn cấu trúc động học (kinematics), tôi quyết định nội suy vị trí dựa vào trục đùi và thân trên, chọn trạng thái `v=1` (bị che khuất) thay vì `v=0` để tránh việc mô hình học sai thành tư thế "cụt chi".
