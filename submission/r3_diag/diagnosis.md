# Phân tích chất lượng — vai C

Nguồn cố định: B1-center, r1 SHA-256 `1277d5996d15ee76521746def2b4b7b9a89d64fae1befac365f4daca9573e81d`. Reference mở sau QA P3. Codex đã làm QA rồi tiếp tục vai C theo yêu cầu; đây không phải thêm một reviewer con người độc lập.

Local quality ở IoU 0.5: TP=19, FP=4, FN=1, micro accuracy=0.792, precision=0.826, recall=0.950, Jaccard=0.792, Dice=0.884. Mean IoU=0.801 chỉ tính 19 cặp đúng class; không đo chất lượng các ca bỏ sót/thừa hay polygon/track. FP/FN diễn tả bất đồng với R, chưa xác định nhãn thật sai.

Pedestrian yếu nhất theo precision 0.500 và recall 0.750, Dice 0.600 (3 TP/3 FP/1 FN). Confusion CSV không có cặp ghép sai class; extra gồm 3 Pedestrian và 1 Bike. Nhãn Bus/Truck không có ở L/R nên không suy chất lượng của chúng từ bảng này. Car/ThreeWheeler đạt 1.000 theo phép so với R vẫn có thể chứa ca rule/class chưa rõ như L11.

L có 24 box, nhưng L10/006840 nằm trong ignore R nên chỉ 23 box được tính: 19 TP+4 FP. R có 20 box: 19 TP+1 FN. 006840 L5/R7 bị tách extra/missing trong quality; compare gọi BOX_GEOMETRY vì có bước ghép khác. Không đếm thành hai người cần gán thêm. IoU sweep 0.3/0.5/0.7 cho matched L=20/19/14, M=11/10/6; đánh giá nhạy với ngưỡng.

Conflict mismatching_attributes của L11/006840 và L8/056040 liên quan occluded khác R. Theo R11, occluded là phán đoán để soát chéo, không dùng làm lỗi đúng/sai reference; giữ giá trị có căn cứ che khuất trong ảnh, không đặt false chỉ để xóa dòng conflict. Các số tự sinh được giữ nguyên.

Quyết định: D4 yêu cầu A sửa geometry xe lớn theo ảnh và export v2; D2/D3/D5/D6/D7/D8 cần bằng chứng/phê duyệt bổ sung, giữ hiện trạng và lưu escalation. E0 ở L6/056040 là giả thuyết reference thiếu người áo hồng; E4 ở model là giả thuyết lệch taxonomy/rider, không khẳng định nguyên nhân huấn luyện. E5 ghi rõ phép kiểm tiếp theo. Xem 40_decision_log.csv và 30_escalation_ticket.md.

P4 hoàn tất phân tích hiện có; P5/v2 và kiểm tích hợp cuối P6 chưa hoàn tất. Không tạo export giả hoặc số delta trước/sau khi chưa có bản A sửa.
