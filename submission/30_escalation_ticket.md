# Escalation ticket

Người soạn: vai C (Trung), Codex hỗ trợ theo yêu cầu. Các ticket được ghi trong repo; chưa gửi cho Lab Coach hay bên ngoài.

## Ticket 1 — Người bám ngoài xe (D2)
- **Frame:** adasind_056040.jpg, L9/person_outside; liên quan 006840/L1.
- **Ảnh chụp:** `submission/screenshots/long-qa-056040.png` (crop ảnh gốc/overlay, không phải screenshot CVAT).
- **Expected impact:** thay đổi số Pedestrian và phạm vi box ThreeWheeler; có thể làm QA giữa hai người không nhất quán.
- **Owner:** guideline — người có quyền phê duyệt R03a/Lab Coach.
- **Recommendation:** xem patch v1.1.0 đề xuất, xác định trường hợp người được xe chở ngoài khoang; giữ nhãn hiện tại đến khi có phê duyệt. Không coi sự vắng mặt ở R là đáp án.
- **Cần trả lời:** người áo trắng đang được xe chở hay đứng trên mặt đường; có cần box riêng, và box xe có bao người không? Nếu ảnh không đủ, xin ảnh lân cận được phép dùng. Trạng thái: escalated, chưa có phản hồi.

## Ticket 2 — Class xe trắng-vàng (D3)
- **Frame:** adasind_006840.jpg L11/R4/M12, vùng (301,818)–(360,886).
- **Ảnh chụp:** `submission/screenshots/trung-diag-006840.png`, `long-qa-006840.png`.
- **Expected impact:** L/R gọi ThreeWheeler, M gọi Bus; quyết định sai có thể che lỗi reference hoặc đổi nhãn đúng thành sai.
- **Owner:** guideline.
- **Recommendation:** giữ ThreeWheeler tạm thời; kiểm đặc điểm bánh/thân/kiểu xe với nguồn ảnh đủ rõ. Nếu không phân xử được, giữ unresolved ngoài tập gold; không bỏ vật ≥40 px chỉ vì class khó. Trạng thái: escalated.

## Ticket 3 — Ignore của reference khác bản người gán (D8)
- **Frame:** adasind_006840.jpg L10, Car (232,828)–(262,872), H=44.
- **Ảnh chụp:** `submission/screenshots/trung-diag-006840.png`; polygon R xem `r1_craft/compare.html` và reference đã mở.
- **Expected impact:** box bị loại khỏi TP/FP theo ignore R, nên 24 box L chỉ còn 23 box được chấm; thay đổi mask làm đổi mẫu số.
- **Owner:** guideline.
- **Recommendation:** reviewer xác nhận lý do ignore R có phù hợp R06 hay xe còn đọc được; giữ L10 và hai bản mask để audit. Không xóa nhãn/chép mask chỉ nhằm tăng chỉ số. Trạng thái: escalated.
