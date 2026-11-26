# Báo cáo Day 14 — Phan Hải Đăng — 2A202603229

- Mã học viên dùng với `--student`: `229`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

- Tham số tôi nghĩ giảm công sửa nhiều nhất: **`score_thr`** (tăng lên khoảng `0.3`–`0.4`) kết hợp **`min_points`** (khoảng `5`).
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Trên ảnh BEV và số đo tập A, **box thừa (FP) áp đảo hoàn toàn** so với box sót (dự đoán `83` box nhưng GT chỉ `37` box, phải xóa `52` box trong khi chỉ thêm `6` box). Phần lớn box thừa là nhiễu ở rìa đường/bụi cây/hàng rào có `score` thấp (ví dụ `0.18`) hoặc rất ít điểm LiDAR. Dù xóa 1 box chỉ tốn `0.3` (thêm 1 box tốn `1.0`), lượng box thừa quá lớn (`52 × 0.3 = 15.6`, chiếm hơn nửa tổng chi phí `27.1`) nên tăng `score_thr` và `min_points` vừa phải sẽ cắt mạnh `xóa` mà chưa làm `thêm` tăng vọt.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | 52 | 6 | 11 | 27.1 |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_229/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_229/frames_B.txt --config d14_229/best_config.json --gt d14_229/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
