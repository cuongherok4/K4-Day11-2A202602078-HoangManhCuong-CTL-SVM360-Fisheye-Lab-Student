# QA review · B1-center

Vai B · Long; thực hiện bằng Codex theo phân công của Cường. Thời điểm chốt QA: 2026-09-29T12:11:10+07:00.

Mã khóa: **1277-D599**. SHA-256: `1277d5996d15ee76521746def2b4b7b9a89d64fae1befac365f4daca9573e81d`.
Nguồn: `submission/r1_craft/annotations.xml`; bản sao đối chiếu `data/_qa/B1-center.xml`.

## Phạm vi và tính độc lập

Đã xem cả 3 ảnh gốc, overlay của 24 box, polygon ignore và các crop chi tiết; đối chiếu rules v1.0.0 R01–R11. Không mở reference, worked overlay hoặc model của B1-center. Đã đọc self-QC và findings của A khi nhận bàn giao trước khi bắt đầu QA: đây là review độc lập với reference, nhưng không mù với nhận xét của A. Các ca còn nghi vấn được giữ trạng thái đề xuất, không coi là lỗi đã được reference xác nhận.

`L1`, `L2`, … đếm lại từ 1 theo thứ tự box trong mỗi frame của XML và khớp QA overlay. Hình PNG là crop/overlay tạo trực tiếp từ ảnh gốc và XML đã khóa, không phải ảnh chụp giao diện CVAT. Cột `what` ở các finding dưới đây là loại vấn đề QA đề xuất để triage; `why` để trống theo quy trình P3. `cell=L_only` là quy ước ghi QA trước reference, không khẳng định R/M bỏ sót.

## Nhận xét cần xử lý

| Mã | frame | object_ref | rule_id | Nhận xét và đề xuất |
|---|---|---|---|---|
| QA-L01 | adasind_056040.jpg | L9 | R02;R05 | Box ThreeWheeler hiện (597,528)–(1080,1200). Crop cho thấy bộ phận xe/bệ bên dưới ở mép phải còn kéo xuống dưới y=1200; mép trái dè bánh cũng nhô ra trái x=597. Đề nghị A soát lại biên phần xe nhìn thấy, đặc biệt vùng x≈570–1080, y≈1100–1350. Đây là vùng cần soi, không phải tọa độ box mới đã chốt. Không kéo box theo bóng đổ hay suy diễn vùng blur. Ưu tiên P1, rework hình học sau khi xác định đúng phần thân xe. Giữ truncated=true vì xe bị cắt ở biên phải. |
| QA-L02 | adasind_056040.jpg | L9/person_outside | R03 | Người áo trắng bám ngoài xe, không nằm trong khoang; R03 chỉ loại người ngồi trong phương tiện và quy định gộp rider cho xe hai bánh. Chưa đủ căn cứ áp dụng câu đó cho người bám ngoài ThreeWheeler. Escalate cho C/guideline quyết định có box Pedestrian riêng và có gộp phần người vào box xe hay không. Chưa yêu cầu A tự thêm box. Mức ưu tiên để mở đến khi phân xử. |
| QA-L03 | adasind_006840.jpg | L11 | R04 | Xe trắng/vàng (301,818)–(360,886) có đầu xe cao, kính rộng và thân bị L8 che. Crop chưa cho thấy rõ cấu trúc ba bánh; cũng chưa đủ để kết luận minibus/van. Giữ nhãn hiện tại trong bản khóa, chuyển C xác minh ThreeWheeler/Bus/Car từ bằng chứng hình ảnh hoặc xin phân xử. Không tự đổi class theo màu sơn. Mức ưu tiên để mở đến khi phân xử. |

Bằng chứng: [056040 — raw và box](../screenshots/long-qa-056040.png), [006840 — raw và box](../screenshots/long-qa-006840.png), [036720 — raw và box](../screenshots/long-qa-036720.png), [overlay toàn bộ box](qa_overlay.html).

## Kết quả soát toàn slice

- **006840 (11 box):** Bike L4/L8 có bao người lái; L6 là xe hai bánh đỗ, vẫn thuộc Bike. L5 bị che nên occluded=true có căn cứ. Không vẽ ego_body phù hợp ngoại lệ R07. L11 cần phân xử như trên.
- **036720 (4 box):** L2 bao người và xe hai bánh; L3 van thuộc Car theo R04. L4 chạm biên phải và truncated=true. Các xe rất xa cần xét chiều cao phần nhìn thấy, không thêm box chỉ vì nhìn thấy xe; chưa xác nhận ca thiếu ≥40 px ở vùng đường xa qua lần soát này.
- **056040 (9 box):** L1 chạm biên trái, truncated=true; L2/L8 bị che có occluded=true. L9 cần xem lại hình học; người ngoài xe cần quy tắc rõ trước khi sửa.
- **Ignore:** mỗi frame có 2 lens_border; ego_body có ở 036720 và 056040, không có ở 006840. Quan sát overlay ignore không thấy lệch lớn cần sửa ngay; đây không phải kết luận polygon chính xác từng pixel. Mỗi ignore có một reason hợp lệ. Kiểm hình học bằng helper của lab: không box nào nằm ≥50% trong một ignore polygon.
- **Hình học/phạm vi:** 24 box đều cao ≥40 px; không thấy box trùng rõ ràng. Không suy mức rủi ro từ vị trí center/mid/edge.

## Bàn giao A và C

1. C phân xử QA-L02 và QA-L03, ghi quyết định gắn rule/evidence; việc mở reference và phân loại WHY thuộc P4.
2. A soi và sửa QA-L01 trong CVAT nếu xác nhận, xuất v2 và khóa rework; không sửa đè XML r1 đã khóa. L02/L03 chỉ sửa theo quyết định đã ghi.
3. B kiểm lại đúng ca sửa trên v2 và mã khóa mới, đối chiếu R02/R03/R04/R05 và ảnh gốc, rồi ghi kết quả kiểm lại. Hiện **chưa có v2 để kiểm**, chưa xác nhận hoàn tất P5.

P3 đã ghi báo cáo và 3 finding QA; đây chưa phải xác nhận toàn bộ bài đủ điều kiện nộp.
