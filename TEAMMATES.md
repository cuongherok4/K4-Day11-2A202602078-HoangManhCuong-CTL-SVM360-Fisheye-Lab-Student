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
| C · Chẩn đoán & điều phối | Trịnh Nam Trung | 2A202602113 | trung | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | [Phân tích P4](submission/r3_diag/diagnosis.md), [bàn giao](submission/r3_diag/handoff.md), findings r3_diag, D2–D10, error card và kế hoạch P6; Codex thực hiện theo yêu cầu. Chưa push; còn chờ v2 và kiểm cuối. |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | `00_setup/mode.json` (slice B1-center), `sensor_context.md`, parking | A, B: slice B1-center trong mode.json; parking 1 ảnh core; sensor_context đủ nội dung | Parking + sensor_context đã có; nháp sampling 200 frame + gold set 4 camera ở commit `e71cfdd`, C sẽ kiểm và hoàn thiện ở P6 |
| P2 · Khóa bản đầu | A → B, C | `r1_craft/annotations.xml`, `lock.txt`, slice B1-center, code `1277-D599` (relock từ `5375-843D`, lý do ở decision log D1) | C: SHA-256 `annotations.xml` khớp `1277-D599`; selfqc 9/9; 5 dòng `r1_craft`; relock trước QA | Đã bàn giao cho B để QA mù |
| P3 · Chốt QA mù | B → C, A | [review](submission/r2_qa/qa_review.md), QA-L01–03 trong findings, screenshots/long-qa-*.png; mã 1277-D599 | B kiểm SHA, 24 box/3 frame, ignore và rule; chưa xem reference/model B1-center, đã đọc self-QC của A (giới hạn độc lập ghi trong review) | P3 hoàn tất; A xem hình học L9/056040, C phân xử người ngoài xe và class L11/006840; chưa kiểm v2 |
| P4 · Quyết định sửa | C → A, B | [decision log](submission/40_decision_log.csv), [handoff](submission/r3_diag/handoff.md), r3_diag | So khớp SHA r1, chất lượng 19 TP/4 FP/1 FN; triage hợp lệ; D4 là việc A sửa, các ca mở có owner/phép kiểm tiếp | P4 đã có phân tích; escalation chưa được owner bên ngoài phản hồi |
| P5 · Kiểm bản sửa | A → B → C | `rework/annotations-v2.xml`, `lock2.txt` (8D7F-BEDD), `delta.md`; decision log D11–D15 | [B điền: đã kiểm lại L9, L11, L6/L7 056040, L7 006840] | v2 sửa 4 ca (L9 geometry, L11 → Bus, gộp L6 vào L7, thu L7 006840). delta: center matched 10→9, edge 3→2 do bất đồng class/IoU với reference, giữ nguyên số; chờ B xác nhận |
| P6 · Chốt nộp | A, B → C | manifest.json; guideline patch, escalation, sampling/review/gold plan, error card, exit ticket | Đủ 3 file rework (v2, lock2 8D7F-BEDD, delta); error card/escalation/review plan/exit ticket đã cập nhật theo v2; check ✓ hình thức đầy đủ | Chờ B kiểm lại ca sửa, B/C tick xác nhận, C push commit chốt và gửi link |

## 4. Bất đồng và phối hợp

- Một ca đã phân xử ở bước quyết định: D4 — 056040/L9, A box tới y=1200, QA đề nghị soi phần xe ngoài box. C yêu cầu sửa theo biên xe thật và tách ca người ngoài xe khỏi geometry; chưa xác nhận A đã sửa. Xem decision log và long-qa-056040.png.
- Ca còn mở: D2/D3/D5/D6/D7/D8, có owner và phép kiểm tiếp trong decision log/escalation; D4 chờ rework của A. Không có phê duyệt guideline bên ngoài được ghi nhận.
- Đóng góp của A/B/C vào kế hoạch và exit ticket: nháp `45_sampling_plan.csv` và `46_gold_set_plan.md` do A soạn trong lúc chờ QA, dựa trên quan sát khi gán nhãn; C (Trung) kiểm, chỉnh lý do và bổ sung sau P4. C giữ ngân sách 55/45/50/50, bỏ suy luận zone→khoảng cách/rủi ro và ADASIND→camera SVM, bổ sung chống trùng clip, review độc lập, metadata và điều kiện gold. Nội dung do Codex hỗ trợ; chưa chọn thật 200 ảnh.
- Thay đổi phân công nếu có: nhóm ban đầu làm theo lộ trình solo đến P2, sau đó chuyển sang nhóm 3 người (A/B/C) trước khi B QA.

## 5. Xác nhận trước khi nộp

- [x] A xác nhận nhãn và export đúng phiên bản: Hoàng Mạnh Cường — `submission/r1_craft/` (lock 1277-D599), `submission/rework/` (lock 8D7F-BEDD), đều export từ CVAT task 75; lý do sửa ở decision log D11–D15
- [ ] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Hoàng Văn Long (`HvLonggg`) — đã QA độc lập trước reference trong [qa_review.md](submission/r2_qa/qa_review.md), 3 finding QA-L01–03 trong `submission/findings.csv`; chưa có bản `rework/` v2 để kiểm lại.
- [ ] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: [Tên / bằng chứng]
- [ ] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
