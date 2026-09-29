# Sensor context

- Rig (theo quan sát, ADASIND không kèm tài liệu rig): một camera fisheye đặt dọc (ảnh 1080×1920, portrait), gắn trên xe nhỏ đi trong làn đường đô thị/ven đô, nhìn về phía trước, hơi lệch về bên trái xe. Đường chân trời nằm khoảng giữa ảnh; mặt đường chiếm nửa dưới. Không có thông số intrinsic/extrinsic, độ cao gắn hay timestamp, nên không suy ra khoảng cách thật hay ghép với camera khác.
- `ego_body`: ở slice B1-center (006840, 036720, 056040) chưa thấy rõ thân xe ego. Góc dưới trái của 006840 có bóng đổ và mảng tối sát vành kính, nhưng không xác định được đó là thân xe, nên theo luật không vẽ `ego_body` cho 006840. Hai frame còn lại soát lại trên CVAT: chỉ vẽ khi nhìn thấy bộ phận thân xe (capo, gương, tay lái).
- Vòng kính: hình tròn/elip gần như chạm hai cạnh trái–phải ảnh (x ≈ 0–1080), theo chiều dọc khoảng y ≈ 140–1760. Vùng ảnh hợp lệ chiếm khoảng 75–80% khung; bốn góc và dải trên/dưới là vành đen (`lens_border`). Méo mạnh nhất ở rìa vòng kính: cột điện, dây điện và cạnh nhà bị cong.
- Giới hạn: đây là dữ liệu **một camera**, không đại diện đủ bốn camera front/rear/left/right của hệ SVM; phần bốn camera trong bài là kế hoạch giả lập.
