# TEAM — Ngày 12

Mã bộ ảnh: **G10** · Hình thức: cá nhân

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| HaTienTrinh | 2A202603154 | có | Vũ Tuyết Trinh (cùng mã G10) | 99/99 (`disagreement_log.csv`: 96 `mine`, 3 `peer`) |

Ca hai bạn vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- Không còn ca nào chưa thống nhất. Các dòng về trẻ ngồi trong ghế (S02, S03, S05, S07: bạn vẽ `person`/`face` cho trẻ) được phân xử theo hướng dẫn coach đã gửi: `child_seat.occupied = yes` nghĩa là có trẻ ngồi, không vẽ box cho trẻ.

## Cá nhân

### HaTienTrinh-2A202603154
- Phần mình làm: tự gán đủ 22 ảnh cốt lõi của G10 trên CVAT local; review chéo với Vũ Tuyết Trinh bằng `cabin_check diff`, xử lý 99 dòng bất đồng; sửa 3 dòng theo bạn (D01 `gaze_zone`, `face` ở S01) và export lại `export_final_v2.zip`.
- Điều mình học được hôm nay: ghế gọi theo xe chứ không theo ảnh (camera OMS nhìn về sau nên trái ảnh là `rear_right`); trên ảnh D## xoay 90° thì trái/phải xét theo người (mắt trái có y lớn hơn); không thấy thì ghi `unknown`, và trạng thái mắt phải khớp với cờ điểm `face13`.
