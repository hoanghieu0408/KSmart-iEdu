# KSmart iEdu — Prototype phân hệ “Công việc”

Prototype desktop-first (HTML/CSS/JavaScript thuần, không cần backend) cho phân hệ **Công việc / Giao việc nội bộ** của CMS KSmart iEdu, dựng bám theo BRD/SRS v1.0 (25/08/2026).

---

## 1. Cách chạy

Prototype là **một file duy nhất** `index.html`, không cần build, không cần cài đặt.

- **Cách nhanh nhất:** nhấp đúp `index.html` để mở bằng trình duyệt (khuyến nghị Chrome/Edge/Firefox bản mới).
- **Hoặc** kéo file thả vào một tab trình duyệt.
- **Hoặc** chạy một web server tĩnh cho “sạch” (ví dụ khi cần tránh chặn font):

```bash
# tại thư mục chứa index.html
python3 -m http.server 8080
# rồi mở http://localhost:8080
```

> Màn hình tối ưu cho độ rộng **≥ 1280px** (chuẩn CMS desktop). Khi cửa sổ hẹp hơn, bảng dữ liệu cuộn ngang trong card, không vỡ cột.
> Font **Be Vietnam Pro** tải từ Google Fonts; nếu máy không có mạng, hệ thống tự dùng font Arial thay thế (không ảnh hưởng chức năng).

---

## 2. Có gì trong prototype

Đủ **3 trạng thái thao tác** theo yêu cầu mục 9.1:

1. **Màn danh sách công việc** — data table (không phải Kanban) đúng cấu trúc màn Học sinh tham chiếu: chip phạm vi nhanh, ô tìm kiếm, bộ lọc nâng cao, tùy chỉnh cột, phân trang, status chip, progress bar, thao tác dòng, các trạng thái rỗng/đang tải/lỗi.
2. **Drawer Tạo mới / Giao việc** — drawer phải 760px, 2 tab *Thông tin chung* + *Checklist*, đủ field bắt buộc, validate khi Giao việc; **Lưu nháp** và **Giao việc** tạo trạng thái khác nhau.
3. **Drawer Chi tiết / Xử lý** — drawer 860px, 4 tab *Thông tin / Checklist / Trao đổi & tệp / Lịch sử*.

Tương tác mock đầy đủ: **lưu nháp, giao việc, bắt đầu xử lý, tick checklist, cập nhật tiến độ, bình luận + @mention, nộp kết quả, xác nhận/trả về bổ sung, gia hạn, chuyển người xử lý, tạm dừng/tiếp tục, hủy, xóa nháp** — **mọi thao tác đều ghi vào Nhật ký hoạt động** (append-only) và bắn toast/thông báo mock.

---

## 3. Thử phân quyền (điểm nhấn)

Bấm vào **khu vực tài khoản góc phải trên** (đang là *Super Admin*) để đổi vai trò đang xem. Có 5 vai trò mock:

| Vai trò | Phạm vi xem | Ghi chú |
|---|---|---|
| **Super Admin** | Mọi công việc | Quản trị toàn quyền |
| **Nguyễn Văn Bình** — Hiệu trưởng (BGH) | Toàn trường, **kể cả việc riêng tư** | Tạo/giao/duyệt/điều hành |
| **Đỗ Mai Phương** — Tổ trưởng | Việc của đơn vị + việc liên quan | Tạo/giao trong đơn vị |
| **Nguyễn Thu Hà** — Giáo viên *(có quyền tạo việc)* | Việc liên quan + việc công khai đơn vị | Có nút “Tạo mới” |
| **Trần Minh Anh** — Giáo viên | Việc liên quan + công khai đơn vị | **Không** có nút “Tạo mới” |

Quy tắc trọng yếu đã cài đặt (mục 2.3): **mặc định “Chỉ người liên quan”** — giáo viên không được phân công **không nhìn thấy** công việc riêng tư của người khác; chỉ người liên quan và BGH/quản trị mới xem được. Ví dụ *CV-2026-0004 (riêng tư)* hiển thị với Super Admin/BGH và cô Thu Hà (người chịu trách nhiệm) nhưng **ẩn** với thầy/cô không liên quan.

Các nút thao tác được **ẩn/disable theo quyền** (không chỉ cảnh báo sau khi bấm): người thực hiện không có nút Xác nhận/Trả về; người không phải người xử lý không thấy nút Bắt đầu/Nộp kết quả; v.v.

---

## 4. Dữ liệu mock

12 công việc tiếng Việt (mã CV-2026-XXXX) phủ đủ trạng thái: **Nháp, Mới, Đang thực hiện, Chờ xác nhận, Hoàn thành, Quá hạn, Tạm dừng**. Hạn xử lý được sinh tương đối theo thời điểm mở nên các trạng thái “sắp đến hạn / quá hạn” luôn đúng khi demo.

Gợi ý luồng demo nhanh:
- Mở **CV-2026-0011** (Chờ xác nhận) → xem kết quả đã nộp → *Xác nhận hoàn thành* / *Trả về bổ sung*.
- Mở **CV-2026-0013** (Đang thực hiện) → tab *Checklist* tick đủ mục → tiến độ 100% (vẫn cần *Nộp kết quả*).
- Bấm **+ Tạo mới** → thử *Giao việc* khi thiếu deadline (lỗi inline) → điền đủ → giao thành công.
- Dùng menu **“…”** trên một dòng để *Gia hạn* / *Chuyển người xử lý* (bắt buộc nhập lý do) → xem tab *Lịch sử* có ghi giá trị cũ/mới.

---

## 5. Design system

Toàn bộ token khai báo dưới dạng **CSS variables** ở đầu file (`:root`) theo mục 6 của tài liệu: bảng màu `--ks-*`, layout shell (rail 66px / menu 262px / top bar), typography Be Vietnam Pro, spacing, bo góc, và bảng màu chip trạng thái. Icon dùng bộ outline Lucide (inline SVG, đồng nhất).

## 6. Ngoài phạm vi (mở rộng sau MVP)

Theo tài liệu, các phần sau **không** thuộc prototype MVP và chưa được dựng: báo cáo/KPI công việc, template & công việc lặp, Gantt/phân bổ tải việc, push mobile/email/SMS, giao việc liên trường. Mục “Báo cáo công việc” ở menu trái để nhãn *Mở rộng*.
