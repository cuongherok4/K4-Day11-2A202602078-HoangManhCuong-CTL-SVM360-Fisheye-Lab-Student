# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 10 | 0 | 2 | 5 | 6 | SPURIOUS (2) |
| mid | 7 | 1 | 2 | 3 | 6 | BOX_GEOMETRY (1) |
| edge | 3 | 0 | 0 | 2 | 5 | — |

## Nhận xét

L có mid missing=1/spurious=2 (7 reference), center missing=0/spurious=2 (10 reference), edge 0/0 (3 reference). Mid là nhóm nhiều bất đồng ghép của L nhất theo tổng thô 3, không phải tỷ lệ rủi ro. M có center missing=5/thừa=6, mid 3/6, edge 2/5; center nhiều sự kiện thô nhất (11), còn edge có rất ít reference nên không so tổng thô như mức khó tuyệt đối.

Model nhiều lần gọi ThreeWheeler là Car/Truck (006840 M13/M8/M10, 036720 M4, 056040 M5), hoặc tách rider khỏi Bike (036720 M1/M2/M6, 056040 M2/M3). Vì ghép cùng class, sai class có thể hiện MISSING cộng SPURIOUS; không kết luận model không phát hiện vùng vật. Lệch taxonomy/đơn vị gán nhãn là giả thuyết có bằng chứng lặp, nhưng không chứng minh nguyên nhân do méo fisheye hay mô hình huấn luyện miền nào.

Xe lớn 036720 L4/R1 và 056040 L9/R5 không có M tương ứng: blur, cắt biên hoặc ngưỡng confidence đều có thể góp phần. Cần thêm frame/block và kiểm cấu hình model, không quy hết về edge. M5/036720 và M10/056040 trên ego bị ignore reference loại khỏi bảng; không lẫn tổng detection gốc với mẫu số đã lọc.

IoU sweep: L matched tổng 20 ở 0.3, 19 ở 0.5, 14 ở 0.7; M lần lượt 11, 10, 6. Ngưỡng làm thay đổi kết quả trên cùng nhãn; ba frame không chứng minh chất lượng toàn camera/bốn camera. Bảng issue đếm theo quy tắc lab, không phải tỷ lệ lỗi sản xuất.
