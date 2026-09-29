# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | BOX_GEOMETRY | 1 |
| center | B1 | MISSING | 7 |
| center | B1 | SPURIOUS | 10 |
| center | B1 | WRONG_CLASS | 2 |
| center | C0 | BOX_GEOMETRY | 1 |
| center | C0 | SPURIOUS | 1 |
| edge | B1 | MISSING | 2 |
| edge | B1 | SPURIOUS | 5 |
| mid | B1 | BOX_GEOMETRY | 3 |
| mid | B1 | IGNORE_SCOPE | 1 |
| mid | B1 | MISSING | 3 |
| mid | B1 | SPURIOUS | 9 |
| mid | C0 | SPURIOUS | 1 |
| unknown | B1 | MISSING | 1 |

## Top defects
- SPURIOUS: 26 (ví dụ frame adasind_019560.jpg)
- MISSING: 13 (ví dụ frame adasind_056040.jpg)
- BOX_GEOMETRY: 5 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

SPURIOUS (26 dòng), MISSING (13) và BOX_GEOMETRY (5) là số dòng findings do công cụ tổng hợp, gồm nhiều vai/nguồn và C0. Chúng không phải số object sai độc lập: cùng xe ba bánh có thể tạo một LR_noM và một M_only vì model sai class. Dòng unknown là QA-L02 object_ref mô tả người ngoài xe, không có box L riêng để suy zone; không gán zone tùy tiện cho đủ bảng.

- **WHY có bằng chứng:** 006840 M13 gọi Car cho xe ba bánh L2/R1; M8/M10 gọi Truck/Car cho L9/R6. 036720 M1 tách rider khỏi M2/M6 và 056040 M2 tách rider khỏi M3. E4_model_domain ở đây là giả thuyết lệch taxonomy/đơn vị gán, cần kiểm mapping và mẫu bổ sung; chưa chứng minh do méo fisheye. 056040 L6 ban đầu nghi E0_reference_defect (người áo hồng bên người áo đỏ L7); khi A soát lại crop tăng tương phản chỉ thấy một đầu và dải sari hồng, nên kết luận E1 (A tách một người thành hai) và gộp vào L7 ở v2 (D12). L11/006840 còn E5 vì L/R và model bất đồng ThreeWheeler/Bus trên ảnh mờ.
- **Sửa và owner:** annotator nhận D4, sửa biên xe L9/056040 theo pixel nhìn thấy rồi export v2 riêng; QA kiểm lại. ai_team rà mapping ThreeWheeler và rider/duplicate model, không ép nhãn L theo M. guideline nhận D2/D3/D6/D8; QA nhận D5/D7. Các ca chưa đủ bằng chứng giữ tạm và escalation, không xóa để tăng điểm so reference.
- **Bằng chứng:** `screenshots/trung-diag-006840.png`, `screenshots/trung-diag-056040.png`, `screenshots/long-qa-056040.png`; findings r3_diag và QA-L01–03; R01/R02/R03/R04/R06/R09. Xem `r3_diag/diagnosis.md`, `40_decision_log.csv`. `local_quality` 19 TP/4 FP/1 FN là độ khớp với teaching reference, không chốt đúng/sai toàn bộ các ca trên.

Kết quả v2 (`rework/delta.md`, khóa 8D7F-BEDD): L9 BOX_GEOMETRY và L6 SPURIOUS đã sửa; L11 (đổi sang Bus, D11) và L7/006840 (thu box, D13) vẫn báo chưa sửa vì lệch reference. Số ghép với teaching reference giảm: center matched 10→9, edge 3→2 — giữ nguyên số, không sửa ngược để khớp R; hai ca này còn bất đồng cần guideline/QA. Chỉ dùng đồng thuận tham chiếu như tín hiệu tìm ca; ngưỡng IoU và ignore quyết định mẫu số. Khi findings đổi sau rework, cần làm mới bảng bằng công cụ và giữ phân tích phù hợp số mới.
