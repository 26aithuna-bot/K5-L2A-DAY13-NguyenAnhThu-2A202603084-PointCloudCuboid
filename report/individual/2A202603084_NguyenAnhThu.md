# Báo cáo Day 13 — Nguyễn Anh Thư, 2A202603084

## 1. Kết luận trước
MSSV: `2A202603084`; task: `D13-2A202603084`; hệ điều hành: Windows; CVAT server version: `2.74.1` (local `/api/server/about`). Tên trình duyệt không được ghi lại. MSSV bạn ngồi cạnh: `2A202603062`.

Task hiện có 56 cuboid (53 Car, 2 Pedestrian, 1 Truck): s01f40=8, s01f58=7, s02f00=5, s02f29=5, s03f40=3, s03f56=5, s04f43=6, s04f59=7, s05f07=5, s05f43=5. Ít nhất là s03f40 với 3 cuboid, thấp hơn mức 5 yêu cầu cho mỗi frame sau frame đầu. Phiếu tự kiểm gắn cờ 50/56 cuboid; đây là cảnh báo cần xem lại, không phải kết luận hộp sai. Bảng A có một cuboid ở 50.10 m, ngoài giới hạn 50 m; cần kiểm tra lại.

Từ số liệu hiện có, các hộp có ground gap gần 0 và không có cờ là nhóm có bằng chứng hình học tốt hơn; phần cần xem lại nhiều nhất là kích thước/đáy do có nhiều cảnh báo. Trong CVAT, các cuboid được lưu dạng Shape; thao tác chuẩn là tạo Shape, đặt hộp, chỉnh mặt bằng ở Top và chiều cao ở Side/Front, rồi Save. Tên trình duyệt không được lưu trong hiện vật.

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ. Ảnh: `t1-<frame>-3d`, `t1-<frame>-top` (ảnh nhìn từ trên xuống, có đánh dấu phía trước, +x, +y).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f40 | Phía trước theo −y; +x sang trái xe, +y ra sau; cạnh màn hình không ghi lại | Mốc tham chiếu khoảng z = −1.7 m; độ phẳng/dốc không có trong CSV | Xa nhất đã gán: 50.10 m (vượt giới hạn) | Bảng A: Car | Chưa ghi nhận vật ngoài cuboid | Chưa ghi nhận |
| s01f58 | Phía trước theo −y; +x sang trái xe, +y ra sau; cạnh màn hình không ghi lại | Mốc tham chiếu khoảng z = −1.7 m; độ phẳng/dốc không có trong CSV | Xa nhất đã gán: 29.20 m | Bảng A: Car | Chưa ghi nhận vật ngoài cuboid | Chưa ghi nhận |

Camera cho biết màu sắc, kết cấu/bề mặt và ngữ cảnh vật; LiDAR cho biết khoảng cách theo mét, hình học 3D và vị trí tương đối. Đây là khác biệt cảm biến nói chung; quan sát cụ thể của từng ảnh không được ghi lại.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| s02f00 | Phía trước là −y; cạnh ảnh không ghi lại | Chưa ghi nhận |
| s02f29 | Phía trước là −y; cạnh ảnh không ghi lại | Chưa ghi nhận |
| s03f40 | Phía trước là −y; cạnh ảnh không ghi lại | Chưa ghi nhận |
| s03f56 | Phía trước là −y; cạnh ảnh không ghi lại | Chưa ghi nhận |
| s04f43 | Phía trước là −y; cạnh ảnh không ghi lại | Chưa ghi nhận |
| s04f59 | Phía trước là −y; cạnh ảnh không ghi lại | Chưa ghi nhận |
| s05f07 | Phía trước là −y; cạnh ảnh không ghi lại | Chưa ghi nhận |
| s05f43 | Phía trước là −y; cạnh ảnh không ghi lại | Chưa ghi nhận |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | s01f58, idx 3 | Car | 272.3° (gần 270°; ảnh cần xác nhận đầu xe) | z_bottom = −1.827 m | Có ảnh t2; kiểm Top/Side/Front không xác minh được từ CSV | 5.278 × 2.315 × 1.883 m; rộng vượt khoảng SUV trong bảng 4.3 |
| (b) xe buýt hoặc xe tải | s05f43, idx 1 | Truck | 272.4° (gần 270°; ảnh cần xác nhận đầu xe) | z_bottom = −1.678 m | Có ảnh t2; kiểm Top/Side/Front không xác minh được từ CSV | 5.319 × 2.571 × 2.133 m; loại xe cần xác nhận trực quan |
| (c) người đi bộ hoặc xe hai bánh | s03f56, idx 5 | Pedestrian | 279.1° (hướng đi cần xác nhận trực quan) | z_bottom = −1.592 m | Có ảnh t2; kiểm Top/Side/Front không xác minh được từ CSV | 1.000 × 1.000 × 1.389 m; dài/rộng hơi lớn, cao thấp hơn khoảng người lớn |

Ảnh cần có: `t2-a-car.png`, `t2-b-bus-truck.png`, `t2-c-ped-cyclist.png`.

Không có file kết quả vòng hiệu chuẩn trong `work/hieuchuan/`, nên không thể xác định bạn lệch so với ai hoặc lớp chốt điểm nào. Bảng A không lưu vật đã bỏ qua; danh sách vật bỏ qua chưa được ghi lại.

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | `t4-1-bong-che.png`; frame s02f00 theo tên ghi trong báo cáo; task cá nhân | Vật thể gần có thể chắn tia LiDAR (giả thuyết; ảnh chưa được đối chiếu trực quan ở đây) | Chi tiết vùng khuất không được ghi lại; hướng dẫn có ví dụ tham khảo ở s02f00 nhưng không xác nhận đó là đúng cảnh trong ảnh học viên | Nếu vật bị che một phần, bật occluded và chỉ suy luận phần có đủ cơ sở |
| 2. Thưa ở xa | s04f43, idx 5; `t4-2-thua-o-xa.png`; task cá nhân | Khoảng cách xa có thể làm điểm LiDAR thưa hơn | Cuboid cách 47.36 m, phiếu tự kiểm đếm 54 điểm | Chỉ gán nếu thấy khoảng 15–20% thể tích hoặc có cơ sở rõ; nếu không, bỏ qua và ghi LOW_VISIBILITY |
| 3. Phản xạ thấp / ít điểm bất thường | `t4-3-phan-xa-thap.png`; frame/nguồn chưa được ghi lại | Bề mặt tối hoặc kính có thể phản xạ yếu hơn (giả thuyết tham khảo từ guideline; chưa xác nhận với ảnh) | Chưa có mô tả quan sát hoặc số intensity ghi lại | Ghi nguyên nhân như giả thuyết; chỉ gán nếu đủ hình dạng/điểm, nếu không ghi LOW_VISIBILITY |
| 4. Điểm lạ | `t4-4-diem-la.png`; frame và nguồn chưa được ghi lại | Nguyên nhân chưa rõ; không kết luận | Chưa ghi lại vị trí/cụm điểm; không thể mặc định đây là frame s02f35 vì frame đó không thuộc job cá nhân | Không gán cụm điểm lạ thành vật |

Tên bốn ảnh T4 có trong thư mục ảnh. Nội dung ảnh và các hiện tượng cần được học viên tự đối chiếu trực tiếp trên CVAT; không suy ra quan sát từ tên ảnh hoặc frame gợi ý trong tài liệu.

## 5. Số từ cuboid của tôi (Bước 5)
Lấy từ `submission/D13-<MÃ SỐ>-bangA.csv`; số điểm lấy từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| s01f40, idx 1, Car | −1.059 | 1.647 | −1.882 | 14.53 | 14.56 | 268.2 | đáy thấp hơn −1.7 m khoảng 0.18 m |
| s03f56, idx 5, Pedestrian | −0.897 | 1.389 | −1.592 | 17.24 | 17.27 | 279.1 | gần mặt đường; selfcheck ground_gap = +0.16 m |
| s05f43, idx 1, Truck | −0.611 | 2.133 | −1.678 | 11.28 | 11.30 | 272.4 | gần mặt đường; selfcheck ground_gap = +0.04 m |

Gần nhất: s01f58, idx 3, Car — `dist_m = 3.13 m`, `r = 3.25 m`. Xa nhất trong bảng: s01f40, idx 7, Car — `dist_m = 50.10 m`, `r = 50.10 m`, vượt giới hạn 50 m; xa nhất không vượt giới hạn là s04f43, idx 5 — `47.36 m`, `r = 47.38 m`.

So kích thước: Car gần nhất có `l × w × h = 5.278 × 2.315 × 1.883 m`; chiều dài/cao nằm trong khoảng SUV, rộng vượt mốc 2.10 m. Pedestrian s03f56 idx 5 có `1.000 × 1.000 × 1.389 m`; dài/rộng hơi vượt và cao thấp hơn khoảng người lớn ở bảng 4.3. Cần xác nhận loại xe/người và kích thước trên ảnh CVAT.

Yaw: xe cùng chiều s01f40 idx 1 là 268.2° (gần 270°); xe ngược chiều s02f29 idx 2 là 92.1° (gần 90°).

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| s01f58, idx 3, Car | 3.13 m | 3175 |
| s01f40, idx 1, Car | 14.53 m | 841 |
| s01f40, idx 4, Car | 23.91 m | 238 |
| s04f43, idx 5, Car | 47.36 m | 54 |

Các mẫu này cho thấy điểm trong cuboid giảm khi khoảng cách tăng: từ 3175 điểm ở 3.13 m xuống 54 điểm ở 47.36 m. Ba mẫu đầu lần lượt gần các trung vị slide 0–10 m (1968), 10–20 m (718) và 20–30 m (214); mẫu cuối ở 40–50 m, ngoài các bin so sánh đã nêu.

Selfcheck gắn cờ 50/56 cuboid, chủ yếu do chiều rộng Car ngoài khoảng kiểm tra. Các điểm cần ưu tiên xem lại gồm s02f00 idx 5 (ground_gap −0.91 m), s02f29 idx 5 (12 điểm, ground_gap +3.65 m), s05f43 idx 5 (ground_gap +0.47 m), và cuboid s01f40 idx 7 ở 50.10 m. Cờ chỉ là gợi ý, không tự xác định cuboid đúng/sai.

CSV hiện tại không lưu lịch sử trước/sau nên không xác định được hai cuboid đã sửa. Các cảnh báo width/ground gap được liệt kê ở trên là danh sách cần xem lại; chưa có ghi chú cho biết cảnh báo nào đã được giữ sau khi kiểm tra trên CVAT.

MSSV bạn đổi kiểm: `2A202603062`. Hai số được đối chiếu và kết quả khớp/lệch không được cung cấp; không khẳng định đã đối chiếu.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
(a) Nhãn theo mét giúp model học vị trí và kích thước vật trong không gian thật, thay vì tọa độ pixel vốn thay đổi theo camera và khoảng cách. Vật xa thường có ít điểm hơn; quy tắc riêng giúp tránh vừa bỏ sót vật còn nhận diện được, vừa đoán nhãn khi dữ liệu không đủ.

(b) Công cụ: Copilot SDK trong VS Code. Câu hỏi chính: cách hoàn thiện cấu trúc nộp bài, chạy checker và tính các số liệu từ CSV CVAT sinh ra. Cách dùng: hỗ trợ điền các phép tính kiểm chứng được; các nhận xét hình ảnh và thao tác của người học được ghi rõ là chưa xác minh nếu không có dữ liệu.

(c) Điều còn cần xác nhận: 50 cảnh báo selfcheck, đặc biệt chiều rộng Car và các ground gap lớn; cuboid ở 50.10 m vượt ngưỡng bài lab; frame s03f40 còn ít hơn mức 5 cuboid yêu cầu. Các trường quan sát chưa ghi lại và nguồn frame của ảnh t4-3 cần xác nhận.
