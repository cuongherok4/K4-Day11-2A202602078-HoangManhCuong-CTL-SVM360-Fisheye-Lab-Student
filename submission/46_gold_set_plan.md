# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ.

Phân bổ: front 55 (25 normal / 30 hard) · rear 45 (20/25) · left 50 (20/30) · right 50 (20/30) = 200.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Xe ba bánh/minibus ở xa, xe máy chen giữa hai xe, cụm người qua đường, ngược sáng lúc sáng sớm/chiều | Vật nhỏ và mờ nên nhầm class (ThreeWheeler ↔ Bus/Car); người lái bị che làm tách Pedestrian + Bike sai R03 | Nhãn trên ảnh fisheye gốc của camera front; giữ intrinsic, extrinsic, mask vòng kính và timestamp theo frame | Hai người gán nhãn độc lập; người thứ ba soát các box bất đồng theo ảnh + rule; bất đồng chưa giải quyết thì đưa lên guideline owner, không biểu quyết |
| rear | Lùi xe có người đi bộ ở rìa vòng kính, xe máy vượt sát, đèn pha ban đêm | Vật rất gần bị méo và bị vòng kính cắt, dễ sai truncated và hình học; đèn pha làm mất biên | Ảnh gốc rear + mask ego (cản sau) riêng cho rear; calibration rear và trạng thái số lùi nếu có | Review độc lập như front; thêm một lượt soát riêng ca rìa vòng kính bằng overlay mask trước khi chốt |
| left | Vật ở seam trước-trái/sau-trái, xe máy song song sát thân xe, cánh tay/tay lái ego che | Cùng vật xuất hiện trên hai camera với hai box khác nhau; ego_body che làm box bị cắt | Ảnh gốc left + polygon ego_body riêng từng frame; calibration left và front/rear để chiếu chéo vùng seam | Review độc lập; ca seam cần reviewer có xem đồng thời hai camera cùng timestamp; ghi quyết định vào decision log |
| right | Xe ba bánh sát bên che nửa khung, người đu bám ngoài xe, xe đỗ lề dày đặc | Luật rider (R03) chưa nói người đu ngoài thân xe; box xe lớn bị mép khung cắt | Ảnh gốc right + mask; calibration right và front/rear cho vùng seam | Review độc lập; ca luật chưa rõ đi qua guideline patch trước, sau đó mới gán lại và chốt |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): khi thay camera/ống kính hoặc vị trí gắn (calibration đổi), khi guideline tăng phiên bản (ví dụ v1.0.0 → v1.1.0 cho luật rider/người đu bám), khi thêm class mới, hoặc khi dữ liệu mới có phân bố khác rõ rệt (đêm, mưa, tuyến đường mới). Mỗi lần refresh ghi phiên bản gold, phiên bản rule và danh sách frame bị gán lại.
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: xe máy đi sát góc trước-trái xuất hiện ở mép phải ảnh front và mép trái ảnh left cùng lúc. Chưa ghép hay xóa box nào cho tới khi có (1) timestamp đồng bộ giữa hai camera, (2) calibration để chiếu hai box về cùng hệ tọa độ (ví dụ BEV), (3) policy output đích: giữ cả hai box theo từng camera hay hợp nhất thành một object cross-camera. Trong gold, hai box hợp lệ ở hai camera không bị tính là DUPLICATE.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera: ADASIND chỉ có một camera fisheye; tỉ lệ đồng thuận hay TP/FP/FN trên vài frame không đại diện cho góc nhìn hông/sau, cho vùng seam hay cho ego_body khác nhau từng camera. Teaching reference cũng có thể sai, nên đồng thuận với nó chưa phải kiểm chứng. Cần review độc lập theo từng camera với mẫu hard riêng.
