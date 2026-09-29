# Exit ticket

Nội dung tổng hợp vai C (Trung) với Codex hỗ trợ. Đây là phản ánh bằng chứng trong repo, không phải xác nhận trải nghiệm hay phê duyệt của từng thành viên.

1. **Seam hai camera:** hai box có thể đều hợp lệ vì là hai quan sát của cùng vật ở hai mặt phẳng ảnh. DUPLICATE là hai nhãn lặp không hợp lệ trong cùng phạm vi output đã định; không tự xóa một box giữa camera. Cần policy giữ quan sát theo camera hoặc hợp nhất object, timestamp đồng bộ, calibration và bằng chứng identity trước ghép. Frame ADASIND không cung cấp dữ kiện này.

2. **Track cùng camera:** giữ track ID khi bằng chứng chuỗi ảnh cho thấy cùng vật tiếp tục hiện diện; thêm keyframe khi vị trí/hình dạng/tư thế thay đổi khiến nội suy không bám vật hoặc khi thuộc tính cần cập nhật theo task. Outside đánh dấu vật ra khỏi trường nhìn/phạm vi track theo guideline, không dùng như đồng nghĩa occluded. Khi bị che tạm thời, ghi occlusion và giữ identity nếu đủ bằng chứng; nếu không đủ, xin phân xử theo policy tái xuất hiện. Nối qua camera cần timestamp và dung sai đồng bộ, calibration tương ứng, quan sát cặp seam/chuỗi chuyển tiếp cùng vật, chính sách track_id và output. Không nối chỉ vì cùng class/màu hoặc trùng vùng BEV.

3. **Nhìn lại một ca:** 006840/L5 ban đầu được A kéo box xuống phần chân và cho rằng đúng; R7 ngắn hơn. compare báo BOX_GEOMETRY còn local-quality tách một FP và một FN vì không qua IoU 0.5. C giữ unresolved D5, không ép box trùng R hoặc thêm người thứ hai. Nếu làm lại, lưu crop chân/người trước lock và đánh dấu phần bị che ngay khi vẽ, nhờ QA xác định chân thuộc ai trước khi xem reference. Bài học là kết quả ghép không tự quyết định ai đúng. P5 vẫn chờ bản rework từ A; chưa tuyên bố đã sửa hay kiểm lại.
