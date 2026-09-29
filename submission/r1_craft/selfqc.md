# Tự soát


## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ
- [x] lens_border và ego_body
- [x] Class sáu nhãn
- [x] Rider và Bike
- [x] Geometry trên ảnh fisheye gốc
- [x] truncated và occluded
- [x] Vật thiếu hoặc box trùng
- [x] ignore_region có reason
- [x] Tên task raw_fisheye và export CVAT 1.1

## Fill ratio (K12)
- adasind_006840.jpg box 7 edge: 0.705
- adasind_036720.jpg box 2 edge: 0.672
- adasind_056040.jpg box 1 edge: 0.768
- adasind_056040.jpg box 9 center: 0.895
mean edge: 0.715 (n=3)
mean center: 0.895 (n=1)

## Ghi chú tự soát (bản nháp exports/r1-draft.xml, slice B1-center)
Soát trên overlay từng frame + bảng box/zone/ignore in từ bản nháp; `selfqc` không báo cảnh báo tự động.

1. **Phạm vi H=40:** 24 box, box thấp nhất là Car 006840 (232,828)-(262,872) cao 44px. Không box các xe ở xa cao < 40px: xe trắng và xe tối màu ở 036720 (x≈225–300, y≈1020–1057), xe máy nhỏ ở 006840. Không box nào ≥50% nằm trong ignore.
2. **lens_border / ego_body:** giữ nguyên 2 polygon lens_border mỗi frame, vì khớp vành kính khi đặt lên ảnh. ego_body: 006840 không vẽ (frame ngoại lệ R07). 036720 vẽ cánh tay áo xanh + tay lái xe ego ở góc dưới trái (x 0–135, y 1285–1600). 056040 vẽ cánh tay + tay lái ở góc dưới trái (x 0–215, y 1085–1590).
3. **Class:** xe ba bánh màu vàng/đen là ThreeWheeler (R04). Van bạc đỗ lề 036720 là Car. Xe trắng–vàng 006840 (301,818)-(360,886) gán ThreeWheeler: thấy kính chắn gió và thân thấp giống auto nhìn từ trước, chưa loại trừ khả năng là minibus. Đã ghi vào findings.
4. **Rider:** người lái + xe máy là một Bike (006840 ×2, 036720 ×1, 056040 ×3). Hành khách trong xe ba bánh 036720 không box. Người áo trắng đu bám bên hông xe ba bánh 056040 (x≈915–1080, y≈590–1300) không box riêng, xem như người đi trên xe (R03). Đây là ca luật chưa nói rõ, đã ghi finding.
5. **Geometry:** box bám phần nhìn thấy trên ảnh gốc; xe ba bánh lớn ở 036720 và 056040 bị cắt ở mép phải, box dừng ở mép ảnh, không kéo dài phần tưởng tượng. 4 polygon K12 bám viền, fill 0.67–0.90.
6. **Attribute:** truncated chỉ đặt cho vật chạm mép khung hoặc vòng kính (036720 xe ba bánh lớn; 056040 Bike mép trái và xe ba bánh lớn). occluded đặt cho vật bị vật khác che (người sau xe ba bánh, xe sau người lái, ô tô bạc sau xe ba bánh…). edge_zone lấy theo bán kính tương đối ≥0.6 của vòng kính.
7. **Thiếu/trùng:** so với prefill đã thêm: 006840 thêm 6 box (người đội mũ trắng mép trái, người lái xe máy đỏ, xe ba bánh xa, van, xe trắng–vàng, người đứng sau xe ba bánh); 036720 và 056040 prefill không có box, tự vẽ toàn bộ. Sửa geometry 5 box prefill ở 006840 (box người thấp 73–91/838–879 kéo thành 75–100/836–902, box Bike thêm phần đầu người lái). Không có box trùng.
8. **ignore_region:** chỉ có lens_border (import) và ego_body (tự vẽ), mỗi polygon một reason. Không dùng crowd_or_group hay unreadable.
9. **Task/định dạng:** task `Day11 · ADASIND · B1-center · raw_fisheye`, export CVAT for images 1.1, không kèm ảnh.
