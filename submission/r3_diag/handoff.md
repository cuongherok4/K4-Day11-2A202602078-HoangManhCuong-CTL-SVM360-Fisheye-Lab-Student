# Bàn giao vai C — chưa push

P4 và tài liệu P6 đã hoàn thiện với bằng chứng hiện có. Vai C do Codex thực hiện theo yêu cầu sau phần QA Long; không ghi là một lượt review con người độc lập mới. Các ticket nằm trong repo, chưa gửi ra ngoài và chưa được guideline owner phê duyệt.

## Cường / vai A làm tiếp

1. Đọc D4 trong `submission/40_decision_log.csv`: soi xe ba bánh L9 ở 056040; cạnh trái x=597 và đáy y=1200 chưa bao hết phần xe nhìn thấy. Crop `submission/screenshots/long-qa-056040.png` có ảnh gốc và nhãn. Xác định biên trên ảnh CVAT, không sao chép R5 hoặc kéo theo người/bóng/blur.
2. Sửa geometry có căn cứ theo R02/R05, giữ truncated=true ở mép phải; chưa tự thêm Pedestrian người áo trắng hay đổi class L11 vì D2/D3 còn mở. Các ca D5–D8 giữ theo quyết định tạm thời tới khi có bằng chứng.
3. Save và export CVAT for images 1.1 bản mới. Dùng file export thật vừa tải:

```bash
python3 lab11.py lock rework /duong-dan/export-v2.zip
python3 lab11.py rework
```

Không sửa đè XML/lock r1. Nếu biên đề xuất không xác nhận được trên ảnh, ghi phản biện cụ thể và chuyển lại QA thay vì sửa hình học để đủ file.

## Long / vai B

Kiểm lại v2 và hash: đúng 3 frame, shape thay đổi đúng D4, không lẫn rule R03a chưa phê duyệt, không thêm/sửa ngoài quyết định. So crop trước/sau, ghi xác nhận hoặc lỗi còn lại. Không dựa riêng dòng “đã sửa” của delta tự động vì ca L9 vốn có thể qua ngưỡng IoU dù box chưa bám vật.

## Trung / vai C sau khi nhận v2

Đọc delta thật trước/sau và kết quả QA; hoàn thiện trạng thái D4/P5 trong TEAMMATES, chạy `python3 lab11.py triage` rồi `python3 lab11.py check`, xem manifest tại thời điểm đó. Hiện check còn thiếu `rework/annotations-v2.xml`, `rework/lock2.txt`, `rework/delta.md`; chưa thể xác nhận đầy đủ bài.

Theo yêu cầu mới nhất: chưa push. Chờ chỉ dẫn mới trước khi đẩy thay đổi phần C. Không gửi link nộp/nhắn thành viên bằng công cụ khi chưa được yêu cầu.
