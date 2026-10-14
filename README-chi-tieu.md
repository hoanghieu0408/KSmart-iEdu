# KSmart iEdu — Prototype phân hệ “Công việc”

Prototype desktop-first (HTML/CSS/JavaScript thuần, không cần backend) cho phân hệ **Công việc / Giao việc nội bộ** của CMS KSmart iEdu, dựng bám theo BRD/SRS v1.0 (25/08/2026). Về sau được mở rộng thêm các module **Chỉ tiêu (KPI)**, **Dự án công việc**, **Quản lý tài liệu** và **Quản lý kho (Xuất–Nhập–Tồn, nhiều kho)** — tất cả **additive**, không phá tính năng cũ. Từ 14/10/2026 bổ sung **lớp giao diện mobile** (responsive shell) dùng chung cho mọi màn hình.

---

## 1. Cách chạy

Prototype là **một file duy nhất** `index.html`, không cần build, không cần cài đặt.

- Nhấp đúp `index.html` để mở bằng trình duyệt (khuyến nghị Chrome/Edge/Firefox bản mới), hoặc chạy web server tĩnh:

```bash
python3 -m http.server 8080   # rồi mở http://localhost:8080
```

> Giao diện co giãn **full theo chiều rộng màn hình** trên web (màn hình kho không còn bị giới hạn 1120px). Chạy tốt trên mobile (≤ 860px): rail + sidemenu thu thành **ngăn kéo trượt** mở bằng nút ☰ trên thanh trên, nội dung co về full-width, không còn cuộn ngang. Font **Be Vietnam Pro** tải từ Google Fonts; offline tự fallback Arial.

---

## 2. Có gì trong prototype (phân hệ Công việc)

Đủ 3 trạng thái thao tác: màn **danh sách công việc** (data table), **drawer Tạo mới / Giao việc** (2 tab), **drawer Chi tiết / Xử lý** (4 tab). Tương tác mock đầy đủ (lưu nháp, giao việc, xử lý, checklist, tiến độ, bình luận @mention, nộp/duyệt/trả về, gia hạn, chuyển người xử lý, tạm dừng/tiếp tục, hủy, xóa) — mọi thao tác ghi **Nhật ký** và bắn toast.

## 3. Phân quyền (điểm nhấn)

Đổi vai trò ở **góc tài khoản phải trên** (5 vai trò mock). Quy tắc: mặc định **“Chỉ người liên quan”** — giáo viên không được phân công không thấy việc riêng tư của người khác; nút thao tác ẩn/disable theo quyền.

## 4. Dữ liệu mock

12 công việc CV-2026-XXXX phủ đủ trạng thái (Nháp → Hoàn thành/Quá hạn/Tạm dừng), hạn sinh tương đối theo thời điểm mở.

## 5. Design system

Token `--ks-*` khai báo ở `:root`; layout shell (rail/menu/topbar); icon Lucide inline. Các module mở rộng **dùng lại** design system này (`.btn*`, `.chip*`, `.menu`, `.modal-*`, `.ks-table`, `.drawer`…).

## 6. Ngoài phạm vi (mở rộng sau MVP)

Báo cáo/KPI công việc, template & việc lặp, Gantt, push mobile/email/SMS, giao việc liên trường — chưa dựng. Kho: **chuyển kho** (điều chuyển giữa 2 kho) tạm chưa làm theo yêu cầu.

## 7. Dự án công việc (project boards kiểu Trello — additive)

Cạnh tiêu đề **“Công việc”** có **bộ chọn dự án** (tên dự án hiện tại + số công việc + mũi tên). Bấm mở **popover** liệt kê dự án + mục **“Tất cả dự án”** + **“Tạo dự án mới”**.

- **Mỗi dự án là một bảng riêng:** chọn dự án → chỉ hiện công việc thuộc dự án đó; **“Tất cả dự án”** xem toàn bộ (giữ hành vi cũ). Tìm kiếm/tab trạng thái/bộ lọc/phân trang chạy **trong phạm vi** dự án đang chọn.
- **Form Tạo/Chỉnh sửa dự án** (bản mới nhất): **Mã dự án** (không bắt buộc) · **Tên dự án** · **Nhãn sử dụng trong công việc** (tag chip) · **Trạng thái** (tag chip). **Không còn** chọn màu/biểu tượng và không còn dòng mô tả.
- **Danh sách dự án không hiển thị icon**; **hover** mới hiện **nút bút chì** để **Chỉnh sửa**. Bấm tên dự án mở board của dự án đó.
- **Nhãn dự án:** ở “Tất cả dự án”, mỗi công việc hiện chip tên dự án; màn Chi tiết có mục **Dự án**; form tạo/sửa công việc có trường **Dự án** (mặc định theo dự án đang chọn).

**Kỹ thuật:** `PROJECTS` (id/code/name/labels/statuses), `state.projectId`, nhánh lọc trong `applyAll()`, cụm hàm `renderProjectSwitch / openProjectMenu / openCreateProject / openEditProject`. Playwright **29/29 PASS** + hồi quy KPI.

## 8. Quản lý tài liệu (Document manager — additive)

Menu riêng **“Quản lý tài liệu”** (icon thư mục) ở sidebar; render vào `#docRoot` qua `showDocs()`, expose `window.DOCS`. Một menu phẳng duy nhất (gộp “của tôi” + “được chia sẻ”) với 3 tab **Tài liệu của tôi / Được chia sẻ / Toàn trường**.

- **Giáo viên tự tạo thư mục & tải tệp:** **Thư mục mới**, **Tải lên** (kéo–thả), breadcrumb, lưới/danh sách, tìm kiếm không dấu, menu **“…”** (Mở/Tải xuống · Chia sẻ · Đổi tên · Di chuyển · Xóa) và **xem trước** tài liệu ở chế độ view.
- **Bảo mật cô lập theo TRƯỜNG:** mỗi node có `school`; mọi truy vấn lọc `school===DOC_SCHOOL` **trước tiên** → tài liệu trường khác **không bao giờ** hiển thị, kể cả khi bị chia sẻ nhầm.
- **Riêng tư theo giáo viên:** mặc định chỉ chủ sở hữu thấy; người khác **không thấy trừ khi được chia sẻ**. Dialog chia sẻ 3 lựa chọn: **Chỉ mình tôi / Toàn trường / Tuỳ chọn người cụ thể** (cùng trường).
- **Tab “Toàn trường”:** **không** hiện số người được chia sẻ; **chỉ** tài liệu do chính mình tải lên mới có nút **“…”** để thao tác.
- **Dung lượng:** nhãn **“Tổng dung lượng / 1 TB”**.

**Kỹ thuật (additive-only):** IIFE `window.DOCS` + khối CSS `.doc-*` + `#docRoot` + `showDocs()`. File HMI giữ nguyên byte (sha256 `664fbb8e…`). Playwright **38/38 PASS**.

## 9. Quản lý kho — Xuất · Nhập · Tồn, **nhiều kho** (additive, cập nhật 14/10/2026)

Menu riêng **“Quản lý kho”** (icon kho hàng) ở sidebar; render vào `#invRoot` qua `showInv()`, expose `window.INV`. Thiết kế **dễ dùng, dễ mở rộng, tối ưu cho cả desktop lẫn mobile**.

- **Chọn kho kiểu “bộ chọn dự án”:** ngay **cạnh tiêu đề “Quản lý kho”** có **nút bộ chọn kho** (icon + tên kho hiện tại + số mặt hàng + mũi tên). Bấm **sổ xuống popover** liệt kê **tất cả kho** + mục **“Tất cả kho”** (gộp) + **“Thêm kho mới”**; **hover** từng kho hiện **nút bút chì** để Sửa/Xóa (giống bộ chọn dự án bên Công việc).
- **Mỗi kho có danh mục riêng:** mỗi kho giữ **danh sách mặt hàng và tồn riêng biệt**; chọn kho để chỉ xem/thao tác trong kho đó. Dữ liệu mẫu: *Kho Văn phòng*, *Kho Thiết bị & CSVC*, *Kho Bán trú*.
- **“Tất cả kho”:** gộp xem toàn bộ mặt hàng của mọi kho, mỗi dòng có **badge tên kho**; KPI và lịch sử cộng gộp toàn bộ.
- **Quản lý danh sách kho:** trong popover — **Thêm kho mới**; bút chì → form **Đổi tên** (mã + tên) kèm nút **Xóa kho** (chặn xóa khi kho còn mặt hàng).
- **Tổng quan nhanh:** 4 thẻ KPI theo kho đang chọn — **Tổng mặt hàng / Sắp hết / Hết hàng / Tổng số lượng tồn**. Bấm *Sắp hết* / *Hết hàng* để lọc nhanh.
- **2 tab:** **Tồn kho** và **Lịch sử xuất–nhập** (phiếu PN-####/PX-#### có ngày, đối tác, người lập, ghi chú — lọc theo kho đang chọn).
- **Trạng thái tồn trực quan:** chip **Còn hàng** (tồn > định mức) · **Sắp hết** (0 < tồn ≤ định mức) · **Hết hàng** (tồn = 0).
- **Thao tác nhanh trên từng mặt hàng:** nút **Nhập** / **Xuất** mở modal có **bộ đếm số lượng cỡ lớn** (− / +), chọn nguồn/đối tác, ghi chú. Menu **“…”**: Nhập/Xuất · Sửa · Xem lịch sử · Xóa.
- **Phiếu nhiều dòng:** **+ Nhập kho** / **− Xuất kho** lập phiếu gồm nhiều mặt hàng; ở chế độ “Tất cả kho” có thêm ô **chọn kho**, danh sách mặt hàng lọc theo kho đã chọn.
- **+ Mặt hàng:** thêm/sửa mặt hàng (mã, **kho**, nhóm, ĐVT, tồn đầu, định mức); chặn trùng mã.
- **Kiểm soát tồn:** xuất quá tồn **bị chặn** kèm cảnh báo; mọi phiếu cập nhật tồn tức thì và ghi lịch sử (có gắn kho).
- **Phân quyền:** mọi thao tác nhập/xuất/thêm/sửa/xóa và quản lý kho gated theo `canCreate(CUR())`; vai trò không đủ quyền chỉ xem.
- **Full-width web & mobile:** màn kho co giãn hết chiều rộng; mobile KPI 2 cột, thẻ mặt hàng xếp dọc, nút full-width, bộ chọn kho full-width, modal dễ chạm.

**Kỹ thuật (additive-only):** IIFE `window.INV` — `WHS` (danh sách kho) + `ITEMS` (có `wh`) + `TXNS` (có `wh`) + `is` state (`is.wh`), các hàm `scopeItems/scopeTxns/whSwitchHtml/openWhMenu/whForm/delWh`, `applyTxn(...,whId)` gắn kho vào phiếu. Bộ chọn kho **dùng lại design system của bộ chọn dự án** (`.proj-switch/.proj-btn/.proj-pop/.proj-row/.proj-opt/.proj-edit/.proj-new`). Khối CSS `.inv-*` (mobile-first) + `#invRoot` + `showInv()` + nhóm menu phẳng + icon mới. **Lớp responsive shell** (nút ☰ `#navHam`, scrim `#navScrim`, media `≤860px` biến rail/sidemenu thành drawer) — thuần CSS + 1 IIFE nhỏ, desktop không đổi. File HMI giữ nguyên byte (sha256 `664fbb8e…`). Playwright **kho 56/56 PASS** (bộ chọn kho popover: chuyển/thêm/sửa/xóa + guard, badge “Tất cả kho”, KPI & lịch sử theo kho, phiếu chọn kho, thêm mặt hàng theo kho, chặn vượt tồn, full-width, mobile drawer & không tràn ngang) + hồi quy **Dự án 29/29**, **Tài liệu 38/38**.