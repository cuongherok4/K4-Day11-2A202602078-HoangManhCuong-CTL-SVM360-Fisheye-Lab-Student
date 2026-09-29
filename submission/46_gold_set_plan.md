# Đề xuất gold set theo camera — tình huống giả lập

Đầu bài: 50.000 frame SVM bốn camera, chọn 200 frame review; chưa có tập đó trong repo. Front 55 (25 normal/30 hard), rear 45 (20/25), left 50 (20/30), right 50 (20/30). Đây là kế hoạch tạo gold sau phân xử, không gọi teaching reference hay 200 ảnh chưa chọn là gold.

| camera_id | Normal cần phủ | Hard cần phủ | Annotation space / metadata | Review riêng |
|---|---|---|---|---|
| front | Xe/người rõ, ánh sáng đủ, ít che | Vật nhỏ/che, ngược sáng, ThreeWheeler/Bus/Car, rider | Ảnh fisheye gốc front; intrinsic/extrinsic, kích thước ảnh, lens mask, timestamp | Hai lượt độc lập; người thứ ba xem class và phần rider, không quyết định theo đa số/model. |
| rear | Cảnh phía sau rõ khi xe đi/lùi theo metadata | Người bị che, đèn pha, vật cắt khung | Ảnh rear, calibration rear, ego mask theo thân xe thật; trạng thái lùi nếu có | Soát riêng mask cản/thân xe, phần nhìn thấy và truncated/occluded. |
| left | Xe/người phía hông trong cảnh dễ đọc | Seam trước-trái/sau-trái, che thân xe, méo/cắt | Ảnh left, calibration riêng và camera láng giềng, timestamp đồng bộ | Xem cặp seam đồng thời; kiểm identity bằng metadata và policy. Không áp mask cánh tay xe máy ADASIND cho ô tô. |
| right | Xe/người rõ phía hông phải | Seam trước-phải/sau-phải, xe lớn che, người ngoài khoang | Ảnh right, mask/calibration riêng, timestamp và policy target | Review geometry xe lớn và ca R03a; chưa thống nhất rule thì chưa đưa ca đó vào gold đã duyệt. |

Chọn ứng viên theo 45_review_plan.md; normal/hard mỗi camera đều có người review, không chỉ sửa nhóm hard. Người A và B gán độc lập khi chưa xem reference/model; C đối chiếu bất đồng và bằng chứng. Phân công này là quy trình đề xuất cho tập tương lai, không khẳng định ba người đã thực hiện trên 200 ảnh. Guideline owner quyết định ca luật thiếu; ca ảnh không đọc được được gắn trạng thái unresolved, tách khỏi phần gold đã duyệt, giữ lịch sử và xin mẫu thay thế cùng tầng nếu cần đủ 200 ca được duyệt.

Hồ sơ mỗi frame lưu nguồn/giấy phép, camera_id, clip/timestamp, phiên bản calibration, annotation space, rule version, hai bản gán, quyết định phân xử và reviewer. Chỉ gọi gold khi bất đồng phạm vi/class/geometry còn lại đã được giải quyết có lý do, mọi frame được xác nhận độc lập và toàn bộ phiên bản đóng băng có hash. Một con số agreement hoặc check exit 0 không thay bước này.

Ca seam: xe máy xuất hiện đồng thời ở front và left. Giữ hai box hợp lệ theo camera; không tính DUPLICATE chỉ vì chung một vật. Trước nối track cần đồng bộ timestamp và sai số cho phép đã định, calibration chính xác theo cấu hình camera, bằng chứng identity qua chuỗi ảnh và policy output (box riêng theo camera hay object hợp nhất). Biến đổi sang BEV cần mô hình hình học/giả định mặt đất phù hợp; không chiếu cả box 2D thành footprint chính xác mà không kiểm độ cao/vị trí vật. Chưa có những dữ kiện đó thì không gán chung track ID.

Refresh khi đổi camera/ống kính/vị trí gắn/calibration, thay taxonomy hoặc rule (như R03a được duyệt), hay dữ liệu mới khác điều kiện sáng/thời tiết/tuyến. Lưu phiên bản cũ, xác định tập ảnh bị ảnh hưởng, gán/review lại rồi phát hành version mới; không lặng lẽ ghi đè.

Ba ảnh ADASIND thuộc một camera, không có bằng chứng đồng bộ bốn camera. Peer agreement, 19 TP hoặc mean IoU 0.801 với teaching reference không xác nhận gold cho hệ SVM; mỗi camera cần mẫu normal/hard và mask/calibration riêng.
