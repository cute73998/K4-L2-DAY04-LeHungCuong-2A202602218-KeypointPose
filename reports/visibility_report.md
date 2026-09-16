# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.69 khớp có v > 0 mỗi người
- Tổng: v=2 325 | v=1 130 | v=0 38

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 7 | 1 | 24% |
| 1 | left_eye | 20 | 8 | 1 | 28% |
| 2 | right_eye | 21 | 7 | 1 | 24% |
| 3 | left_ear | 10 | 17 | 2 | 59% |
| 4 | right_ear | 14 | 15 | 0 | 52% |
| 5 | left_shoulder | 24 | 5 | 0 | 17% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 23 | 4 | 2 | 14% |
| 8 | right_elbow | 24 | 5 | 0 | 17% |
| 9 | left_wrist | 18 | 9 | 2 | 31% |
| 10 | right_wrist | 19 | 9 | 1 | 31% |
| 11 | left_hip | 16 | 12 | 1 | 41% |
| 12 | right_hip | 20 | 8 | 1 | 28% |
| 13 | left_knee | 19 | 6 | 4 | 21% |
| 14 | right_knee | 19 | 6 | 4 | 21% |
| 15 | left_ankle | 14 | 6 | 9 | 21% |
| 16 | right_ankle | 16 | 4 | 9 | 14% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
