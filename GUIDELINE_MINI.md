# Mini guideline - nhóm: **\_\_** | người gán: **\_\_** | ngày: **\_\_**

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống                                        | Luật nhóm bạn chọn                                                                                                                                             | Vì sao                                                                                                                                      |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Hông của người mặc quần áo dài                    | Ước lượng vị trí khớp háng thực tế bên dưới lớp vải, gán cờ `v = 1` (bị che).<br><br>[train_01.jpg]![alt text](image.png)                                      | Khớp xương thực tế quyết định tư thế, không phải bề mặt áo. Việc ước lượng giúp mô hình học đúng cấu trúc động học (kinematics) của cơ thể. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần          | Ước lượng vị trí tai dựa trên góc mặt/đầu, gán cờ `v = 1` (bị che khuất).<br><br>[train_04.jpg] ![alt text](image-3.png)                                       | Tuân thủ quy tắc "bị che, còn trong ảnh -> `v = 1`". Việc gán tai giúp mô hình nhận biết hướng quay của khuôn mặt dù có đội mũ/tóc dài.     |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Gán bình thường các điểm từ hông trở lên. Các điểm ở chân (đầu gối, mắt cá) gán `v = 0` (không đặt chấm).<br><br>[train_04.jpg]![alt text](image-2.png)        | Tuân thủ quy tắc "Ra ngoài mép ảnh -> `v = 0`". Việc cố gắng chấm ở sát mép ảnh hoặc ngoài ảnh sẽ khiến mtrain_03.jpgô hình học sai toạ độ. |
| Cổ tay nằm sau tay lái / sau thân mình            | Ước lượng vị trí cổ tay dựa theo hướng cẳng tay và bàn tay đang cầm lái, gán `v = 1`.<br><br>[train_02.jpg]![alt text](image-4.png)                            | Cổ tay vẫn nằm trong khung ảnh nên cần duy trì tính toàn vẹn của cánh tay. Góc độ cẳng tay cung cấp đủ thông tin để nội suy.                |
| Hai người chồng lên nhau                          | Phân biệt kĩ điểm của từng người. Khớp của người A bị người B che -> gán cho người A với `v = 1`.<br><br>[train_03.jpg]![alt text](image-6.png)                | Nếu bỏ qua các khớp bị che, mô hình sẽ coi như người A bị cụt tay/chân. Việc ước lượng giúp định hình trọn vẹn bộ khung từng cá thể.        |
| Người nhỏ đến mức nào thì không gán nữa           | Bỏ qua (không gán khung và điểm) những người quá nhỏ (VD: chiều cao < 50px hoặc không thể phân biệt mắt thường).<br><br>[train_13.jpg]![alt text](image-7.png) | Các đối tượng quá bé không đủ thông tin pixel về khớp xương. Nếu cố đoán mập mờ sẽ sinh ra nhiễu (label noise) làm mô hình bị rối.          |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_01.jpg`, người thứ `1` (cô gái), khớp `Khớp háng (Hip)`

- Mơ hồ ở chỗ nào: Khớp háng nằm hoàn toàn dưới lớp tạp dề rộng, không có nếp gấp vải rõ ràng để xác định chính xác vị trí.
- Bạn quyết thế nào: Ước lượng vị trí khớp háng dựa trên tỷ lệ cơ thể (khoảng cách từ vai xuống đầu gối) và gán cờ `v = 1` (bị che).
- Vì sao: Khớp háng là điểm bản lề quan trọng của chân. Ước lượng hợp lý giúp duy trì trọn vẹn cấu trúc thân dưới.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu người khác quyết định bỏ qua (`v = 0`), model sẽ bị mất tính liên kết giữa thân trên và chân. Khi test với những người mặc váy/áo dài, model sẽ thường xuyên dự đoán thiếu hai chân.

### Ca 2 - ảnh `train_03.jpg`, người thứ `2` (người đứng sau), khớp `Vai phải (Right Shoulder)`

- Mơ hồ ở chỗ nào: Bị người phía trước che lấp gần như toàn bộ nửa người bên phải, vai phải hoàn toàn không nhìn thấy trên ảnh.
- Bạn quyết thế nào: Ước lượng vị trí vai phải nằm chìm phía sau người phía trước, gán cờ `v = 1`.
- Vì sao: Đối tượng này vẫn hiện rõ nửa người bên trái. Việc gán vai phải giúp định hình lại chiều ngang của cơ thể dù đang bị che khuất (occlusion).
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu bỏ qua khớp này, model sẽ học lầm rằng "khi bị che khuất thì điểm đó không tồn tại". Hệ quả là khi nhận diện đám đông (crowd pose), model sẽ bối rối và vẽ ra những khung xương bị biến dạng hoặc đứt gãy.

### Ca 3 - ảnh `train_02.jpg`, người thứ `1` (người đi xe đạp), khớp `Cổ tay (Wrist)`

- Mơ hồ ở chỗ nào: Cổ tay nắm vào tay lái (ghi-đông) xe đạp nên bị che khuất. Thêm vào đó, góc chụp từ sau lưng làm cổ tay bị gập lại rất khó thấy phần khớp nối.
- Bạn quyết thế nào: Gán cờ `v = 1` tại đúng điểm giao ước lượng giữa cẳng tay và bàn tay đang cầm ghi-đông.
- Vì sao: Dù bị vật thể (xe đạp) che mất, ta vẫn có thể nội suy chính xác vị trí cổ tay thông qua hướng của cẳng tay.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu người khác sợ bị che nên cố tình chấm điểm nhích lên phía cẳng tay (nơi nhìn thấy rõ da thịt), điểm cổ tay sẽ bị sai toạ độ vật lý thực tế. Khi đó model sẽ học sai cấu trúc động học (kinematics) của cánh tay khi đang thao tác cầm nắm vật thể.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `______` (bạn `___%` / họ `___%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**:
- Luật mới bổ sung vào mục 2 sau khi thống nhất:
