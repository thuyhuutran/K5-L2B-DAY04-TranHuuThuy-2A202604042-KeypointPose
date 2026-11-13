# Báo cáo cá nhân — HoVaTen-MSSV

Đổi tên file theo quy tắc `MSSV_HoTen.md`. Ví dụ: `20241234_NguyenVanAn.md`.

Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Trần Hữu Thủy
- MSSV: 2A202604042
- Email: <huuthuytrann3004@gmail.com>
- Link repository: <https://github.com/thuyhuutran/K5-L2B-DAY04-TranHuuThuy-2A202604042-KeypointPose-Student>
- Phiên bản CVAT (Help → About):
- Máy: hệ điều hành Window · RAM 16 GB · chạy CVAT chế độ đầy đủ / lite
- Bộ ảnh (src/exports/assignment.json do lab.py setup tạo): có
- Giờ bắt đầu: **17:06**
- Giờ kết thúc:

## 1. Kiểm tra label (CP0)

| Label    | Kiểu       | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
| -------- | ---------- | ------: | --------------------------- | ------------ | ------: |
| `person` | `skeleton` |      17 | Có                          | Có           |      19 |
| `face`   | `skeleton` |      13 | Có                          | Có           |      13 |

Mẫu `person` dựng theo người quay mặt về camera (các điểm `left_*` nằm bên phải khung mẫu)? Trả lời: Có

Cách đổi cờ `occluded` / `outside` cho **một điểm** trên phiên bản CVAT này (một câu):
Chọn điểm đó trên ảnh hoặc trong danh sách bên phải, bấm biểu tượng mắt gạch (outside) / biểu tượng occluded của điểm đó, hoặc dùng phím tắt O (outside) và Q, / (occluded).

## 2. Số lượng đã gán và thời gian (CP1, CP2)

| Nhóm ảnh   | Số ảnh | Số skeleton đã gán | Số người dưới 60 px (không gán) | Số poster / gương / màn hình bỏ qua | Thời gian (phút) |
| ---------- | -----: | -----------------: | ------------------------------: | ----------------------------------: | ---------------: |
| `easy`     |      5 |                  5 |                               0 |                                   0 |               20 |
| `occluded` |      3 |                  6 |                               0 |                                   0 |               25 |
| `back`     |      2 |                  4 |                               0 |                                   0 |               20 |
| `small`    |      1 |                  0 |                               4 |                                   0 |               10 |
| `crowd`    |      1 |                  3 |                               0 |                                   0 |               15 |
| `faces`    |      8 |                  8 |                               — |                                   0 |               30 |

- Số ảnh người đã xong ở mốc phút 105: 8 / 10
- Thời gian thực tế (phút): CP1 ~90 · CP2 ~30 · CP3 ~20
- Ảnh tốn thời gian nhất là ảnh nào, vì sao?
  Ảnh 000000326480: nhiều người, Vì nhiều điểm bị che nên phải cân nhắc cờ và trái/phải cho từng người.

## 3. Bảng ca khó (CP1, CP2)

Mỗi điểm bạn phân vân một dòng, nhất là phân vân giữa `v = 1` và `v = 0`, hoặc không chắc hướng người.

| Ảnh    | Người / mặt                 | Điểm                           | Cờ đã chọn | Phân vân giữa        | Lý do chọn                                                                                           |
| ------ | --------------------------- | ------------------------------ | ---------: | -------------------- | ---------------------------------------------------------------------------------------------------- |
| 157271 | Người áo trắng              | `nose`, `left_eye`, `left_ear` |          1 | `v = 1` / `v = 0`    | Cẳng tay người áo xanh che ngang mặt nhưng vẫn thấy hướng đầu, nên đoán chắc được vị trí             |
| 360811 | Cô gái (trái ảnh)           | `left_ear`, `right_ear`        |          1 | `v = 1` / `v = 0`    | Tóc dài che hai tai nhưng đầu cúi thẳng, đoán được vị trí tai                                        |
| 360811 | Cả hai người                | `hip`, `knee`, `ankle`         |          0 | `v = 1` / `v = 0`    | Bàn che hết thân dưới, hai người đang ngồi nên không đoán chắc được khớp                             |
| 467300 | Người đàn ông đứng sau      | `left_shoulder`                |          1 | `v = 1` / `v = 0`    | Vai bị đầu cô dâu che hoàn toàn, suy ra từ bề ngang vai và vai còn lại                               |
| 467300 | Cô dâu                      | `left_hip`                     |          1 | `v = 1` / `v = 0`    | Hông bị cánh tay và váy che, đoán theo đường eo và hông còn lại                                      |
| 210604 | Hai người đang nhảy         | `hip`, `knee`, `ankle`         |          2 | điểm thuộc người nào | Hai người chồng lên nhau, chân đan xen; tôi lần theo từng chân từ hông xuống để biết thuộc người nào |
| 480582 | Người quay lưng (trái ảnh)  | `nose`, `eye`, `ear`           |          0 | `v = 1` / `v = 0`    | Quay lưng hoàn toàn nên không thấy mặt, không có cơ sở đoán vị trí                                   |
| 480582 | Người quay lưng (trái ảnh)  | `knee`, `ankle`                |          1 | `v = 1` / `v = 0`    | Đàn cừu che hai chân nhưng thân đứng thẳng nên đoán được vị trí                                      |
| 326480 | Người áo xanh lá (phải ảnh) | `left_ear`                     |          1 | `v = 1` / `v = 2`    | Điện thoại và tay áp vào tai, tai bị che nhưng đoán được vị trí                                      |
| 326480 | Ba người                    | `wrist`                        |          2 | điểm thuộc người nào | Ba người đứng sát nhau, tay chồng ở giữa; tôi lần theo cánh tay từ vai để biết cổ tay của ai         |

## 4. Bảng tự review (CP3)

Đi từ ảnh cuối về ảnh đầu. 5 ảnh người (ít nhất 2 ảnh `back` hoặc `crowd`) và 3 ảnh mặt. Ít nhất 8 dòng, kể cả dòng `ok`. Ghi lỗi trước, sửa sau.

Mã lỗi dùng đúng một trong: `lr_swap`, `vis_flag`, `position`, `missing`, `extra`, `ok`.

| Ảnh         | Người / mặt                | Điểm                                                                            | Mã lỗi | Ghi chú                                                                                                   | Đã sửa? |
| ----------- | -------------------------- | ------------------------------------------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------- | ------- |
| 480582      | Người quay lưng (trái ảnh) | `left_*` / `right_*`                                                            | `ok`   | Quay lưng nên `left_*` nằm bên trái ảnh, đúng hướng; chân bị đàn cừu che đã đặt cờ che                    | —       |
| 467300      | Cô dâu                     | `left_hip`, `right_hip`                                                         | `ok`   | Hông bị cánh tay và váy che, vị trí hợp lý theo đường eo                                                  | —       |
| 360811      | Cô gái, người đàn ông      | `left_hip`, `right_hip`, `left_knee`, `right_knee`, `left_ankle`, `right_ankle` | `ok`   | Bàn che hết nửa thân dưới nên không đặt điểm (`v = 0`)                                                    | —       |
| 326480      | Ba người (crowd)           | `left_wrist`, `right_wrist`                                                     | `ok`   | Ba người đứng sát nhau; cổ tay đã lần theo cánh tay từ vai, không nhầm người                              | —       |
| 210604      | Hai người đang nhảy        | `left_hip`, `right_hip`, `left_knee`, `right_knee`, `left_ankle`, `right_ankle` | `ok`   | Chân hai người đan xen nhưng mỗi chân gắn đúng với người của nó; áo xanh quay lưng, `left_*` bên trái ảnh | —       |
| face_421231 | Mặt                        | `left_eye_*`, `right_eye_*`, `mouth_left`, `mouth_right`                        | `ok`   | Mắt trái (đỏ) và `mouth_left` nằm bên phải ảnh, khóe ngoài xa sống mũi hơn khóe trong                     | —       |
| face_415073 | Mặt nghiêng                | `mouth_right`                                                                   | `ok`   | Mặt nghiêng nên khóe miệng bên trái người không đặt điểm (`v = 0`); trái/phải hai mắt đúng                | —       |
| face_300399 | Mặt thẳng                  | `nose_tip`, mí mắt                                                              | `ok`   | Trái/phải đúng, mí trên và mí dưới nằm ở mép mí quanh tâm con ngươi                                       | —       |

Tổng hợp số lỗi theo mã:

| `lr_swap` | `vis_flag` | `position` | `missing` | `extra` | `ok` |
| --------: | ---------: | ---------: | --------: | ------: | ---: |
|         0 |          0 |          0 |         0 |       0 |    8 |

Điểm còn phân vân giữa `v = 1` và `v = 0` (ghi cả hai lựa chọn và lý do chọn):

- 360811, thân dưới: chọn `v = 0` thay vì `v = 1`, vì hai người ngồi sau bàn nên không đoán chắc được vị trí khớp.
- 467300, `left_shoulder` của người đàn ông: chọn `v = 1` thay vì `v = 0`, vì suy ra được từ bề ngang vai và vai còn lại.
- 480582, chân người quay lưng: chọn `v = 1` thay vì `v = 0`, vì cừu che chân nhưng thân đứng thẳng nên đoán được.
- face_415073, `mouth_right`: chọn `v = 0` thay vì `v = 1`, vì mặt nghiêng nên khóe miệng bên trái người bị che và không đoán chắc được vị trí.

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

Tự kiểm tay trong file JSON:

| File                      | Người / mặt kiểm (ảnh, thứ tự)                                                                                                         | Số phần tử `keypoints` | Điểm `outside` ra `v = 0`, `occluded` ra `v = 1`?             | `num_keypoints` khớp?                      |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ---------------------: | ------------------------------------------------------------- | ------------------------------------------ |
| `TranHuuThuy_persons.zip` | 000000157271, người #1 (ann id 1): `nose` = 1; `left_eye`, `right_eye`, `left_ear`, `left_elbow` = 0                                   |                     51 | Có: 4 điểm `outside` ra `v = 0`, 1 điểm `occluded` ra `v = 1` | Có (13 = 12 điểm `v = 2` + 1 điểm `v = 1`) |
| `TranHuuThuy_persons.zip` | 000000467300, người #1 (ann id 13): `left_shoulder`, `left_hip` = 1; `right_ear`, `left_elbow`, `left_wrist`, hai gối, hai cổ chân = 0 |                     51 | Có: 7 điểm `v = 0`, 2 điểm `v = 1`                            | Có (10 = 8 điểm `v = 2` + 2 điểm `v = 1`)  |
| `TranHuuThuy_faces.zip`   | face_415073 (ann id 4): `mouth_right` = 0, 12 điểm còn lại `v = 2`                                                                     |                     39 | Có: 1 điểm `v = 0`                                            | Có (12 = 12 điểm `v = 2`)                  |

Sau khi xuất, CVAT ghi `occluded` thành `v = 1` và `outside` thành `v = 0`. Tọa độ của điểm `outside` trong file là `0, 0` hay tọa độ cũ? **Tọa độ cũ**: ví dụ ở 000000157271 người #1, `left_eye` có `v = 0` nhưng vẫn mang tọa độ (450.1, 77.19); có điểm còn mang tọa độ âm (ngoài ảnh), ví dụ (-58.51, 171.52) ở 000000162046.

Hai job đã chuyển sang completed trên CVAT? Có

Dán phần **TÓM TẮT** in ra từ `src/check_submission.py`:

```text
----- TÓM TẮT (dán vào báo cáo mục 5) -----
- File person: TranHuuThuy_persons.zip
  Số ảnh: 12 · số skeleton `person`: 18 · mỗi annotation 51 số
  Skeleton theo ảnh: 000000157271=2, 000000162046=1, 000000167027=0, 000000210604=2, 000000326480=3, 000000360811=2, 000000390585=1, 000000433787=1, 000000467300=2, 000000480582=2, 000000558066=1, 000000564304=1
  Cờ v: v=2 213 (70%) · v=1 17 (6%) · v=0 76 (25%)
  num_keypoints trung bình: 12.8
- File face: TranHuuThuy_faces.zip
  Số ảnh: 8 · số skeleton `face`: 8 · mỗi annotation 39 số
  Skeleton theo ảnh: face_185015=1, face_296767=1, face_300399=1, face_415073=1, face_421231=1, face_425647=1, face_504911=1, face_79367=1
  Cờ v: v=2 93 (89%) · v=1 0 (0%) · v=0 11 (11%)
  num_keypoints trung bình: 11.6
- Ghi chú: 87 điểm v = 0 vẫn mang tọa độ cũ (CVAT 2.x xuất như vậy; chuẩn COCO ghi 0, 0). Không phải lỗi: điểm v = 0 bị bỏ qua khi huấn luyện và khi chấm.
- Số [LỖI]: 0 · số [CẢNH BÁO]: 1
Kết quả: ĐẠT (đọc lại từng cảnh báo ở trên)
```

Các `[CẢNH BÁO]` còn giữ nguyên và lý do:

- `000000167027.jpg`: không có skeleton `person` nào. Ảnh nào cũng có ít nhất một người cần gán.

## 6. Ảnh chụp màn hình

1. Label `person` trong label constructor (đủ 17 điểm có tên):

   ![Label person](images/1_label_person.png)

2. Một người quay lưng đã gán, cờ đã đặt:

   ![Người quay lưng](images/2_nguoi_quay_lung.png)

3. Một khuôn mặt đã gán 13 điểm, phóng to vùng mắt:

   ![Khuôn mặt phóng to](images/3_mat_phong_to.png)

4. Một đoạn JSON xuất của một người:

   ![JSON export](images/4_json_export.png)

Nguồn ảnh: COCO val2017 / Wikimedia Commons.

## 7. Tự đánh giá

1. Lỗi bạn mắc nhiều nhất hôm nay, và cách bạn sẽ tránh ở dự án thật:
   - Lỗi tôi mắc nhiều nhất là nhầm trái/phải (`lr_swap`), nhất là ở người quay lưng hoặc nghiêng, vì quen đặt `left_*` theo mẫu quay mặt về camera. Ở dự án thật tôi sẽ nói ra hướng người trước khi kéo điểm, bật hiển thị tên điểm, và nhìn lại đường nối sau mỗi người (chân hoặc tay bắt chéo bất thường là dấu hiệu đổi trái/phải).

2. Một quy tắc bạn đề xuất thêm vào guideline (kèm ảnh ví dụ nếu có):
   - Không có

3. Nhãn của bạn đi vào model ở đâu? Chọn một lỗi bạn đã sửa và nói nó sẽ làm model học sai thế nào (heatmap, trọng số trong loss, hay bước lật ảnh):
   - Nhãn đi vào heatmap (mỗi khớp một kênh), trọng số loss (`v = 0` không bị phạt) và bước lật ảnh (đổi kênh `left_*` với `right_*`). Với lỗi `lr_swap` ở người quay lưng, điểm đặt đúng khớp nhưng sai tên nên đỉnh heatmap nằm sai kênh, và khi lật ảnh nhãn này mâu thuẫn với các ảnh đúng khác nên model học lẫn trái/phải.

## 8. Khó khăn và lỗi kỹ thuật

- Ở ảnh `000000167027.jpg` , tôi không biết cách xác định đối tượng có cao hơn 60px hay không?
  Tôi đã giải quyết bằng cách thêm một label tên là `rec` để có thể gán vào kiểm tra được chiều cao đối tượng đó.

## Bonus (nếu làm)

### B1 — 3 ảnh người khó thêm

| Ảnh | Số skeleton | Số người dưới 60 px | Ca khó nhất |
| --- | ----------: | ------------------: | ----------- |
|     |             |                     |             |
|     |             |                     |             |
|     |             |                     |             |

Kết quả `src/check_submission.py` cho `HoVaTen_bonus_persons.zip`:

```text
(dán ở đây)
```

### B2 — 5 ảnh khuôn mặt đen trắng / hồng ngoại

Ảnh không màu khó hơn ở điểm nào? Kính râm trên ảnh hồng ngoại có còn là `v = 1` không?

...

### B3 — Model so với nhãn của tôi (không tính điểm)

- Demo đã dùng:
- Ảnh đã thử:

1. Model có trả điểm cho khớp bị che, hay khớp ngoài ảnh không? Confidence ở các điểm đó thế nào?

   ...

2. Với người quay lưng, model có đổi trái/phải không?

   ...

3. Bộ điểm của model có giống 17 điểm COCO không?

   ...

## 9. Tài liệu tham khảo và công cụ AI đã dùng

...
