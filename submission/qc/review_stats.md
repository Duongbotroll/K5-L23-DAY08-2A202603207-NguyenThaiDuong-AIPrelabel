# Số đo review (det)

- Ảnh tính: 24 (batch,gold); box máy: 437; đối tượng sau duyệt: 430; ngưỡng accepted: IoU >= 0.9
- Đã **loại 21 đối tượng nằm quanh các bẫy** khỏi bảng này: lỗi nhân tạo không phản ánh chất lượng máy.

## 1. Bốn trạng thái (AI error profile)
| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 410 | 410/437 = 93.8% | 437 box máy |
| edited (sửa) | 0 | 0/437 = 0.0% | 437 box máy |
| deleted (xoá) | 23 | 23/437 = 5.3% | 437 box máy |
| added (vẽ thêm) | 20 | 20/430 = 4.7% | 430 đối tượng sau duyệt |

Trong số deleted + added, **2 trường hợp là đổi lớp** (máy gán sai lớp, box đúng chỗ): đó không phải máy bỏ sót. Số vật máy **sót thật** ≈ added − đổi lớp = 18.

### Theo lớp
| Lớp | accepted | edited | deleted | added |
|---|---|---|---|---|
| person | 179 | 0 | 7 | 5 |
| bicycle | 6 | 0 | 2 | 1 |
| motorbike | 137 | 0 | 1 | 1 |
| car | 50 | 0 | 1 | 12 |
| bus | 2 | 0 | 1 | 1 |
| truck | 5 | 0 | 1 | 0 |
| three_wheeler | 31 | 0 | 10 | 0 |

## 2. So với gold (nhãn gán tay từ đầu)
Ảnh gold: 4; đối tượng gold: 37 (mẫu số của recall)
| | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Máy (pre-label) | 33 | 21 | 4 | 61.1% (KTC 95%: 48–73%) | 89.2% (KTC 95%: 75–96%) |

*Lạc quan: ngưỡng được chọn trên chính các ảnh gold này (in-sample), và chỉ có 4 ảnh. Đọc khoảng tin cậy, đừng đọc con số đơn.*


(Ảnh gold không nằm trong nhãn sau duyệt: không tính residual / new error. Đó là chủ ý: duyệt lại ảnh bạn vừa tự gán tay sẽ đo trí nhớ, không đo thiên kiến.)

## 3. Ảnh bẫy
- Bắt được 4/12 = 33.3% (KTC 95%: 14–61%) (ngưỡng đạt: 80%). Chỉ 12 bẫy nên khoảng tin cậy rất rộng: đọc cùng danh sách bẫy lọt, đừng kết luận chỉ từ %.
  - wrong_class: 1/3
  - extra: 3/3
  - missing: 0/3
  - loose: 0/3
  - lọt: img_004.jpg (wrong_class), img_033.jpg (missing), img_005.jpg (missing), img_029.jpg (loose), img_028.jpg (loose), img_010.jpg (wrong_class), img_025.jpg (missing), img_031.jpg (loose)

## Gợi ý đọc số
- `deleted` cao → ngưỡng thấp hoặc ánh xạ nhãn sai; `added` cao → máy bỏ sót (xem lớp nào);
- `accepted` thấp (< 50%) → sửa mỗi box lâu hơn vẽ mới: đổi model/ngưỡng hoặc vẽ tay;
- bắt bẫy < 80% → duyệt lại lô, siết checklist (quét ảnh TRƯỚC khi nhìn nhãn máy).
