# SỔ TAY THAO TÁC THỰC HIỆN CHI TIẾT (INSTRUCTION.MD)

## Ngày 13 — Robotaxi A: LiDAR 3D Object Detection & Annotation QC

> **Tài liệu tham chiếu chuẩn trong repo:**
>
> - [README.md](file:///home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/README.md): Tổng quan buổi học, chuẩn bị hệ thống và phân bổ thời gian.
> - [HUONG-DAN.md](file:///home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/HUONG-DAN.md): Thao tác bài nguồn trên CVAT, nộp v1, QC chéo và nộp v2 trên portal.
> - [PRE-LABEL.md](file:///home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/PRE-LABEL.md): Hướng dẫn chi tiết thực hành PointPillars pretrained và kiểm tra lỗi pipeline z.
> - [LABEL_GUIDELINE.md](file:///home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/LABEL_GUIDELINE.md): Quy tắc gán nhãn 5 class và hiệu chỉnh 7 DoF cuboid qua 4 góc nhìn.
> - [RUBRIC.md](file:///home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/RUBRIC.md): Tiêu chí tự kiểm tra và nguyên tắc đánh giá formative.
> - [PRE-LABEL-REPORT.md](file:///home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/PRE-LABEL-REPORT.md): Mẫu báo cáo thí nghiệm nhóm PointPillars.
> - [TEAMMATES.md](file:///home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/TEAMMATES.md): Danh sách thành viên và phân công vai trò.
> - [bundle/README-STUDENT.md](file:///home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/bundle/README-STUDENT.md): Hướng dẫn chạy gói Student KITTI tự động không cần GPU.

---

# MỤC LỤC

1. [Bản đồ Kiến trúc &amp; Phân bổ Thời gian (240 phút)](#1-bản-đồ-kiến-trúc--phân-bổ-thời-gian-240-phút)
2. [PHẦN I: THỰC HÀNH NHÓM 4 NGƯỜI (60 – 70 PHÚT)](#phần-i-thực-hành-nhóm-4-người-60--70-phút)
   - [Bước 1.1: Chuẩn bị Thư mục, Phân vai &amp; Kích hoạt Phiên (10 phút)](#bước-11-chuẩn-bị-thư-mục-phân-vai--kích-hoạt-phiên-10-phút)
   - [Bước 1.2: Chạy Thực nghiệm 3 Lượt A / B / C (25 phút)](#bước-12-chạy-thực-nghiệm-3-lượt-a--b--c-25-phút)
   - [Bước 1.3: Chạy 3 Ca QC Pipeline &amp; Nhận diện Lỗi Hệ thống (15 phút)](#bước-13-chạy-3-ca-qc-pipeline--nhận-diện-lỗi-hệ-thống-15-phút)
   - [Bước 1.4: Viết Nhận xét Cá nhân, Hoàn tất Báo cáo &amp; Nghiệm thu (15 phút)](#bước-14-viết-nhận-xét-cá-nhân-hoàn-tất-báo-cáo--nghiệm-thu-15-phút)
3. [PHẦN II: THỰC HÀNH CÁ NHÂN TRÊN CVAT &amp; PORTAL (~170 PHÚT)](#phần-ii-thực-hành-cá-nhân-trên-cvat--portal-170-phút)
   - [Bước 2.1: Mở Job &amp; Nhận diện Giao diện CVAT 3D](#bước-21-mở-job--nhận-diện-giao-diện-cvat-3d)
   - [Bước 2.2: Kỹ thuật Hiệu chỉnh 7 DoF Bằng Tay trên CVAT (Cầm tay chỉ việc)](#bước-22-kỹ-thuật-hiệu-chỉnh-7-dof-bằng-tay-trên-cvat-cầm-tay-chỉ-việc)
   - [Bước 2.3: Lưu trong CVAT và Nộp Snapshot v1 trên Portal](#bước-23-lưu-trong-cvat-và-nộp-snapshot-v1-trên-portal)
   - [Bước 2.4: Nhận bài &amp; Soi lỗi QC Chéo (QC Reviewer)](#bước-24-nhận-bài--soi-lỗi-qc-chéo-qc-reviewer)
   - [Bước 2.5: Đón nhận Feedback, Sửa Bài Nguồn &amp; Nộp v2 Hoàn tất (done)](#bước-25-đón-nhận-feedback-sửa-bài-nguồn--nộp-v2-hoàn-tất-done)
4. [Bảng Ma trận Xử lý Sự cố &amp; Troubleshooting](#4-bảng-ma-trận-xử-lý-sự-cố--troubleshooting)

---

# 1. Bản đồ Kiến trúc & Phân bổ Thời gian (240 phút)

Phiên làm việc độc lập của mỗi học viên kéo dài **240 phút** (tính từ thời điểm bạn bấm nút kích hoạt trên Portal). Quy trình chia thành 2 phần rõ rệt:

```
+---------------------------------------------------------------------------------------------------+
|                        TỔNG THỜI LƯỢNG PHIÊN CÁ NHÂN: 240 PHÚT                                     |
+------------------------------------+--------------------------------------------------------------+
| PHẦN I: LÀM NHÓM 4 NGƯỜI (~60 - 70p)| PHẦN II: LÀM CÁ NHÂN ĐỘC LẬP (~170 - 180p)                   |
| - Phân vai 4 thành viên            | - Bước 2.1 - 2.3: Sửa 30 job nguồn & Nộp v1 (~110p)          |
| - Setup môi trường Docker          | - Bước 2.4: Nhận bài QC ngẫu nhiên & Viết feedback (~45p)    |
| - Chạy A/B/C & 3 ca QC pipeline    | - Bước 2.5: Nhận feedback, sửa v2 & Chuyển 'done' (~15p)     |
| - Hoàn thiện PRE-LABEL-REPORT.md   | *(Các bước cá nhân có thể làm xen kẽ tùy theo hàng đợi)*     |
| - LC nghiệm thu trước khi chuyển   |                                                              |
+------------------------------------+--------------------------------------------------------------+
```

---

# PHẦN I: THỰC HÀNH NHÓM 4 NGƯỜI (60 – 70 PHÚT)

### 4 Vai trò (Roles) và Ma trận Luân chuyển (Bắt buộc theo TEAMMATES.md)

- **R1 — Operator (Người gõ lệnh):** Trực tiếp ngồi máy, mở terminal, gõ lệnh Docker, kiểm tra container chạy xong hay báo lỗi.
- **R2 — Config Inspector (Người kiểm tra tham số):** Soát cờ lệnh (`--deltas`, `--voxel-size`), mở file `manifest.json` và file JSON kết quả kiểm tra từng key.
- **R3 — Geometry Analyst (Người xem hình ảnh 3D & Side view):** Mở các file ảnh `side-*.png`, quan sát đám mây điểm và các bounding box, phân tích độ chìm/lơ lửng.
- **R4 — Scribe (Người ghi chép số liệu & Bấm giờ):** Mở file `PRE-LABEL-REPORT.md`, chép số liệu từ `summary.csv` vào bảng, nhắc nhở thời gian.

**Ma trận đổi vai qua các lượt:**

| Thành viên | Lượt A ($\text{delta}=0$) | Lượt B ($\text{delta}=1.73$) | Lượt C ($\text{voxel}=0.32$) |    3 Ca QC Pipeline    |                        |                        |
| :----------: | :-------------------------------------------------------------------------------------------------: | :---------------------: | :---------------------: | :---------------------: |
| **M1** |                                       **R1 (Operator)**                                       |    R2 (Config Insp.)    |      R3 (Geometry)      |       R4 (Scribe)       |
| **M2** |                                          R2 (Config Insp.)                                          | **R3 (Geometry)** |       R4 (Scribe)       | **R1 (Operator)** |
| **M3** |                                            R3 (Geometry)                                            |       R4 (Scribe)       | **R1 (Operator)** |    R2 (Config Insp.)    |
| **M4** |                                             R4 (Scribe)                                             | **R1 (Operator)** |    R2 (Config Insp.)    | **R3 (Geometry)** |

---

### Bước 1.1: Chuẩn bị Thư mục, Phân vai & Kích hoạt Phiên (10 phút)

#### 1. Thao tác ở đâu?

- **Trên trình duyệt:** Mở Portal của lớp học (URL do Lab Coach - LC gửi qua Zalo/Discord/Slack của lớp).
- **Trên máy tính trạm của nhóm:** Mở ứng dụng **Terminal** (Linux/macOS) hoặc **PowerShell** (Windows).

#### 2. Thao tác chi tiết từng bước:

1. **Cả 4 thành viên (M1, M2, M3, M4):**
   - Đăng nhập Portal bằng tài khoản cá nhân do LC cấp.
   - Nhìn lên góc trên/giữa màn hình Portal $\rightarrow$ Tìm nút màu xanh có chữ **"Bắt đầu phiên 240 phút"** $\rightarrow$ Click chuột vào nút này.
   - **Xác nhận xem ở đâu:** Nhìn đồng hồ đếm ngược trên góc màn hình Portal bắt đầu chạy lùi từ `04:00:00`.
2. **R1 (Operator - M1):**
   - Mở Terminal, kiểm tra Docker đang bật:
     ```bash
     docker info
     ```
   - Nhìn dòng `Server Version` và `OSType: linux`. Nếu báo lỗi `Cannot connect to the Docker daemon` $\rightarrow$ Phải bật Docker Desktop lên trước!
   - Kiểm tra vị trí repo hiện tại:
     ```bash
     pwd
     # Kết quả phải trả về đường dẫn repo:
     # /home/leduchuy/project-coding/ai-in-action-projects/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student
     ```
3. **R4 (Scribe - M4):**
   - Tạo thư mục nhóm private nằm ngoài Git (để không bị commit lộ thông tin):
     ```bash
     mkdir -p ../K4-DAY13-Nhom01/output
     ```
   - Sao chép 2 file mẫu từ repo vào thư mục nhóm:
     ```bash
     cp TEAMMATES.md ../K4-DAY13-Nhom01/
     cp PRE-LABEL-REPORT.md ../K4-DAY13-Nhom01/
     ```
   - Mở file `../K4-DAY13-Nhom01/TEAMMATES.md` bằng VSCode/text editor, điền chính xác Họ tên, MSSV của 4 người và phân vai theo ma trận trên.

#### Checkpoint Bước 1.1:

| Tiêu chí         | Xem ở đâu?                         | Nhìn thấy gì là ĐẠT?                                                    |
| :----------------- | :------------------------------------ | :---------------------------------------------------------------------------- |
| Kích hoạt phiên | Trên trang Portal của cả 4 người | Đồng hồ đếm ngược đang chạy lùi từ`04:00:00`.                    |
| Docker sẵn sàng  | Terminal máy trạm                   | Lệnh`docker info` trả về thông tin hệ thống, không báo lỗi daemon. |
| File phân vai     | `../K4-DAY13-Nhom01/TEAMMATES.md`   | Có đủ 4 dòng họ tên, MSSV, và 3 cột vai trò A, B, C đã điền.     |

---

### Bước 1.2: Chạy Thực nghiệm 3 Lượt A / B / C (25 phút)

Ta chạy model PointPillars trên cùng 1 file PCD chuẩn `data/demo.pcd`. File này gồm **17.238 điểm LiDAR** của KITTI đã được gán nhãn chuyển đổi.

---

#### 📍 LƯỢT A: `delta = 0.0m`, `voxel_size = 0.16m` (Giả định sensor nằm sát mặt đất)

*Phân vai: R1 = M1 (Gõ lệnh) | R2 = M2 (Check config) | R3 = M3 (Xem ảnh) | R4 = M4 (Ghi chép)*

1. **R1 (Operator):** Chạy container Docker cho Lượt A từ thư mục gốc của repo:
   ```bash
   IMAGE='day13-pointpillars:lab'
   DATA_DIR="$(pwd)/data"
   OUT_DIR="$(pwd)/../K4-DAY13-Nhom01/output"

   docker run --rm --network none --cpus 4 --memory 4g \
     --mount "type=bind,source=$DATA_DIR,target=/data,readonly" \
     --mount "type=bind,source=$OUT_DIR,target=/out" \
     "$IMAGE" --data /data/demo.pcd --out /out/run-A --from KITTI --deltas 0 --voxel-size 0.16 --score-thresh 0.3
   ```
2. **Nhìn cái gì trên màn hình Terminal?**
   - Đợi khoảng 5–15 giây, Terminal sẽ in ra dòng chữ dạng:
     `frame=demo dataset=KITTI points=17238 z_ground=-1.730 passes=identity`
     `delta=0 voxel=0.16 boxes=1 rear=0 mean_z=-0.410 vehicles=1`
   - **Xác nhận:** Container kết thúc mà không in ra `Tracebachttps://github.com/VinUni-AI20k/K4-L2L3-Day13-Robotaxi-LiDAR-3D-Object-Student/releasesk` hay `Error`.
3. **R2 (Config Inspector) — Xác nhận file sinh ra ở đâu?**
   - Mở thư mục `../K4-DAY13-Nhom01/output/run-A/` kiểm tra xem có đủ 3 file sau không:
     1. `boxes-demo-delta-0.0-voxel-0.16.json`
     2. `side-demo-delta-0.0-voxel-0.16.png`
     3. `summary.csv`
   - Mở file `boxes-demo-delta-0.0-voxel-0.16.json` bằng text editor, nhìn các trường:
     `"delta": 0.0`, `"voxel_size": 0.16`, và đếm số phần tử trong mảng `"boxes": [ ... ]`.
4. **R3 (Geometry Analyst) — Mở ảnh nhìn cái gì?**
   - Mở file ảnh `../K4-DAY13-Nhom01/output/run-A/side-demo-delta-0.0-voxel-0.16.png`:
     - Trục nằm ngang (trục $X$): Khoảng cách từ xe tự hành ra phía trước (từ $0$ đến $70$ mét).
     - Trục thẳng đứng (trục $Z$): Độ cao (mét). Đường nét đứt màu xám là mốc $z=0$.
     - Các chấm xám/xanh: Đám mây điểm LiDAR của mặt đường và xe cộ.
     - Các hình chữ nhật màu đỏ viền quanh: Là các cuboid 3D được model dự đoán chiếu lên mặt phẳng $X-Z$.
     - **Hiện tượng thực tế nhìn thấy:** Chỉ có đúng **1 hộp** xuất hiện! Hầu như toàn bộ ô tô trên đường đều bị bỏ sót.
     - **Giải thích hiện tượng:** Vì ta đặt $\text{delta}=0$ (sai lệch hoàn toàn so với thực tế LiDAR đặt trên nóc xe cao $1.73m$), đám mây điểm khi đưa vào model bị lệch độ cao so với phân bố chuẩn mà mạng PointPillars được học $\rightarrow$ Model "bị mù", không nhận diện được xe.
5. **R4 (Scribe) — Ghi số liệu vào đâu?**
   - Mở file `../K4-DAY13-Nhom01/output/run-A/summary.csv`, đọc dòng dữ liệu:
     `demo,KITTI,0,0.16,1,-0.410`
   - Mở file `../K4-DAY13-Nhom01/PRE-LABEL-REPORT.md`, chép số liệu vào dòng **Lượt A** của bảng "Ba lượt inference thật": Số hộp = `1`, mean_z = `-0.410`.

---

#### 🔄 TIẾN HÀNH ĐỔI VAI LẦN 1

*Thành viên đổi vai: R1 = M4 (Gõ lệnh) | R2 = M1 (Check config) | R3 = M2 (Xem ảnh) | R4 = M3 (Ghi chép)*

---

#### 📍 LƯỢT B: `delta = 1.73m`, `voxel_size = 0.16m` — BASELINE CHUẨN

1. **R1 (Operator - M4):** Gõ lệnh chạy Lượt B:
   ```bash
   docker run --rm --network none --cpus 4 --memory 4g \
     --mount "type=bind,source=$DATA_DIR,target=/data,readonly" \
     --mount "type=bind,source=$OUT_DIR,target=/out" \
     "$IMAGE" --data /data/demo.pcd --out /out/run-B --from KITTI --deltas 1.73 --voxel-size 0.16 --score-thresh 0.3
   ```
2. **Nhìn Terminal:**
   - Dòng log in ra:
     `delta=1.73 voxel=0.16 boxes=13 rear=0 mean_z=-0.420 pedestrian=2 two-wheels=1 vehicles=10`
   - **Xác nhận:** Số hộp tăng vọt lên **13 hộp**!
3. **R2 (Config Inspector - M1):**
   - Mở thư mục `../K4-DAY13-Nhom01/output/run-B/`, xác nhận đủ 3 file tương ứng (`boxes-demo-delta-1.73-voxel-0.16.json`, `side-demo-delta-1.73-voxel-0.16.png`, `summary.csv`).
   - Mở file JSON, kiểm tra key `"delta": 1.73`.
4. **R3 (Geometry Analyst - M2):**
   - Mở ảnh `side-demo-delta-1.73-voxel-0.16.png` đặt song song cạnh ảnh của Lượt A:
     - **Nhìn thấy gì?** Xuất hiện 13 hộp đỏ bao bọc rất khít các cụm điểm ô tô chạy dọc trục $X$ từ $10m$ đến $50m$.
     - Đáy của các hộp đỏ tiếp xúc sát với hàng điểm mặt đường bên dưới.
     - **Bản chất rút ra:** Khi trừ $\text{delta}=1.73m$ trước inference, các điểm rơi đúng vào các trụ (pillar) chuẩn của mô hình, giúp mạng trích xuất đúng đặc trưng và phát hiện đủ các loại xe.
5. **R4 (Scribe - M3):**
   - Mở `summary.csv` của lượt B, chép số liệu vào bảng Lượt B: Số hộp = `13`, mean_z = `-0.420`.
   - Ghi câu trả lời vào báo cáo: *"Dịch delta trước inference làm thay đổi hoàn toàn số lượng hộp (từ 1 hộp lên 13 hộp) vì nó làm thay đổi phân bố điểm trong các pillar, KHÁC HOÀN TOÀN với việc tịnh tiến kết quả sau inference."*

---

#### 🔄 TIẾN HÀNH ĐỔI VAI LẦN 2

*Thành viên đổi vai: R1 = M3 (Gõ lệnh) | R2 = M4 (Check config) | R3 = M1 (Xem ảnh) | R4 = M2 (Ghi chép)*

---

#### 📍 LƯỢT C: `delta = 1.73m`, `voxel_size = 0.32m` — TĂNG KÍCH THƯỚC PILLAR

1. **R1 (Operator - M3):** Gõ lệnh chạy Lượt C:
   ```bash
   docker run --rm --network none --cpus 4 --memory 4g \
     --mount "type=bind,source=$DATA_DIR,target=/data,readonly" \
     --mount "type=bind,source=$OUT_DIR,target=/out" \
     "$IMAGE" --data /data/demo.pcd --out /out/run-C --from KITTI --deltas 1.73 --voxel-size 0.32 --score-thresh 0.3
   ```
2. **Nhìn Terminal:**
   - Dòng log in ra:
     `delta=1.73 voxel=0.32 boxes=6 rear=0 mean_z=-0.380 pedestrian=6`
   - **Xác nhận:** Số hộp giảm từ 13 xuống còn **6 hộp**, và toàn bộ bị gán nhãn thành `pedestrian`!
3. **R3 (Geometry Analyst - M1):**
   - Mở ảnh `side-demo-delta-1.73-voxel-0.32.png`:
     - **Nhìn thấy gì?** Kích thước pillar tăng gấp đôi ($0.32m$) khiến độ phân giải không gian bị giảm một nửa.
     - Các đặc trưng hình học sắc nét của ô tô bị mờ đi do nhiều điểm bị gom chung vào 1 trụ to $\rightarrow$ Mô hình bị nhầm lẫn nghiêm trọng về phân loại class.
4. **R4 (Scribe - M2):**
   - Chép số liệu Lượt C vào bảng: Số hộp = `6`, mean_z = `-0.380`.
   - Ghi kết luận so sánh B vs C: *"Pillar size 0.16m giữ được chi tiết hình học tốt hơn, pillar size 0.32m làm mất hộp và sai lệch class."*

#### Checkpoint Bước 1.2:

| Tiêu chí            | Xem ở đâu?                              | Nhìn thấy gì là ĐẠT?                                                    |
| :-------------------- | :----------------------------------------- | :---------------------------------------------------------------------------- |
| Kết quả Lượt A    | `../K4-DAY13-Nhom01/output/run-A/`       | Đủ 3 file JSON, PNG, CSV. Số hộp = 1.                                     |
| Kết quả Lượt B    | `../K4-DAY13-Nhom01/output/run-B/`       | Đủ 3 file. Số hộp = 13 (có xe, người, xe máy).                        |
| Kết quả Lượt C    | `../K4-DAY13-Nhom01/output/run-C/`       | Đủ 3 file. Số hộp = 6 (bị giảm độ phân giải).                       |
| Bảng số liệu A/B/C | `../K4-DAY13-Nhom01/PRE-LABEL-REPORT.md` | Điền đủ cả 3 dòng, có số hộp, mean_z và giải thích nguyên nhân. |

---

### Bước 1.3: Chạy 3 Ca QC Pipeline & Nhận diện Lỗi Hệ thống (15 phút)

#### 🔄 TIẾN HÀNH ĐỔI VAI LẦN 3

*Thành viên đổi vai: R1 = M2 (Gõ lệnh) | R2 = M3 (Check config) | R3 = M4 (Xem ảnh) | R4 = M1 (Ghi chép)*

Mục tiêu bước này: Dùng script `pipeline-qc-cases.py` để tạo ra 3 ca mô phỏng từ kết quả Lượt B chuẩn, giúp cả nhóm phân biệt: **Khi nào lỗi cả batch thì DỪNG LẠI, khi nào lỗi 1 hộp thì SỬA.**

1. **R1 (Operator - M2):** Gõ lệnh tạo 3 ca QC:
   ```bash
   PRACTICE_DIR="$(pwd)/practice"
   docker run --rm --network none --cpus 4 --memory 4g --entrypoint python \
     --mount "type=bind,source=$PRACTICE_DIR,target=/practice,readonly" \
     --mount "type=bind,source=$DATA_DIR,target=/data,readonly" \
     --mount "type=bind,source=$OUT_DIR,target=/out" \
     "$IMAGE" /practice/pipeline-qc-cases.py \
     --prediction /out/run-B/boxes-demo-delta-1.73-voxel-0.16.json \
     --out /out/qc-cases --pcd /data/demo.pcd
   ```
2. **R2 (Config Inspector - M3) — Xem ở đâu để xác nhận?**
   - Mở thư mục `../K4-DAY13-Nhom01/output/qc-cases/`, kiểm tra phải có đủ:
     - `case-correct.json` và `side-correct.png`
     - `case-batch-z.json` và `side-batch-z.png`
     - `case-one-box-z.json` và `side-one-box-z.png`
     - `manifest.json`
   - Mở file `manifest.json`, nhìn dòng:
     `"height_offset_m": 3.46` (hoặc tổng của $\text{delta} + z_{ground}$).
3. **R3 (Geometry Analyst - M4) — Nhìn ảnh để phân biệt lỗi:**
   - Mở đồng thời 3 ảnh:
     1. `side-correct.png`: Đây là bản chuẩn giữ nguyên từ Lượt B. Hộp bám sát các cụm điểm xe trên mặt đất.
     2. `side-batch-z.png`: **NHÌN KỸ:** Toàn bộ 13 hộp màu đỏ đều bị chìm sâu xuống lòng đất cùng 1 khoảng cách đúng bằng `height_offset_m`! Không có hộp nào nằm trên mặt đường cả.
        $\rightarrow$ **KẾT LUẬN HÀNH ĐỘNG:** **DỪNG SỬA TAY NGAY LẬP TỨC!** Đây là lỗi hệ thống do kỹ sư pipeline quên cộng bù lại lượng $z$ khi xuất dữ liệu. Nếu ngồi sửa tay từng hộp thì sửa cả ngày cũng không hết và sẽ bị sai tiếp ở các frame sau. Phải báo ngay cho LC/kỹ sư sửa code pipeline!
     3. `side-one-box-z.png`: **NHÌN KỸ:** 12 hộp vẫn nằm yên đúng vị trí bám mặt đất, chỉ có duy nhất **01 hộp đầu tiên** bị chìm xuống đất.
        $\rightarrow$ **KẾT LUẬN HÀNH ĐỘNG:** **KIỂM TRA VÀ SỬA TỪNG HỘP**. Đây là lỗi cục bộ của đối tượng đó (do điểm bị che khuất hoặc model nhận nhầm), annotator sẽ dùng công cụ trong CVAT để kéo hộp đó lên.
4. **R4 (Scribe - M1):**
   - Mở file `../K4-DAY13-Nhom01/PRE-LABEL-REPORT.md`, điền bảng **"Ca QC có kiểm soát"**:
     - `case-correct`: 0 hộp lệch, Giữ nguyên.
     - `case-batch-z`: 13/13 hộp lệch, Dừng batch báo pipeline.
     - `case-one-box-z`: 1/13 hộp lệch, Kiểm tra sửa từng hộp.

#### Checkpoint Bước 1.3:

| Tiêu chí        | Xem ở đâu?                           | Nhìn thấy gì là ĐẠT?                                                          |
| :---------------- | :-------------------------------------- | :---------------------------------------------------------------------------------- |
| Đủ 3 ca QC      | `../K4-DAY13-Nhom01/output/qc-cases/` | Đủ 3 file JSON, 3 file ảnh Side view và`manifest.json`.                       |
| Hiểu lỗi batch  | Ảnh`side-batch-z.png`                | Thấy toàn bộ hộp bị chìm đồng loạt$\rightarrow$ Biết phải dừng batch. |
| Hiểu lỗi 1 hộp | Ảnh`side-one-box-z.png`              | Thấy chỉ 1 hộp bị chìm$\rightarrow$ Biết phải sửa cục bộ.               |

---

### Bước 1.4: Viết Nhận xét Cá nhân, Hoàn tất Báo cáo & Nghiệm thu (15 phút)

#### 1. Thao tác ở đâu?

Mở file `../K4-DAY13-Nhom01/PRE-LABEL-REPORT.md` trên máy tính trạm hoặc chia sẻ file cho 4 người cùng mở.

#### 2. Thao tác chi tiết:

Mục **"Nhận xét cá nhân"** trong báo cáo: **Cả 4 thành viên (M1, M2, M3, M4) mỗi người tự viết 1 đoạn văn riêng** gồm 4 ý:

- **Ý 1:** Em đã làm những vai trò gì qua các lượt? (VD: *Em làm Operator ở lượt A, Config ở lượt B, Geometry ở lượt C, Scribe ở ca QC*).
- **Ý 2:** Quan sát định lượng rút ra từ file kết quả? (VD: *Ở lượt B summary.csv có 13 hộp với mean_z=-0.420m, sang lượt C voxel=0.32m làm số hộp giảm còn 6 và sai nhãn sang pedestrian*).
- **Ý 3:** Phép đổi z có ý nghĩa gì? (VD: *Phép đổi z_source = z_model + z_ground + delta giúp đưa hộp từ hệ tọa độ của mô hình trở lại đúng độ cao thực tế của mặt đường*).
- **Ý 4:** Quyết định khi gặp lỗi batch và điều còn chưa chắc? (VD: *Khi lệch cả batch như case-batch-z em sẽ dừng sửa và báo kỹ sư pipeline. Em còn chưa chắc chắn về hướng yaw của xe tải ở xa vì mật độ điểm LiDAR bị thưa*).

#### 3. Trình LC Nghiệm thu:

- Gọi Lab Coach (LC) lại bàn kiểm tra thư mục `K4-DAY13-Nhom01/`.
- LC xem file `TEAMMATES.md` (có đủ 4 người), xem file `PRE-LABEL-REPORT.md` (có đủ 4 nhận xét cá nhân), và xem lướt các ảnh trong thư mục `output/`.
- **LC xác nhận:** *"Nhóm 01 đã hoàn thành phần pre-label, các bạn có thể chuyển sang làm bài nguồn cá nhân trên CVAT."*

---

# PHẦN II: THỰC HÀNH CÁ NHÂN TRÊN CVAT & PORTAL (~170 PHÚT)

Sau khi LC duyệt, **cả 4 thành viên giải tán về máy cá nhân của mình**. Mỗi người tự làm việc trên tài khoản riêng được giao 30 job.

---

### Bước 2.1: Mở Job & Nhận diện Giao diện CVAT 3D

#### 1. Mở Job ở đâu?

1. Trên trình duyệt web cá nhân, vào trang **Portal**.
2. Click vào thẻ menu **"Bài nguồn"**.
3. Nhìn danh sách 30 dòng (mỗi dòng là 1 Job/Frame): Click vào nút màu xanh **"Mở bài nguồn trong CVAT"** ở dòng đầu tiên.
4. Một tab mới mở ra dẫn vào hệ thống CVAT (`cvat.note.transformerlabs.ai`).

#### 2. Nhận diện các khu vực trên màn hình CVAT 3D:

```
+----------------------------------------------------------------------------------------------------+
| MENU CHÍNH (Góc trên bên trái: Nút SAVE, Undo, Redo)                                               |
+-------------------+-----------------------------------------------------------+--------------------+
| THANH CÔNG CỤ     | KHUNG NHÌN 3D CHÍNH (3D Perspective View)                 | DANH SÁCH OBJECTS  |
| (Bên trái)        | - Chuột trái: Xoay góc nhìn (Orbit)                       | (Bên phải)         |
| - Mũi tên: Chọn   | - Chuột phải: Dịch chuyển (Pan)                           | - Danh sách các    |
| - Khối hộp 3D:    | - Cuộn chuột: Phóng to / Thu nhỏ (Zoom)                   |   hộp đã có.       |
|   Vẽ Cuboid mới   +-----------------------------------------------------------+ - Đổi class ở đây. |
|                   | 3 KHUNG HÌNH CHIẾU TRỰC GIAO (Orthographic Views)         | - Khóa / Ẩn hộp.   |
|                   | [Top View (Trên)]   [Side View (Bên)]   [Front View (Trước)| - Xóa hộp (Thùng   |
|                   +-----------------------------------------------------------+   rác).            |
|                   | KHUNG ẢNH CAMERA 2D (Chiếu ảnh chụp thật tương ứng)       |                    |
+-------------------+-----------------------------------------------------------+--------------------+
```

---

### Bước 2.2: Kỹ thuật Hiệu chỉnh 7 DoF Bằng Tay trên CVAT (Cầm tay chỉ việc)

Mỗi hộp Cuboid 3D có **7 bậc tự do (7 DoF)**: Tâm $(x, y, z)$, Kích thước $(\text{dài } l, \text{rộng } w, \text{cao } h)$ và Góc xoay $\text{Yaw}$. Dưới đây là hướng dẫn thao tác chi tiết:

#### 1. Đổi Class (Nhãn đối tượng) khi model nhận nhầm:

- **Hiện tượng:** Model vẽ hộp xe con (`vehicles`), nhưng nhìn vào khung ảnh Camera thấy rõ ràng là người đi xe máy.
- **Cách sửa:**
  1. Click chuột trái vào chiếc hộp đó trên màn hình 3D (hộp sẽ sáng viền lên).
  2. Nhìn sang bảng **Objects** ở cột bên phải màn hình: Item của chiếc hộp đó sẽ tự động được bôi đậm.
  3. Click vào ô dropdown **Label** (hiện tại đang là `vehicles`) $\rightarrow$ Chọn lại thành `two-wheels`.
  4. Hộp trên màn hình 3D sẽ đổi sang màu đặc trưng của class `two-wheels`.

#### 2. Xoay hướng đầu xe (Chỉnh Yaw — Loại bỏ lỗi ngược 180°):

- **Hiện tượng:** Hộp ôm vừa thân xe, nhưng mũi tên chỉ hướng (màu vàng/trắng) lại chĩa về phía sau xe (ngược $180^\circ$).
- **Cách nhìn xác nhận:** Nhìn vào khung ảnh Camera $\rightarrow$ Thấy đèn pha và kính chắn gió trước đang quay sang phải, trong khi mũi tên trong 3D lại chĩa sang trái.
- **Cách sửa:**
  1. Nhìn xuống khung hình chiếu **Top View (Góc nhìn từ trên xuống)** ở phía dưới màn hình.
  2. Click vào hộp để làm xuất hiện vòng tròn xoay hoặc nút chấm tròn điều khiển hướng ở đầu mũi tên.
  3. Bấm giữ chuột trái vào chấm tròn điều khiển $\rightarrow$ Kéo xoay tròn $180^\circ$ cho đến khi mũi tên chĩa thẳng theo hướng đèn pha xe trên ảnh camera.
  4. Thả chuột ra.

#### 3. Chỉnh kích thước hộp $(l, w, h)$ khi bị co cụm hoặc quá to:

- **Hiện tượng:** Xe ô tô ở xa bị thưa điểm, model chỉ vẽ một chiếc hộp ngắn tẹo ôm 3–4 điểm ở đầu xe.
- **Cách sửa:**
  1. Nhìn vào khung **Top View** (để chỉnh chiều dài $l$ và chiều rộng $w$) hoặc khung **Side View** (để chỉnh chiều cao $h$).
  2. Rê chuột vào đường viền cạnh của chiếc hộp $\rightarrow$ Con trỏ chuột chuyển từ hình bàn tay sang **mũi tên 2 chiều** ($\leftrightarrow$ hoặc $\updownarrow$).
  3. Bấm giữ chuột trái $\rightarrow$ Kéo dãn cạnh hộp ra cho đúng chiều dài thực tế của dòng xe con (~$4.2m - 4.8m$) hoặc thu hẹp lại nếu hộp bị trùm ra ngoài vỉa hè.

#### 4. Căn đáy hộp bám sát mặt đường cục bộ (Local Ground):

- **Hiện tượng:** Chiếc hộp đang lơ lửng trên không trung cách mặt đường $0.5m$, hoặc cắm sâu xuống dưới lòng đất.
- **Cách sửa:**
  1. Nhìn vào khung **Side View (Góc nhìn bên)** hoặc **Front View (Góc nhìn trước)**.
  2. Cuộn chuột để phóng to cụm điểm ngay bên dưới chiếc xe: Bạn sẽ nhìn thấy một dải điểm nằm ngang hơi dày — đó chính là bề mặt đường nhựa!
  3. Rê chuột vào **cạnh đáy nằm ngang của chiếc hộp** $\rightarrow$ Con trỏ chuột chuyển thành mũi tên 2 chiều thẳng đứng ($\updownarrow$).
  4. Bấm giữ chuột trái $\rightarrow$ Kéo cạnh đáy dịch lên/xuống sao cho cạnh đáy **tiếp xúc vừa chạm mép trên của dải điểm mặt đường**.
  5. Thả chuột ra. Kiểm tra lại hộp không bị cắm xuống đất và không bị bay lơ lửng.

#### 5. Xóa hộp thừa (False Positive):

- **Hiện tượng:** Model vẽ một chiếc hộp màu đỏ vào bụi cây ven đường, vách tường nhà hoặc bóng ma nhiễu.
- **Cách sửa:**
  1. Click chuột trái chọn chiếc hộp đó trên màn hình 3D.
  2. Bấm phím **Delete** trên bàn phím (hoặc click vào icon hình **Thùng rác** màu đỏ bên cạnh tên hộp ở cột Objects bên phải).
  3. Hộp biến mất khỏi không gian 3D.

#### 6. Thêm hộp thiếu (False Negative - Đặc biệt là `Animal` và `Obstacle`):

- **Hiện tượng:** Model bỏ sót một người đi bộ (`pedestrian`) hoặc một tảng đá/vật cản (`Obstacle`) trên làn đường.
- **Cách sửa:**
  1. Nhìn thanh công cụ bên trái màn hình, click vào icon **Draw 3D Cuboid** (hoặc bấm phím tắt **N** trên bàn phím).
  2. Một popup nhỏ hiện ra: Chọn **Label** muốn vẽ (ví dụ: `Obstacle` hoặc `Animal`).
  3. Chọn phương thức vẽ: Thường chọn **By 2 points** hoặc **By 4 points**.
  4. Click chuột lên cụm điểm trên màn hình 3D để dựng khung hộp sơ bộ.
  5. Dùng Top View và Side View kéo các cạnh cho vừa khít với vật thể.

---

### Bước 2.3: Lưu trong CVAT và Nộp Snapshot v1 trên Portal

#### 1. Lưu trong CVAT:

- Sau khi đã chỉnh sửa xong toàn bộ các hộp của frame:
- Nhìn lên góc trên bên trái menu CVAT $\rightarrow$ Click vào nút **Save** (hoặc bấm tổ hợp phím **Ctrl + S**).
- **Xem ở đâu để biết đã lưu thành công?** Nhìn góc trên bên phải màn hình xuất hiện một thông báo màu xanh lá cây ghi: `"Changes saved successfully"`.

#### 2. Nộp v1 trên Portal:

1. Đóng tab CVAT hoặc quay trở lại tab **Portal**.
2. Tại dòng của Job vừa làm:
   - Nhìn vào ô dropdown **"Phạm vi rà soát"**:
     - Chọn **"Toàn frame, gồm hộp thiếu/thừa"**: Nếu bạn đã kiểm tra kỹ toàn bộ 360 độ xung quanh xe, soát cả vật cản lẫn hộp thừa.
     - Chọn **"Một phần"**: Nếu bạn chỉ mới sửa tập trung vào vài chiếc xe ở làn giữa.
   - Click vào nút màu xanh **"Nộp job vào hàng đợi QC"**.
3. **Xác nhận trạng thái:**
   - Cột Trạng thái của job đó trên Portal chuyển từ `draft` sang `importing`, sau đó chuyển sang `ready`.
   - Xuất hiện nút **"Đối chiếu snapshot v1 chỉ đọc"** $\rightarrow$ Click vào xem thử: Bạn sẽ thấy phiên bản v1 của mình đã được đóng băng cố định để gửi cho người khác chấm chéo.

> [!CAUTION]
> Sau khi đã nộp v1, **TUYỆT ĐỐI KHÔNG mở lại CVAT để sửa tiếp job đó** trong lúc đang chờ người khác review. Hãy chuyển sang làm tiếp các job nguồn khác!

---

### Bước 2.4: Nhận bài & Soi lỗi QC Chéo (QC Reviewer)

#### 1. Nhận bài ở đâu?

1. Trên thanh menu chính của Portal, click chuyển sang tab **"QC"**.
2. Nhìn thấy nút màu xanh **"Nhận bài QC ngẫu nhiên"** $\rightarrow$ Click chuột vào nút này.
3. Hệ thống sẽ gán cho bạn 1 snapshot của một bạn học viên khác.
4. **Nhìn đồng hồ:** Bạn có **20 phút giữ quyền (lease)** để hoàn thành bài này.

#### 2. Thao tác soi lỗi:

1. Click vào nút **"Xem snapshot 3D chỉ đọc"**. Màn hình viewer 3D và ảnh Camera hiện ra (giao diện chỉ đọc, không thể kéo sửa hộp).
2. Xoay 3D, nhìn Top view, nhìn ảnh camera để tìm lỗi:
   - Có hộp nào bị quay ngược đầu xe $180^\circ$ không?
   - Có xe máy nào bị gán nhãn thành ô tô không?
   - Có người đi bộ nào chưa được đóng hộp không?
   - Có hộp nào đáy bị cắm xuống đất hay lơ lửng không?
3. Khi phát hiện 1 lỗi $\rightarrow$ Click chuột trái vào chiếc hộp đó:
   - **Xem ở đâu để lấy ID?** Ngay trên chiếc hộp hoặc góc màn hình sẽ hiện lên số ID, ví dụ: `ID: 17`.

#### 3. Viết nhận xét trên Form Portal:

Quay lại form nhập nhận xét trên Portal, click nút **"Thêm nhận xét"** và điền đủ 4 ô:

1. **Ô 1 — Loại lỗi:** Chọn từ dropdown (VD: `Hướng`, `Sai class`, `Vị trí`, `Kích thước`, `Thiếu đối tượng`).
2. **Ô 2 — ID QC hoặc vùng tọa độ:** Gõ `ID 17` (Nếu là lỗi thiếu đối tượng chưa có hộp, hãy gõ tọa độ tương đối, ví dụ: `Vùng bên phải xe, cách tâm 15m`).
3. **Ô 3 — Bằng chứng & góc nhìn:** Gõ rõ bạn nhìn thấy ở đâu (VD: *Ở góc nhìn Top và đối chiếu ảnh Camera 1, đèn pha xe quay về bên phải nhưng mũi tên yaw lại chỉ sang trái*).
4. **Ô 4 — Đề xuất sửa:** Gõ hướng xử lý cho tác giả (VD: *Đề xuất tác giả vào CVAT xoay lại yaw 180 độ theo chiều tiến của xe*).
5. Nếu thấy lỗi khác $\rightarrow$ Bấm **"Thêm nhận xét"** để viết tiếp.
6. Chọn phạm vi review của bạn: `Một phần` hoặc `Toàn frame`.
7. Click nút màu xanh **"Nộp toàn bộ feedback"**.
8. **Xác nhận:** Form đóng lại và hệ thống thông báo nộp feedback thành công.

---

### Bước 2.5: Đón nhận Feedback, Sửa Bài Nguồn & Nộp v2 Hoàn tất (done)

#### 1. Xem Feedback ở đâu?

1. Trên Portal, quay lại tab **"Bài nguồn"**.
2. Tìm những job đã có người review: Trên thẻ bài sẽ xuất hiện khung màu vàng có tiêu đề **"Feedback từ Reviewer"**.
3. Đọc từng dòng nhận xét: Reviewer bắt lỗi ở ID nào, lỗi gì, bằng chứng họ đưa ra là gì.

#### 2. Mở CVAT sửa bài:

1. Click nút **"Mở bài nguồn trong CVAT"** ở ngay trên thẻ bài đó.
2. Tìm đến đúng ID chiếc hộp bị bắt lỗi.
3. Đối chiếu giữa ý kiến của Reviewer với ảnh Camera và cụm điểm LiDAR:
   - **Nếu Reviewer nói đúng:** Thực hiện sửa lại (xoay lại yaw, đổi class, kéo lại kích thước...) $\rightarrow$ Bấm **Save (`Ctrl + S`)** trong CVAT.
   - **Nếu Reviewer nói sai (bắt lỗi oan):** Giữ nguyên trong CVAT không sửa.

#### 3. Nộp v2 trên Portal:

1. Quay lại tab Portal ở thẻ bài nguồn đó.
2. Nhìn xuống mục **"Kết luận"**, click chọn 1 trong 3 nút tròn (radio button):
   - **Đồng ý:** Nếu bạn công nhận reviewer đúng và đã sửa trong CVAT.
   - **Chưa đồng ý:** Nếu bạn thấy reviewer bắt sai và quyết định giữ nguyên.
   - **Cần coach phân xử:** Nếu hai bên mâu thuẫn hoặc điểm quá mờ không thể xác định.
3. Nhìn xuống ô văn bản **"Save ở bài nguồn rồi ghi đã sửa gì / lý do"**:
   - Nếu Đồng ý: Gõ *"Đã kiểm tra lại camera trước, đồng ý yaw bị ngược. Đã xoay lại 180 độ và Save trong CVAT."*
   - Nếu Chưa đồng ý: Gõ *"Giữ nguyên kích thước vì ảnh camera cho thấy phần đuôi xe bị xe buýt che khuất, chiều dài 4.5m là đúng thực tế xe."*
4. Click nút màu xanh **"Nộp bản sửa và phản hồi"**.
5. **Xác nhận hoàn thành:** Cột trạng thái của job nguồn chuyển sang màu xanh lá cây với chữ **`done`**! Vòng đời của job này đã hoàn thành 100%.

---

# 4. Bảng Ma trận Xử lý Sự cố & Troubleshooting

| Bạn gặp hiện tượng gì?                                                | Xem / Kiểm tra ở đâu?             | Cách xử lý dứt điểm                                                                                                                                                                                                                                                           |
| :-------------------------------------------------------------------------- | :------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Không đăng nhập được CVAT hoặc báo lỗi quyền truy cập** | Thanh địa chỉ trình duyệt CVAT   | Kiểm tra xem có đang đăng nhập tài khoản cá nhân khác không. Bấm nút Logout ở góc trên bên phải CVAT, sau đó đăng nhập lại bằng đúng username/password do LC cấp.                                                                                      |
| **Bấm Nộp v1 trên Portal báo lỗi không cho nộp**               | Màn hình CVAT                       | Bạn chưa bấm**Save** trong CVAT! Hãy quay lại CVAT bấm `Ctrl + S`, đợi hiện chữ xanh "Changes saved successfully" rồi mới quay lại Portal bấm Nộp.                                                                                                           |
| **Hàng đợi QC báo "Không có bài sẵn sàng"**                  | Tab QC trên Portal                   | Các bạn khác trong lớp chưa nộp kịp v1. Hãy quay lại tab**Bài nguồn** để làm tiếp các job khác của mình, 10–15 phút sau quay lại bấm kiểm tra lại. Chờ hàng đợi không bị trừ điểm.                                                          |
| **Màn hình CVAT 3D bị đen xì hoặc giật lag khủng khiếp**     | Trình duyệt Chrome/Edge             | Tắt bớt các tab YouTube, tab mạng xã hội đang mở. Bấm tổ hợp phím`Shift + F5` để reload lại trang CVAT. Nếu dùng laptop có 2 card màn hình, hãy cắm sạc để kích hoạt card rời.                                                                        |
| **Bị mất quyền QC khi đang viết nhận xét dở dang**            | Đồng hồ đếm ngược trên tab QC | Bạn đã để tab không hoạt động quá 20 phút nên hệ thống thu hồi bài giao cho người khác. Hãy bấm**"Nhận bài QC ngẫu nhiên"** để lấy bài mới. Lần sau hãy bấm Nộp feedback trước khi rời máy tính.                                       |
| **Reviewer bắt lỗi vô lý, đòi xóa hộp đúng của bạn**      | Khung Feedback trên Portal           | Không làm theo reviewer! Trong form phản hồi v2, chọn nút**"Chưa đồng ý"** hoặc **"Cần coach phân xử"**, ghi rõ lý do vào ô văn bản và bấm Nộp phản hồi. Coach sẽ là người chấm điểm cuối cùng dựa trên bằng chứng bạn đưa ra. |
| **Terminal chạy Docker báo lỗi "permission denied"**               | Cửa sổ Terminal                     | Thêm`sudo` vào đầu lệnh Docker (trên Linux) hoặc mở PowerShell bằng quyền `Run as Administrator` (trên Windows).                                                                                                                                                     |
