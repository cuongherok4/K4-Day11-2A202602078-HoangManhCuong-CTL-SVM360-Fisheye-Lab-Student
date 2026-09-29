# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: K4 · VinUni AI20k
- Tên nhóm: CTL
- Repo Public: https://github.com/cuongherok4/K4-Day11-2A202602078-HoangManhCuong-SVM360-Fisheye-Lab-Student
- Máy giữ hồ sơ chính / người quản lý: máy của Hoàng Mạnh Cường (vai A) — giữ CVAT local (task 73 parking, 74 C0, 75 B1-center), `data/`, `exports/` và `submission/` chính thức
- Slice chung lấy từ mode.json: `B1-center` (adasind_006840, adasind_036720, adasind_056040)
- Tên định danh vai A dùng cho --self: `cuong` (mode chạy với một tên trước khi chốt nhóm; không chạy lại `mode` để giữ nguyên slice và các bản khóa)
- Kênh trao đổi nội bộ: Zalo nhóm + trao đổi trực tiếp tại lab
- Đại diện nộp (vai C): Trịnh Nam Trung, 2A202602113
- Commit chốt bài: [SHA hoặc URL commit]

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Hoàng Mạnh Cường | 2A202602078 | cuong | Parking/C0/slice, self-QC, lock, rework | `submission/parking/`, `submission/p1_calib/`, `submission/r1_craft/` (lock `1277-D599`), dòng `calib`/`r1_craft` trong `findings.csv` |
| B · QA độc lập | Hoàng Văn Long | 2A202602071 | long | Review trước reference, finding QA, kiểm lại ca sửa | [QA review](submission/r2_qa/qa_review.md), [overlay](submission/r2_qa/qa_overlay.html), 3 finding r2_qa QA-L01–03 và 3 PNG long-qa trong screenshots; Codex thực hiện theo phân công. Chưa có v2 để kiểm lại. |
| C · Chẩn đoán & điều phối | Trịnh Nam Trung | 2A202602113 | trung | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | [Link file/commit và mô tả phần đã làm] |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | `00_setup/mode.json` (slice B1-center), `sensor_context.md`, parking | A, B: slice B1-center trong mode.json; parking 1 ảnh core; sensor_context đủ nội dung | Parking + sensor_context đã có; nháp sampling 200 frame + gold set 4 camera ở commit `e71cfdd`, C sẽ kiểm và hoàn thiện ở P6 |
| P2 · Khóa bản đầu | A → B, C | `r1_craft/annotations.xml`, `lock.txt`, slice B1-center, code `1277-D599` (relock từ `5375-843D`, lý do ở decision log D1) | C: SHA-256 `annotations.xml` khớp `1277-D599`; selfqc 9/9; 5 dòng `r1_craft`; relock trước QA | Đã bàn giao cho B để QA mù |
| P3 · Chốt QA mù | B → C, A | [review](submission/r2_qa/qa_review.md), QA-L01–03 trong findings, screenshots/long-qa-*.png; mã 1277-D599 | B kiểm SHA, 24 box/3 frame, ignore và rule; chưa xem reference/model B1-center, đã đọc self-QC của A (giới hạn độc lập ghi trong review) | P3 hoàn tất; A xem hình học L9/056040, C phân xử người ngoài xe và class L11/006840; chưa kiểm v2 |
| P4 · Quyết định sửa | C → A, B | [finding, decision log, commit] | [Điền] | [Điền] |
| P5 · Kiểm bản sửa | A → B → C | [v2, lock2, review kiểm lại, delta] | [Điền] | [Điền] |
| P6 · Chốt nộp | A, B → C | [manifest, commit chốt] | [Điền] | [Điền] |

## 4. Bất đồng và phối hợp

- Một ca đã phân xử: [Frame/object/rule; ý kiến A/B; bằng chứng; quyết định và link]
- Ca còn mở: [Nội dung, người theo dõi, phép kiểm tiếp theo; nếu không còn thì ghi rõ]
- Đóng góp của A/B/C vào kế hoạch và exit ticket: nháp `45_sampling_plan.csv` và `46_gold_set_plan.md` do A soạn (có AI hỗ trợ) trong lúc chờ QA, dựa trên quan sát khi gán nhãn; C (Trung) kiểm, chỉnh lý do và bổ sung sau P4. [C điền phần đã sửa]
- Thay đổi phân công nếu có: nhóm ban đầu làm theo lộ trình solo đến P2, sau đó chuyển sang nhóm 3 người (A/B/C) trước khi B QA.

## 5. Xác nhận trước khi nộp

- [ ] A xác nhận nhãn và export đúng phiên bản: [Tên / bằng chứng]
- [ ] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: [Tên / bằng chứng]
- [ ] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: [Tên / bằng chứng]
- [ ] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
