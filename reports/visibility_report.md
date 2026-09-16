# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.28 khớp có v > 0 mỗi người
- Tổng: v=2 429 | v=1 43 | v=0 21

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 27 | 2 | 0 | 7% |
| 1 | left_eye | 27 | 2 | 0 | 7% |
| 2 | right_eye | 27 | 1 | 1 | 3% |
| 3 | left_ear | 27 | 2 | 0 | 7% |
| 4 | right_ear | 25 | 3 | 1 | 10% |
| 5 | left_shoulder | 27 | 2 | 0 | 7% |
| 6 | right_shoulder | 29 | 0 | 0 | 0% |
| 7 | left_elbow | 26 | 3 | 0 | 10% |
| 8 | right_elbow | 28 | 1 | 0 | 3% |
| 9 | left_wrist | 24 | 4 | 1 | 14% |
| 10 | right_wrist | 24 | 4 | 1 | 14% |
| 11 | left_hip | 27 | 2 | 0 | 7% |
| 12 | right_hip | 29 | 0 | 0 | 0% |
| 13 | left_knee | 20 | 9 | 0 | 31% |
| 14 | right_knee | 23 | 5 | 1 | 17% |
| 15 | left_ankle | 18 | 3 | 8 | 10% |
| 16 | right_ankle | 21 | 0 | 8 | 0% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
