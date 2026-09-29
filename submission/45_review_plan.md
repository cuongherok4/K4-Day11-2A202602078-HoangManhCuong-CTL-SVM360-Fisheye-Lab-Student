# Kế hoạch review từ lỗi quan sát được

Vai C rà báo cáo r1 khóa 1277-D599, không suy rủi ro an toàn từ zone.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| 006840 — người bị che, xe nhỏ khó phân lớp | Local quality: 8 TP, 2 FP, 1 FN. Ba dòng compare: L10 IGNORE_SCOPE, L1 SPURIOUS, L5+R7 BOX_GEOMETRY. L11/R4/M12 thêm bất đồng class ba nguồn. | Scope R loại L10 khỏi mẫu số; L5/R7 đổi cách ghép theo IoU. Cần phân xử trước sửa để tránh xóa nhãn đúng hoặc thêm box trùng. | XML r1, reference.txt, local_quality_conflicts.csv, trung-diag-006840.png, D3/D5/D8. |
| 056040 — xe lớn bị cắt và người sát xe | Local quality: 7 TP, 2 FP, 0 FN; L4/L6 báo SPURIOUS. QA-L01 thêm geometry L9; QA-L02 là khoảng trống R03. | L9 cần sửa theo phần xe nhìn thấy dù ghép với R vẫn qua 0.5; L6 ban đầu nghi reference thiếu người; soát lại (D12) thấy là một người nên gộp vào L7 ở v2. Tách lỗi nhãn, rule và model. | long-qa-056040.png, trung-diag-056040.png, D2/D4/D6/D7; giữ v1 và v2 để B soát lại. |

036720 có 4 TP/0 FP/0 FN nhưng model vẫn tách rider và bỏ xe lớn; dùng làm ca kiểm đối chứng chứ không coi frame hoàn hảo theo mọi tiêu chí. Ba frame B1-center không đại diện cho các block khác hoặc hệ bốn camera. Số issue trong findings có thể lặp cùng vật qua vai/nguồn; không dùng tổng dòng làm tỷ lệ lỗi vật.

## Chuyển sang kế hoạch bốn camera giả lập

50.000 frame là tình huống đề bài, không có trong repo. Giữ phân bổ front 55, rear 45, left 50, right 50 (85 normal/115 hard, tổng 200) như lựa chọn ngân sách cần thử nghiệm. Chênh lệch 5–10 frame không xuất phát từ tần suất lỗi hoặc mức an toàn đã đo. Cột risk là ưu tiên review giả định; cập nhật sau pilot nếu metadata cho thấy nhóm thiếu độ phủ.

Lập bảng ứng viên gồm frame_id, camera_id, clip_id, timestamp, ngày/tuyến, điều kiện sáng/thời tiết, normal/hard và lý do hard. Phân loại normal/hard loại trừ nhau để một ảnh chỉ vào một ô. Chọn ngẫu nhiên có seed trong từng ô sau khi gắn tag; rải tối thiểu 5 clip mỗi camera nếu dữ liệu cho phép, cách nhau ít nhất 2 giây trong một clip và tối đa 5 frame/camera/clip ở lượt đầu. Đây là quy tắc chọn đề xuất, không khẳng định các mẫu đã được chọn.

Kiểm đủ 8 ô và tổng 200; kiểm frame_id duy nhất, thống kê theo clip/ngày/tuyến/thời tiết/class/che khuất/cắt biên. Ca seam ghi pair_id đồng bộ; hai ảnh hai camera tính là hai frame trong ngân sách nhưng một sự kiện liên quan, không hai bằng chứng độc lập. Nếu thiếu nguồn cho ô nào, ghi thiếu và xin điều chỉnh phân bổ, không lặp ảnh cho đủ.

Hard oversampling giúp tìm ca khó; không đo trực tiếp tỷ lệ lỗi trên 50.000 frame. Muốn ước lượng tỷ lệ toàn tập cần kích thước từng tầng, xác suất chọn và trọng số phù hợp hoặc tập đánh giá ngẫu nhiên riêng. Không suy khoảng cách từ center/mid/edge; cần calibration/geometry và metadata tác vụ để bàn về khoảng cách.
