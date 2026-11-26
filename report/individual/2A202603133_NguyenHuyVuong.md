# Báo cáo Day 14 — Nguyễn Huy Vương — 2A202603133

> Sao chép file này thành `report/individual/<MSSV>_<HoTen>.md` (không dấu), ví dụ `2A20260000_NguyenVanAn.md`.

- Mã học viên dùng với `--student`: `133`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: Tăng ngưỡng độ tin cậy `score_thr` từ mức mặc định `0.1` lên khoảng `0.3` – `0.4` (kết hợp lọc số điểm tối thiểu `min_points=5`).
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Nhìn trên ảnh BEV và 7 góc camera của 3 frame tập A (`s03f44`, `s04f17`, `s05f27`), số lượng box thừa (false positives) chiếm áp đảo so với số box bị sót: cấu hình gốc sinh ra tới 92 box nhưng thực tế chỉ có 49 vật thể thật (phải xóa tới 52 box thừa, thêm 9 box sót và sửa 5 box, tổng chi phí gốc 27.1; riêng `s03f44` có tới 31 box thừa do model nhận nhầm hàng cây/bụi rậm ven đường thành `Cyclist` và `Car` ở điểm tin cậy thấp `0.24–0.38`, cùng các box nhận nhầm chính thân xe ego hoặc hộp rỗng rất ít điểm). Vì phần lớn các box nhiễu này có `score < 0.4` hoặc ít điểm LiDAR, việc nâng `score_thr` lên `0.3–0.4` và `min_points=5` sẽ loại bỏ hàng loạt hộp thừa mà ít làm mất các xe thật gần có `score` cao (`> 0.5`), giúp giảm mạnh thành phần `0.3 * xóa` trong tổng chi phí sửa.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_133/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_133/frames_B.txt --config d14_133/best_config.json --gt d14_133/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
