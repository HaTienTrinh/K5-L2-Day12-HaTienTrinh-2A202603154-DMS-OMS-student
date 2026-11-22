# Khai báo dùng AI — HaTienTrinh · 2A202603154 · G10

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude Code (Claude Opus 5.5, trong VS Code) | Đọc repo và tóm tắt các bước của lab (Task 0–4, checkpoint, review chéo, nộp bài) | Dùng làm checklist làm bài | Có, đối chiếu README, GUIDE, CHECKPOINTS, SUBMISSION |
| 2 | Claude Code | Giải thích guideline: `camera_blocked`, `seat`/`seatbelt` (ghế theo xe), `child_seat` vs `infant_seat`, `occupied`, `facing`, thuộc tính `face`/`hand`/`phone`, nhãn nào dùng cho S## và D##, `gaze_zone` khi mắt nhắm, box chỉ ôm phần thấy được | Tự áp dụng khi gán nhãn trên CVAT | Có, đối chiếu từng mục GUIDE §3, §4.1–4.8 mà AI trích dẫn |
| 3 | Claude Code | Cập nhật `scripts/cabin_schema.py` theo bản coach gửi (`child_seat.occupied` thêm `yes`) cho khớp `labels_cabin_v1.json` | `cabin_check` kiểm theo đúng schema coach cập nhật | Có, AI so nội dung JSON với schema: khớp |
| 4 | Claude Code | Chạy `cabin_check` (Checkpoint A, B, soát S01–S08, kiểm cuối, kiểm v2) và đọc `annotations.xml` (toạ độ + thuộc tính) để chỉ ra chỗ cần xem lại: S08 `seat = driver`, D05 trạng thái mắt không khớp cờ điểm, D03 hướng điểm `face13`, D01 `glasses`/`gaze_zone` | Tự mở lại ảnh trên CVAT, tự quyết định sửa hay giữ (sửa S08, D05; giữ D03) | Có, chạy lại `cabin_check`: `check_final.txt` và `check_final_v2.txt` 0 lỗi, 0 cảnh báo |
| 5 | Claude Code | Chạy `cabin_check diff` với file của Vũ Tuyết Trinh, phân nhóm 99 dòng bất đồng; điền sẵn các dòng phân xử được bằng guideline (ghế theo xe, trẻ trong ghế, `object` trên D##, thiếu `face13`, thuộc tính `face` trên S##) | Tôi tự quyết định các dòng cần xem ảnh (D01–D06, S05, S06, S08) rồi AI ghi vào `disagreement_log.csv`; sửa D01 và S01, export `export_final_v2.zip` | Có, đọc lại log; `diff` v1–v2 chỉ ra đúng 5 thay đổi đã định |
| 6 | Claude Code | Viết nháp `self_review.md`, `class_list_v0.md`, `ir_log.csv`, `TEAM.md` từ số liệu bài G10; tạo khung `exit_ticket.md` (chỉ điền tên, mã) | Đọc lại, sửa câu chữ và phần quan sát ảnh trong `ir_log.csv` theo những gì tôi thấy trên CVAT; tự trả lời exit ticket | Có, đọc lại từng file và đối chiếu với ảnh trên CVAT |

