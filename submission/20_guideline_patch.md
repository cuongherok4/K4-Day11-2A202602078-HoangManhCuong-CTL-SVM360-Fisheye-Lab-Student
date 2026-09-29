# Guideline patch

- **Rule mới đề xuất:** R03a — Người được phương tiện chở ở ngoài khoang (đứng trên bậc hoặc bám thành) phải được đánh dấu ca cần phân xử khi không xác định được có đang được chở hay đứng trên mặt đường. Trong taxonomy sáu class hiện tại, đề xuất không tạo Pedestrian cho người xác định đang được xe chở; box ThreeWheeler bao phần xe nhìn thấy, không tự mở rộng theo người bám ngoài. Người đứng/đi trên mặt đường được box Pedestrian riêng nếu phần nhìn thấy cao ≥40 px. Không suy tư thế từ một chi tiết mờ.
- **Áp dụng cho:** ThreeWheeler, Pedestrian; các zone như nhau, không đổi ignore. Giữ luật gộp rider + Bike hiện hành. Đây là quy ước cho tập con sáu class của lab.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R03 chỉ nói người ngồi trong xe và rider xe hai bánh, chưa nói người bám ngoài xe ba bánh. 056040 có người áo trắng x≈915–1080, y≈590–1300; 006840/L1 cũng cần xác định quan hệ người–xe. Hai cách hiểu có thể đổi số object và box xe.
- **`rules_version` mới:** đề xuất v1.1.0, chưa được Lab Coach/guideline owner phê duyệt. Nhãn và báo cáo hiện tại vẫn theo v1.0.0.
- **Hiệu lực từ:** vòng gán nhãn sau phê duyệt và rà lại toàn bộ ca tương tự; không áp dụng hồi tố lên r1 hoặc thay reference để cải thiện điểm.

Ví dụ dương: người đi bộ áo đỏ bên xe ở 056040 vẫn là Pedestrian. Ví dụ cần phân xử: người áo trắng bám ngoài xe lớn ở cùng frame. Phải xem dấu tiếp xúc chân/bậc xe; nếu ảnh tĩnh không giải quyết được thì ghi unresolved, xin frame lân cận nếu được phép và đánh dấu khỏi tập gold đã phê duyệt.

Quyết định tạm thời D2: giữ hiện trạng nhãn của ca áo trắng, không thêm Pedestrian từ suy đoán. Chỉ gửi đề xuất; chưa có phản hồi ngoài nhóm, chưa tuyên bố rule mới có hiệu lực.
