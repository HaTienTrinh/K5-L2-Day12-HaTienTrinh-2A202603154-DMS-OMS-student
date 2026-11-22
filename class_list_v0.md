# Danh sách lớp v0 — HaTienTrinh · G10

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face.eye_left`, `face.eye_right` | open / closed / unknown | Phát hiện buồn ngủ: mắt nhắm kéo dài qua nhiều khung, tần suất chớp mắt | Có khi thấy vùng mắt; cúi đầu hoặc quay đi thì `unknown` |
| 2 | `face.gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung: rời mắt khỏi đường quá lâu, nhìn xuống đùi hoặc màn hình giữa | Có, dựa vào hướng đầu và mắt; khi không thấy mắt vẫn xét được hướng đầu |
| 3 | `face` + `face13` (13 điểm) | box + 13 điểm, cờ occluded / outside | Định vị mặt; tính tư thế đầu (yaw, pitch) và độ mở mắt, miệng (ngáp) | Có; điểm bị tay hoặc điện thoại che phải có cờ |
| 4 | `face.glasses` | none / clear / sun / unknown | Biết khi nào trạng thái mắt kém tin cậy; điều chỉnh ngưỡng phát hiện buồn ngủ | Một phần: trên NIR kính râm có thể trông trong suốt, dễ nhầm với `clear` |
| 5 | `hand` + `on_wheel` | yes / no / unknown (kèm `whose`, `side`) | Cảnh báo rời tay khỏi vô lăng khi xe ở chế độ tài xế phải cầm lái | Có, nhận bằng hình dạng bàn tay |
| 6 | `phone` + `state` | held / mounted / lap | Phát hiện dùng điện thoại khi lái (`held`, `lap`); `mounted` thường là dẫn đường | Một phần: màn hình không sáng như trên RGB, phải nhận bằng cạnh, góc |
| 7 | `person.seatbelt` (tài xế) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây; phát hiện dây đeo sai vị trí (dưới nách, sau lưng) | Một phần: dây tối trên áo tối khó thấy |

## OMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` + `seat` | rear_left / rear_middle / rear_right / front_passenger | Đếm người theo từng ghế: nhắc thắt dây hàng ghế sau, điều chỉnh túi khí theo ghế có người | Có, nhận bằng hình dạng |
| 2 | `person.seatbelt` | worn_correct / worn_incorrect / not_worn / unknown | Cảnh báo hành khách chưa thắt dây, theo từng ghế | Một phần: dây chìm trong áo tối hoặc bóng tối thì `unknown` |
| 3 | `child_seat` + `occupied` | no / unknown / yes | Nhắc quên trẻ trong xe khi khóa cửa; biết ghế đó đang có trẻ | Có (hình dạng ghế); bên trong ghế khó thấy khi tối hoặc bị phủ |
| 4 | `infant_seat` + `facing` | rear / front | Tắt túi khí khi nôi quay ra sau đặt ở ghế phụ trước; nhắc quên trẻ | Có (hình dạng vỏ nôi, quai xách) |
| 5 | `object` + `type` | bag / box / cup / other | Nhắc quên đồ khi rời xe; tránh nhầm vật trên ghế là người (báo dây an toàn sai) | Có (hình dạng); không dựa vào màu |
| 6 | `face` (OMS, chỉ box) | box | Xác nhận ghế có người thật chứ không phải vật; ước lượng tư thế, chiều cao hành khách | Một phần: người quay đi thì không thấy mặt |
| 7 | `frame_meta.camera_blocked` | yes / no | Phát hiện camera bị che để báo hệ thống OMS/DMS không hoạt động | Có |

## 2 câu hỏi leo thang

1. Câu hỏi: `infant_seat` không có thuộc tính `occupied`, nên khi có em bé nằm trong nôi thì nhãn không ghi được điều này, trong khi đây chính là ca nguy hiểm nhất cho chức năng nhắc quên trẻ. Bản 1.x có nên thêm `infant_seat.occupied` (no / unknown / yes) giống `child_seat` không?
   Khung ví dụ: G10 S05_rgb, `infant_seat` ở `rear_right` (S03 `rear_middle`, S07 `rear_right` tương tự).
   Bạn tạm gán thế nào và vì sao: chỉ vẽ `infant_seat` với `seat` và `facing`, không vẽ box cho em bé, theo hướng dẫn coach (trẻ trong ghế không vẽ box). Thông tin "có em bé" bị mất.
2. Câu hỏi: Khi tài xế nhắm mắt thì `gaze_zone` nên ghi gì? Ghi `road` theo hướng đầu sẽ dạy model rằng mắt nhắm vẫn là "nhìn đường"; ghi `other` thì lẫn với "nhìn chỗ khác". Có nên thêm giá trị riêng như `none` / `eyes_closed` không?
   Khung ví dụ: G10 D02 (`eye_left = closed`, `eye_right = closed`).
   Bạn tạm gán thế nào và vì sao: `gaze_zone = other` và ghi lý do "mắt nhắm, không có hướng nhìn" trong `self_review.md`, theo GUIDE 4.5 (không chắc thì `other`, ghi lý do).
