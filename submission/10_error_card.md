# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | ATTRIBUTE | 1 |
| center | B4 | BOX_GEOMETRY | 2 |
| center | B4 | IGNORE_SCOPE | 1 |
| center | B4 | SPURIOUS | 1 |
| center | C0 | BOX_GEOMETRY | 1 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B4 | ATTRIBUTE | 1 |
| edge | B4 | MISSING | 4 |
| mid | B4 | MISSING | 1 |
| unknown | B4 | BOX_GEOMETRY | 1 |

## Top defects
- MISSING: 6 (ví dụ frame adasind_019560.jpg)
- BOX_GEOMETRY: 4 (ví dụ frame adasind_019560.jpg)
- SPURIOUS: 2 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và lập luận**: Lỗi nổi bật nhất là `MISSING` (4 lỗi tại vùng `edge`, 1 tại `mid`, 1 tại `center`). Nguyên nhân chính là `E4_model_domain` và `E1_annotator_error`: vùng rìa thấu kính fisheye méo quang học rất lớn khiến model YOLO (vốn pre-train trên ảnh phối cảnh thẳng COCO) bị tụt độ tự tin và giảm IoU (< 0.5), đồng thời người gắn nhãn dễ bỏ sót các phương tiện nhỏ (Bike, Pedestrian) bị biến dạng dọc theo viền `lens_border`.
- **Cách sửa và phân công (`owner`)**:
  - `ai_team`: Fine-tune model phát hiện đối tượng trên tập dữ liệu fisheye góc rộng (ADASIND/FishEye8K) hoặc hiệu chỉnh camera un-distortion trước khi inference.
  - `guideline`: Cập nhật luật R02 quy định rõ bounding box bao tight quanh phần nhìn thấy dù bị méo, cho phép một phần khoảng trống do hình elip thấu kính.
  - `annotator`: Thực hiện rework các ca P0/P1 bị sót tại vùng center (như Truck H=88px tại `adasind_236370.jpg`).
- **Bằng chứng**:
  - Báo cáo so sánh `compare.html` và `model_compare.md` cho thấy ô `RM_noL` và `M_only` tập trung nhiều nhất ở rìa.
  - Phân tích IoU sweep trong `iou_sweep.md` cho thấy khi hạ ngưỡng IoU từ 0.5 xuống 0.3, số lượng matched box tăng rõ rệt, chứng minh vật có tồn tại nhưng box bị lệch do méo quang học.
  - Ảnh minh chứng lưu tại `submission/screenshots/01_local_quality.png` và `02_rework_delta.png`.
