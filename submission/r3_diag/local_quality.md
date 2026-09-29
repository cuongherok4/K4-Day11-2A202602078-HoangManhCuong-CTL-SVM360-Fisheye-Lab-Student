# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `1277d5996d15ee76521746def2b4b7b9a89d64fae1befac365f4daca9573e81d`; slice `B1-center`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_006840.jpg, adasind_036720.jpg, adasind_056040.jpg. Frame thiếu trong export: không.
TP=19; FP=4; FN=1; số lần đối chiếu=24; mean IoU của TP=0.801.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.792 | 0.948 | 0.833 |
| precision | 0.826 | 0.839 | 0.500 |
| recall | 0.950 | 0.938 | 0.750 |
| jaccard | 0.792 | 0.821 | 0.429 |
| dice | 0.884 | 0.881 | 0.600 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 6 | 1 | 0 | 0.958 | 0.857 | 1.000 | 0.857 | 0.923 |
| Car | 3 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Pedestrian | 3 | 3 | 1 | 0.833 | 0.500 | 0.750 | 0.429 | 0.600 |
| ThreeWheeler | 7 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_006840.jpg | 8 | 2 | 1 | 0.727 | 0.800 | 0.889 |
| adasind_036720.jpg | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_056040.jpg | 7 | 2 | 0 | 0.778 | 0.778 | 1.000 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | <missing> |
|---|---:|---:|---:|---:|---:|
| Bike | 6 | 0 | 0 | 0 | 0 |
| Car | 0 | 3 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 3 | 0 | 1 |
| ThreeWheeler | 0 | 0 | 0 | 7 | 0 |
| <extra> | 1 | 0 | 3 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
