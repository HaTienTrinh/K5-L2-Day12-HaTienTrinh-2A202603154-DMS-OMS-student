# Tự đánh giá — HaTienTrinh · G10

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S08_rgb / S08_ir | `child_seat` bên trái ảnh | `missing` | Vẽ box xong quên chọn `seat`, còn để mặc định `driver` (cabin_check WARN khi soát S01–S08); `occupied` cũng còn mặc định `unknown` | Có: `seat = rear_right`, `occupied = no` |
| S01_rgb / S01_ir | `face` của người `rear_left` | `extra` | Đã chỉnh thuộc tính `eye_left = open`, `glasses = none` trong khi OMS chỉ cần box (GUIDE 4.1). Review chéo chỉ ra | Có, đưa về mặc định trong `export_final_v2.zip` |
| D05 | `face.eye_left` / `eye_right` | `lr_flip` | Ghi `eye_left = unknown`, `eye_right = open` nhưng điểm `face13` thì mắt trái thấy rõ, 4 điểm mắt phải `outside` (cabin_check 2 WARN) | Có: `eye_left = open`, `eye_right = unknown` |
| D03 | `face13` | `ok` | Tài xế quay đầu mạnh nên các điểm không theo quy ước của ảnh xoay 90° (mắt trái không có y lớn hơn). Đã kiểm lại trên ảnh: trái/phải theo người | Không cần sửa |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: để sót giá trị mặc định của CVAT hoặc chọn thuộc tính không khớp với những gì đã vẽ: S08 `seat = driver` và `occupied = unknown` (cả `_rgb` lẫn `_ir`), D05 trạng thái hai mắt bị đảo so với cờ điểm `face13`.

Vì sao mình mắc: vẽ box và đặt điểm xong thì chuyển sang đối tượng khác ngay, không mở lại **DETAILS** để soát từng thuộc tính; khi copy từ `_rgb` sang `_ir` thì lỗi bị nhân đôi.

Lần sau tránh bằng cách: vẽ xong mỗi box thì chọn `seat` ngay; sau khi đặt `face13` thì soát từng mắt (điểm `outside`/`occluded` ↔ mắt `unknown`, điểm thấy rõ ↔ `open`/`closed`); soát xong ảnh `_rgb` rồi mới copy sang `_ir`.

## 3. Ghi chú về disagreement_log

Review chéo với Vũ Tuyết Trinh: 99 dòng, 96 `mine`, 3 `peer`.

Dòng bạn đúng, mình sai: D01 `gaze_zone`. Mình ghi `other` vì không thấy được mắt, nhưng tài xế đang cúi đầu nhìn xuống đùi nên theo hướng đầu thì là `lap` (GUIDE 4.5 xét vùng nhìn theo hướng mắt **và đầu**). Mình học được: không thấy mắt thì `eye_*` là `unknown`, nhưng `gaze_zone` vẫn có thể xác định từ hướng đầu, không phải lúc nào cũng là `other`.

Phần lớn bất đồng còn lại đến từ ba lỗi hệ thống của bạn: gọi ghế theo ảnh thay vì theo xe (20 dòng), vẽ `person`/`face` cho trẻ trong ghế (14 dòng), vẽ `object` trên ảnh D## (7 dòng).

## 4. Ghi chú quy trình

- Checkpoint A (S01–S02): 0 lỗi, 0 cảnh báo. Soát thêm S01–S08 thấy 2 WARN ở S08 (`seat = driver`), sửa trước Checkpoint B.
- Checkpoint B (D01–D02): 0 lỗi, 0 cảnh báo.
- Kiểm cuối lần đầu: 0 lỗi, 2 WARN ở D05 (mắt đảo), sửa rồi export lại: `check_final.txt` 0 lỗi, 0 cảnh báo.
- Sau review chéo: sửa D01 `gaze_zone = lap` và thuộc tính `face` ở S01, export `export_final_v2.zip`: `check_final_v2.txt` 0 lỗi, 0 cảnh báo.
- Coach gửi `labels_cabin_v1.json` và `cabin_schema.py` cập nhật: `child_seat.occupied` có thêm `yes` (có trẻ ngồi, không vẽ box cho trẻ). GUIDE.md vẫn ghi "không có `yes`"; bài này theo schema coach cập nhật.
- `gaze_zone = other` ở D02 vì tài xế nhắm mắt, không có hướng nhìn.
