# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 4 | 0 | 0 | 2 | 2 | — |
| mid | 9 | 1 | 4 | 3 | 8 | SPURIOUS (3) |
| edge | 7 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone **mid** gay nhieu nhat: L missing=7/9 ref (78%), M missing=3/9, M thua=8. Tiep theo edge: L missing=4/7, M missing=1.
- Nguyen nhan chinh: vung mid co nhieu xe nho (Bike H<60px) bi che khuat boi xe lon phia truoc nen annotator kho phat hien va model low-confidence. Vung edge meo fisheye manh lam IoU<0.5. Slice chi co 3 frame nen so lieu variance cao, khong the ngoai suy cho toan bo dataset.
