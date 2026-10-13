# KSmart iEdu — Module “Chỉ tiêu” (KPI nhà trường)

Bổ sung **độc lập, additive-only** vào `index.html`. Không xóa/thay thế bất kỳ nội dung/chức năng/page/menu nào đã có — Công việc, Sơ đồ điều khiển thiết bị điện (HMI) và Trung tâm Ứng dụng (App Launcher) giữ nguyên. Module dùng lại **toàn bộ design system hiện có** (`--ks-*`, `.btn*`, `.chip*`, `.table-card`, `.ks-table`, `.drawer`, `.modal-*`, overlay/toast/drawer helpers), không tạo design system mới.

Bối cảnh demo: **Trường THCS Nguyễn Du · Năm học 2026-2027 · Kỳ báo cáo Tháng 9/2026** (hôm nay 18/09/2026).

> **Cập nhật:** Module “Chỉ tiêu” nay **nhập tay hoàn toàn — không lấy dữ liệu từ CSDL**. Mọi chỉ tiêu demo trước đây ở nguồn DATABASE/FORMULA đã được chuyển sang **MANUAL**. Nhóm sidebar trùng lặp **“Chỉ tiêu điều hành”** đã được **bỏ khỏi sidebar**. **Màn Tạo mới và màn Chi tiết là bảng tính (Excel-like) có công thức** (xem mục riêng bên dưới); **danh sách vẫn là bảng thường** (không có chế độ bảng tính trên danh sách).

## Điều hướng (Sidebar)
Nhóm **“Chỉ tiêu”** (icon target) gồm **2 mục con**: `Tổng quan` · **`Quản lý chỉ tiêu`**. *(Đã bỏ menu **“Loại chỉ tiêu”** và **“Báo cáo”** khỏi sidebar — trang Loại/Báo cáo còn trong mã nhưng không hiện trên menu.)*
Ba menu cũ (Danh sách / Kế hoạch / Kết quả) đã **gộp làm một** trong *Quản lý chỉ tiêu*, chia theo **trạng thái** bằng tab. Chuyển view qua `showKpi(page)` (cùng cơ chế `currentView`); vào Chỉ tiêu → ẩn `.content-inner` và `#electricalFrame`, hiện `#kpiRoot`; rời đi → khôi phục nguyên trạng. (Các giá trị cũ `catalog/plan/result` vẫn map về `manage` để không vỡ liên kết.)

> **Đồng bộ với các trường của Form Tạo mới** (Mã · Tên · Người phụ trách · Ghi chú · Đơn vị · Loại · Kế hoạch · Thực hiện · % · Thời hạn): đã **bỏ trường “Nhóm”** khỏi Tổng quan/Danh sách/Chi tiết/Báo cáo, và bổ sung **Loại · Ghi chú · Người phụ trách · Thời hạn** cho nhất quán.

## 4 trang
- **Tổng quan** — 5 thẻ KPI (Tổng chỉ tiêu, Đạt, Có nguy cơ, Chưa đạt, Chưa có dữ liệu; bấm để lọc nhanh sang Quản lý), khối **Việc cần làm**, **Tiến độ theo người phụ trách** (thay cho “theo nhóm”; **nhãn là TÊN người phụ trách**, đồng nhất với dropdown Người phụ trách — dữ liệu demo đã map vai trò/đơn vị → tên người qua `OWNER_MAP`), bảng **Chỉ tiêu nổi bật** — **bộ cột đồng nhất với màn Quản lý chỉ tiêu**: Mã · Tên chỉ tiêu · Loại · % hoàn thành · Trạng thái · Deadline · Cập nhật lần cuối · Người phụ trách · Ghi chú.
- **Quản lý chỉ tiêu** *(màn hình chính, gộp 3 menu cũ)* —
  - **Tab trạng thái**: Tất cả · Đạt · Có nguy cơ · Chưa đạt · Chưa có dữ liệu · Chưa đến hạn · Không áp dụng (kèm số đếm).
  - **Filter**: tìm kiếm không dấu (mã/tên/người phụ trách/ghi chú) + **Loại** (thay cho Nhóm/Nguồn).
  - **Bảng**: Mã · **Tên chỉ tiêu** (từ ô “Tên chỉ tiêu” khi tạo mới) · **Loại** · % hoàn thành · Trạng thái · **Deadline** · Cập nhật lần cuối · Người phụ trách · **Ghi chú**. *(Đã bỏ các cột “Đơn vị”, “Kế hoạch”, “Thực hiện”; “Chỉ tiêu” đổi thành “Tên chỉ tiêu”, “Thời hạn” đổi thành “Deadline”.)* Pill cột **Loại** hiển thị **trên một hàng** (`white-space:nowrap`, cột nới rộng); bảng có cuộn ngang khi cần nên không bị xuống dòng.
  - Nút **“+ Tạo mới chỉ tiêu”** (cán bộ quản lý) và **“Xuất báo cáo”**.
- **Chi tiết chỉ tiêu** *(bố cục giống hệt màn Tạo mới, mặc định chế độ XEM)* — **không có dòng mô tả phụ đề** và **không có dải kết quả** (đã bỏ theo yêu cầu); chỉ gồm **khối thông tin** giống Tạo mới (Mã · Tên chỉ tiêu · Người phụ trách · Deadline · Ghi chú — chế độ xem là input khóa mờ) → **bảng tính thành phần** → **Lịch sử cập nhật**. **Bảng Excel bê nguyên bộ cột của màn Tạo mới**: *Chỉ tiêu · Đơn vị · Loại · Kế hoạch · Thực hiện · % hoàn thành · [cột tính] · Trạng thái*; **dòng dưới cùng là chính chỉ tiêu** (cột Chỉ tiêu = **tên chỉ tiêu**, kèm Đơn vị + Loại, không còn “Tổng cộng”; ở chế độ Sửa có icon xóa chỉ tiêu). Thực hiện là cột **E** nên công thức tổng dùng `=SUM(E1:E…)`/`=AVERAGE(E1:E…)`. Nút **“Chỉnh sửa”** (chỉ hiện với người có quyền) mở sang **giao diện giống hệt màn Tạo mới** (chỉ khác là đã có sẵn dữ liệu): mở khóa khối thông tin + bảng tính, hàng thanh công thức có **Thêm cột tính · Thêm dòng**, kèm **footer nổi Hủy / Lưu** (`.kpi-cx-foot`). **Sửa trên bản sao tạm** — bấm **Lưu** mới ghi lại (cập nhật kết quả + nhật ký nếu Thực hiện đổi); bấm **Hủy** hoàn tác. Chỉ tiêu **checklist / Có‑Không / Mốc / Tham chiếu** vẫn dùng trang Chi tiết cũ (không phải bảng tính).
- **Báo cáo** — bảng: Mã · Chỉ tiêu (kèm Ghi chú) · ĐVT · **Loại** · Kế hoạch · Thực hiện · % HT · Đánh giá · **Thời hạn** · **Người phụ trách**; nút **Xuất Excel/PDF** (mock).
- **Loại chỉ tiêu** — quản lý danh mục loại + cấu hình điều kiện (đã bỏ khỏi sidebar; vẫn cấp danh sách “Loại” cho Form Tạo mới).

## Thời hạn (deadline) cho chỉ tiêu
Mỗi chỉ tiêu có trường **`deadline`** (dd/mm/yyyy). Hiển thị dạng **chip** theo mốc hôm nay (18/09/2026):
- **Quá hạn N ngày** (đỏ) khi đã trễ và chưa đạt · **Còn N ngày** (vàng) khi còn ≤ 14 ngày · ngày hạn (xanh) khi còn nhiều · ngày hạn (xanh lá) khi đã Đạt.
- Cột **“Deadline”** trong bảng *Quản lý chỉ tiêu* (mobile: một dòng trong card) và mục **Thời hạn** trong Chi tiết.
- **Tổng quan → Việc cần làm** ưu tiên theo hạn: *Quá hạn* → *Chưa có dữ liệu* → *Chưa đạt* → *Sắp đến hạn* → *Có nguy cơ*.
- **Tạo mới chỉ tiêu** có ô **Deadline** (lịch chọn ngày, nằm cùng hàng với *Người phụ trách* ở header); áp dụng cho mọi dòng của đợt nhập, lưu theo dd/mm/yyyy và ghi vào nhật ký khởi tạo.

## Quản lý loại chỉ tiêu (thêm / sửa / xóa + cấu hình đa dạng)
Trang **“Loại chỉ tiêu”** liệt kê danh mục loại (`TYPES`): Mã · Tên loại (kèm chiều hướng, kỳ) · Cách đánh giá · **Kiểu dữ liệu** · **Điều kiện đạt** · Số chỉ tiêu đang dùng · Thao tác.

**Mỗi loại cấu hình đa dạng** (modal Thêm/Sửa):
- **Cách đánh giá** (comparator): MIN, MAX, EXACT, RANGE, **BAND — thang điều kiện nhiều mức**, BOOLEAN, MILESTONE, CHECKLIST, REFERENCE.
- **Kiểu dữ liệu**: Số nguyên / Số thập phân / Phần trăm / Có–Không / Mốc / Checklist / Văn bản.
- **Đơn vị mặc định**, **Nguồn dữ liệu mặc định** (Nhập tay / CSDL / Công thức).
- **Chiều hướng** (Cao hơn tốt / Thấp hơn tốt / Đúng mục tiêu / —), **Số chữ số thập phân** (0–3), **Kỳ đánh giá** (Tháng/Quý/Học kỳ/Năm/Không định kỳ).
- **Điều kiện đạt**:
  - comparator dạng số chuẩn (MIN/MAX/EXACT/RANGE) → **1 ngưỡng cảnh báo “Có nguy cơ” (%)**.
  - **BAND** → **thang nhiều mức**: mỗi mức = phép so sánh (≥, ≤, >, <, =, trong khoảng, còn lại) + giá trị + → trạng thái. Xét lần lượt từ trên xuống, so **ngưỡng tuyệt đối** với giá trị thực hiện. VD: ≥98 → Đạt · 95–98 → Có nguy cơ · còn lại → Chưa đạt.

**Loại mẫu sẵn (đa dạng, 11 loại)**: Đạt tối thiểu (≥), Không vượt (≤), Đúng chỉ tiêu (=), Trong khoảng, **Tỷ lệ % theo thang** (BAND), **Điểm trung bình**, **Số lượng tuyệt đối**, Có/Không, Mốc hoàn thành, Checklist, Tham chiếu. Chỉ tiêu demo **CT005 “Tỷ lệ hài lòng của phụ huynh”** dùng loại BAND (96% → Có nguy cơ) để minh hoạ.

- **Xóa** — hộp **xác nhận nội bộ**; chặn xóa loại **hệ thống** và loại **đang được sử dụng**.
- Loại **hệ thống** (builtin) không xóa/không đổi cách đánh giá (vẫn chỉnh được tên, kiểu dữ liệu, đơn vị, ngưỡng, **thang điều kiện**, mô tả); loại **tùy chỉnh** (mã `TPxx`) sửa/xóa tự do.
- Trong **bảng tính Tạo mới**, cột **Loại** lấy từ các loại đánh giá số (MIN/MAX/EXACT, gồm cả loại tùy chỉnh vừa thêm) — thêm loại mới là dùng được ngay. **Dropdown Loại ở màn Tạo mới đã bỏ “Điểm trung bình” (SCORE) và “Số lượng tuyệt đối” (COUNT)** — chỉ còn 3 loại nền (≥ / ≤ / =) và các loại tùy chỉnh MIN/MAX/EXACT.
- `evaluate()` đọc `comparator` + (`riskBand` | `bands`) của loại; per-indicator `riskBand` vẫn ghi đè nếu có. Số thập phân/đơn vị hiển thị ưu tiên theo dữ liệu của từng chỉ tiêu.
- Chỉ **cán bộ quản lý** thao tác; giáo viên xem chỉ đọc.

## Bản thân màn **Tạo mới** và **Chi tiết** là bảng tính (Excel-like) + công thức
Theo yêu cầu, **màn Tạo mới** và **màn Chi tiết** *chính chúng* là bảng tính — **không** biến danh sách thành bảng tính (danh sách giữ nguyên dạng bảng thường). Cả hai dùng chung một **engine công thức an toàn** (parser + evaluator tự viết, **không `eval()`, không SQL**): tham chiếu ô **A1** (`E3`), **dải** (`C1:C6`), **tên cột cùng dòng** (`[Thực hiện]`); hàm **SUM, AVERAGE/AVG, MIN, MAX, COUNT, ROUND (+UP/DOWN), IF, AND, OR, IFERROR**; toán tử `+ - * / ^ %`, so sánh `= <> < > <= >=`; ngăn tham số bằng `;`/`,`, thập phân dấu chấm; phát hiện **vòng lặp** (`#CIRC!`) và lỗi `#DIV/0!`/`#REF!`/`#NAME?`/`#VALUE!`.

### Màn Tạo mới = NHIỀU **chỉ tiêu to**, mỗi cái chứa các **chỉ tiêu nhỏ** (cây cha–con trong bảng)

> **Cập nhật (turn 13) — mô hình cây cha–con ngay trong bảng.** Theo bảng chỉ tiêu thực tế của trường (các mục I, II… là chỉ tiêu to, các dòng số là chỉ tiêu nhỏ), màn Tạo mới nay cho phép **nhiều “Chỉ tiêu to” trong CÙNG một bảng**, mỗi chỉ tiêu to chứa các **“Chỉ tiêu nhỏ”** thuộc về nó:
> - Mỗi dòng có **cấp**: **Chỉ tiêu to** (`row.level='to'` — dòng nhóm, tô đậm/tô nền xanh, có badge “CHỈ TIÊU TO”) hoặc **Chỉ tiêu nhỏ** (`level='nho'` — thụt vào, có ký hiệu └).
> - Nút **“+ Chỉ tiêu to”** và **“+ Chỉ tiêu nhỏ”** (thay cho “Thêm dòng”). Chỉ tiêu nhỏ thuộc về **chỉ tiêu to gần nhất phía trên**.
> - **Chỉ tiêu to = tổng hợp SUM** các chỉ tiêu nhỏ của nó: KH/TH/%/Trạng thái là ô **read-only** (`.xsumcell`), tính từ các con, cập nhật **live**. Chỉ tiêu to **không nhập số/công thức trực tiếp**.
> - Mỗi chỉ tiêu nhỏ có **Đơn vị phụ trách** riêng; ô số nhận công thức Excel như cũ (cột D/E giữ vị trí).
> - **Khi Lưu**: mỗi **chỉ tiêu to** → **một chỉ tiêu** trong danh sách (các chỉ tiêu nhỏ là `sheet.rows` của nó, tổng `=SUM(D…)`/`=SUM(E…)`). Tạo được **nhiều chỉ tiêu một lần**. Khối thông tin phía trên chỉ còn **Người phụ trách / Deadline / Ghi chú (mặc định, áp dụng chung)** — đã bỏ ô “Mã” và “Tên chỉ tiêu” (mã tự sinh; tên lấy từ dòng chỉ tiêu to). Màn **Chi tiết/Chỉnh sửa** giữ nguyên (mỗi chỉ tiêu = 1 chỉ tiêu to + các con).

#### (Lịch sử) Mô hình cũ: một chỉ tiêu to ở header + các chỉ tiêu nhỏ là dòng bảng
Nút **“+ Tạo mới chỉ tiêu”** mở màn Tạo mới gồm **khối thông tin chỉ tiêu phía trên** và **bảng tính** bên dưới. Mô hình: **một “Tên chỉ tiêu” (dòng header — KHÔNG áp dụng công thức)** + **các chỉ tiêu nhỏ đi kèm** là các dòng của bảng. **Điền 1 hay nhiều dòng đều thuộc CÙNG một chỉ tiêu**; các dòng là **chỉ tiêu nhỏ (thành phần chi tiết)**, KHÔNG tách thành nhiều chỉ tiêu riêng.
- **Header (định nghĩa chỉ tiêu)**: hàng 1 **Mã chỉ tiêu · Tên chỉ tiêu**; hàng 2 **Người phụ trách** (dropdown) **· Deadline**; hàng 3 **Ghi chú**. Đây là chỉ tiêu “to”, **không mang công thức**.
- **Dòng cha ngay đầu bảng tính**: một **banner “CHỈ TIÊU: &lt;Tên chỉ tiêu&gt;”** (đồng bộ **live** từ ô *Tên chỉ tiêu* ở khối trên, id `#kpiCxParentName`), **KHÔNG có ô công thức**, nằm trên cùng của `<tbody>` (class `tr.xparentrow`) — bên dưới là **các chỉ tiêu nhỏ**. Mục đích: nhìn thấy rõ cấu trúc *một **chỉ tiêu cha** (không công thức) → nhiều **chỉ tiêu nhỏ***. Khi chưa đặt tên, dòng cha hiện gợi ý “Chưa đặt tên — nhập ở ‘Tên chỉ tiêu’ phía trên”.
- **Thanh hành động cố định dưới cùng**: **Hủy / Lưu tất cả** trong **footer nổi luôn nằm cuối trang** (`.kpi-cx-foot` sticky + `#kpiRoot.kpi-root-fill`).
- **Thanh công thức (fx)**: hiển thị **địa chỉ ô** đang chọn + **công thức đang áp dụng cho ô đó**, sửa trực tiếp và bấm **Enter** để áp dụng. **Trỏ/bấm vào ô nào có công thức thì công thức đó hiện lên** ở thanh fx và ở **tooltip** (thuộc tính `title`) của ô; ô có công thức được đánh dấu `.xhasf`. Có nút **“Công thức” (dropdown)** liệt kê các hàm mẫu (SUM, AVERAGE, MIN, MAX, COUNT, ROUND, %, IF, IFERROR) kèm **chú thích** — chọn để chèn nhanh vào ô. Cạnh đó là nút **Thêm cột tính · Thêm dòng** (như Excel).
- **Bảng tính (chỉ tiêu nhỏ)**: cột **Chỉ tiêu · Đơn vị · Loại · Kế hoạch · Thực hiện · % hoàn thành · [cột tính thêm được] · Đơn vị phụ trách · Trạng thái**. Ô số nhận **công thức** (VD `=D1*0.95`, tham chiếu chéo dòng `=E1+10`, `=SUM(D1:D6)`); **% hoàn thành** trống thì tự tính; cột **Trạng thái** xem trước từng chỉ tiêu nhỏ.
- **Đơn vị phụ trách theo từng chỉ tiêu nhỏ**: mỗi dòng có cột **“Đơn vị phụ trách”** (dropdown người/đơn vị, mặc định theo Người phụ trách của chỉ tiêu to) — mỗi chỉ tiêu nhỏ có thể giao cho đơn vị khác nhau (VD Phòng Kinh tế, BHXH cơ sở…). Cột nằm **sau các cột số, trước Trạng thái** nên **không** ảnh hưởng tham chiếu `D`/`E` của công thức tổng.
- **Dòng cha (banner) tổng hợp SUM**: banner đầu bảng hiển thị **Tổng Kế hoạch / Tổng Thực hiện / % / Trạng thái** = **SUM** các chỉ tiêu nhỏ, cập nhật **live** khi nhập. Chỉ tiêu to **không nhập công thức trực tiếp**.
- **Lưu** → tạo **đúng MỘT chỉ tiêu** (nguồn **MANUAL**) từ header (Mã/Tên/Người phụ trách/Deadline/Ghi chú), với các dòng đã điền làm **chỉ tiêu nhỏ (thành phần chi tiết)** của chỉ tiêu đó; **Kế hoạch/Thực hiện của chỉ tiêu = tổng hợp (`=SUM`) các chỉ tiêu nhỏ**; Đơn vị/Loại lấy theo dòng đầu. Tên chỉ tiêu bắt buộc (nếu bỏ trống header, lấy tên dòng đầu).

### Màn Chi tiết = giống Tạo mới, chế độ XEM + nút Chỉnh sửa
Bấm một chỉ tiêu (loại số: MIN/MAX/EXACT/RANGE/BAND) → chi tiết mở **chế độ XEM** (tiêu đề *Chi tiết: …*), bố cục **giống màn Tạo mới**: **khối thông tin** (Mã/Tên/Người phụ trách/Deadline/Ghi chú) → **bảng Excel** → **Lịch sử**. **Đã bỏ dòng phụ đề** và **dải kết quả** khỏi màn này. Có **“← Quay lại”** và (với người có quyền) nút **“Chỉnh sửa”**.
- **Chế độ XEM**: khối thông tin là input **khóa mờ** (disabled), bảng **chỉ đọc**, **vẫn có Lịch sử cập nhật**. Không có quyền → không hiện nút Chỉnh sửa, kèm ghi chú khóa.
- **Lịch sử cập nhật ghi lại thao tác xử lý chỉ tiêu**: mỗi mục có **nhãn thao tác** (Tạo / Sửa) + mô tả. **Tạo** ghi khi tạo mới; **Sửa** ghi mỗi lần bấm Lưu, liệt kê trường đã đổi (tên, người phụ trách, đơn vị, loại, deadline, ghi chú, thực hiện). *(Xóa: chỉ tiêu bị gỡ khỏi danh sách nên không còn lịch sử riêng để hiển thị.)*
- **Chế độ Chỉnh sửa** (bấm “Chỉnh sửa”) — **giao diện giống hệt màn Tạo mới**, chỉ khác là các trường đã có sẵn dữ liệu; **tiêu đề đổi thành *Chỉnh sửa: …***. Mở khóa khối thông tin **và** bảng; **không có nút Quay lại**; hàng thanh công thức chứa **fx · Thêm cột tính · Thêm dòng**; **bỏ dòng chú thích Tổng cộng và bỏ Lịch sử**; footer **Hủy / Lưu luôn nằm cuối trang** (`.kpi-cx-foot` sticky + `#kpiRoot.kpi-root-fill` đẩy footer xuống đáy vùng nội dung kể cả khi nội dung ngắn). Mã chỉ tiêu luôn khóa.
- **Bảng Excel bê nguyên bộ cột màn Tạo mới**: *Chỉ tiêu · Đơn vị · Loại · Kế hoạch · Thực hiện · % hoàn thành · [cột tính] · **Đơn vị phụ trách** · Trạng thái*. **Đồng nhất với Tạo mới**: **dòng cha (banner) nằm ĐẦU bảng** — hiển thị **tên chỉ tiêu to** + **Tổng KH/TH/% + Trạng thái** (SUM các chỉ tiêu nhỏ), read-only; **đã bỏ dòng “Tổng cộng” ở cuối bảng**. **Đơn vị** và **Loại đánh giá** của chỉ tiêu to chuyển lên **khối thông tin** (sửa được khi Chỉnh sửa: `dxUnit`/`dxEval`). Mỗi chỉ tiêu nhỏ có cột **Đơn vị phụ trách** riêng. **Xóa chỉ tiêu** chuyển thành nút ở **footer** (trái) khi Chỉnh sửa. Tổng hợp: **Kế hoạch = SUM(D…)**, **Thực hiện = SUM(E…)** (cột D/E giữ nguyên vị trí dù thêm cột phụ trách).
- **Lưu / Hủy có hoàn tác**: chỉnh sửa diễn ra trên **bản sao tạm** (`EDIT_IND`); **Lưu** mới ghi vào chỉ tiêu thật (cập nhật kết quả + **ghi nhật ký** nếu Thực hiện đổi), **Hủy** bỏ toàn bộ thay đổi và quay về chế độ xem.
- **Phân quyền**: nút Chỉnh sửa & lưu chỉ dành cho người có quyền (superadmin/BGH/tổ trưởng, hoặc giáo viên được phân công); người khác chỉ xem.

> **Loại chỉ tiêu**: menu “Loại chỉ tiêu” đã **bỏ khỏi sidebar**; danh mục loại vẫn dùng để cấp danh sách “Loại” cho cột Loại ở Tạo mới. Chỉ tiêu **checklist / Có-Không / Mốc / Tham chiếu** vẫn dùng **trang Chi tiết cũ** — giữ nguyên, additive.

> Toàn bộ chỉ tiêu đều là **MANUAL** — mọi chỉ tiêu (kể cả các chỉ tiêu demo trước đây đồng bộ CSDL/công thức) đều **nhập tay/cập nhật thủ công** theo quyền; không còn nhãn “Tự động cập nhật từ CSDL/công thức”.

## Đánh giá theo nguồn & loại (metadata-driven)
Loại đánh giá (BR-09): **MIN, MAX, EXACT, RANGE, BOOLEAN, MILESTONE, REFERENCE** và **CHECKLIST** (mới — đạt theo % hạng mục: đủ 100% → Đạt, một phần → Có nguy cơ, chưa bắt đầu → Chưa có dữ liệu). Logic trong `evaluate(ind)`.

## Phân quyền (mock)
Phục vụ **giáo viên & cán bộ**. Xem: mọi vai trò xem chỉ tiêu/kế hoạch/kết quả. Cập nhật MANUAL: superadmin/BGH/tổ trưởng, hoặc giáo viên **được phân công** (`assignedTeacher`); ngược lại hiện “Bạn không có quyền cập nhật…”. Xuất báo cáo: chỉ cán bộ quản lý (giáo viên thấy nút mờ + ghi chú). Dùng chung role switcher của Công việc.

## Trạng thái & loại đánh giá
Trạng thái (badge có **icon/ký hiệu**, không chỉ dùng màu): **Đạt** (success) · **Có nguy cơ** (warning) · **Chưa đạt** (danger) · **Chưa có dữ liệu** (neutral, “–”) · **Chưa đến hạn** (info) · **Không áp dụng**. Loại đánh giá (BR-09): **MIN, MAX, EXACT, RANGE, BOOLEAN, MILESTONE, REFERENCE** — logic trong `evaluate(ind)`, có ngưỡng “nguy cơ” (`riskBand`).

## Dữ liệu demo (đủ trạng thái — tất cả nguồn MANUAL)
Mọi chỉ tiêu demo đều là **MANUAL** (nhập tay). Loại đánh giá & trạng thái vẫn đa dạng:
2201 Tổng số lớp (EXACT→Đạt) · 2202 Tổng số học sinh (MIN→Đạt) · 2203 Tổng số giáo viên (MIN→Có nguy cơ) · 2204 Học sinh nữ (REFERENCE→Không áp dụng) · 2205 Học sinh DTTS (MIN→Chưa có dữ liệu) · 2206 HS/GV (MAX→Đạt) · 2207 HS/lớp (MAX→Đạt) · 2210 Tỷ lệ tốt nghiệp THCS (MIN→Chưa đạt) · CT001 Tỷ lệ chuyên cần (MIN→Có nguy cơ) · CT002 Đạt chuẩn QG mức 2 (BOOLEAN→Đạt) · CT003 Phổ cập GDTHCS đúng độ tuổi (MILESTONE→Chưa đến hạn) · CT004 Tỷ lệ bán trú (RANGE→Đạt) · CT005 Tỷ lệ hài lòng PH (BAND→Có nguy cơ).
Các chỉ tiêu trước đây đồng bộ CSDL/công thức (2201–2204, 2206, 2207, 2210, CT001, CT004, CT005) nay giữ nguyên số liệu nhưng đổi nguồn sang **nhập tay** — người phụ trách cập nhật thủ công.

## Trạng thái giao diện
Loading (skeleton khi chuyển trang) · Empty (không có kết quả lọc) · Error (try/catch + “Thử lại”) · No-Permission (chặn ở hành động cập nhật/xuất).

## Responsive
Desktop: bảng đầy đủ. Mobile (≤768px): bảng → **card** (`.kpi-card-list`), thẻ KPI 2 cột, **không tràn ngang**. Đã kiểm ở 760px (delta overflow = 0).

## Không hồi quy (đã kiểm chứng — Playwright)
- Chỉ **thêm**: 1 khối CSS (`.kpi-*`, scope `.kpi-screen`/`#kpiDrawer`), vài phần tử (`#kpiRoot`, `#kpiDrawer`, `#kpiModalWrap`), wiring điều hướng, và 1 script IIFE (`window.KPI`). Không sửa/xóa CSS/JS/DOM của Công việc/HMI/Launcher.
- File HMI `so-do-dieu-khien-thiet-bi-dien.html` **không đổi** (sha256 `664fbb8e…`).
- **Sửa lỗi:** nút **Hủy** ở popup tạo mới/cập nhật trước đây không đóng được — nguyên nhân `window.confirm` bị chặn trong iframe render (trả về false). Đã bỏ phụ thuộc `window.confirm`: Hủy/nền/ESC đóng trực tiếp; xác nhận **xóa** dùng hộp thoại nội bộ (`kpiConfirm`).
- **Tạo mới & Chi tiết dạng bảng tính + công thức** (bộ test riêng, PASS): engine tính đúng số học/`SUM`/`AVERAGE`/`MIN`/`MAX`/`ROUND`/`IF`/`AND`/`OR`/`IFERROR`, `50%`→0.5, `-2^2`→-4; tham chiếu `=A1+A2`, dải `=SUM(A1:A3)`, tên cột `[c1]`, phát hiện `#CIRC!`, báo `#DIV/0!`/`#NAME?`; **danh sách vẫn là bảng thường**; **menu “Loại chỉ tiêu” + “Báo cáo” đã bỏ khỏi sidebar** (còn 2 mục: Tổng quan, Quản lý chỉ tiêu).
- **Turn 10 (đã kiểm, 22/22 PASS)**: cột đầu bảng Tạo mới đổi **“Thành phần” → “Chỉ tiêu”**; dropdown **Loại** bỏ **“Điểm trung bình” + “Số lượng tuyệt đối”** (chỉ còn ≥ / ≤ / =); **trỏ vào ô có công thức → thanh fx hiện đúng công thức (`=100+90` ở D1) + tooltip `title` trên ô**, ô số thường không có tooltip; **Tạo mới = MỘT chỉ tiêu (Tên chỉ tiêu header, không công thức) + các chỉ tiêu nhỏ** (Lưu Tên + 2 dòng → tạo **đúng 1** chỉ tiêu gồm **2 chỉ tiêu nhỏ**, `Thực hiện = SUM(E1:E2) = 390`, giữ đúng tên các dòng, lịch sử có thao tác **Tạo**).
- **Turn 11 (đã kiểm, 26/26 PASS)**: thêm **dòng cha ở đầu bảng Tạo mới** (`tr.xparentrow`) hiển thị Tên chỉ tiêu, **không có ô công thức** (0 `.xcell`), là **dòng đầu tiên** của `<tbody>`, và **đồng bộ live** khi gõ ở ô *Tên chỉ tiêu* → giúp thấy rõ *chỉ tiêu cha → các chỉ tiêu nhỏ*. Các test turn 10 vẫn PASS (không hồi quy).
- **Turn 12 (đã kiểm, 29/29 + 26/26 PASS)** — đồng nhất mô hình **chỉ tiêu to + chỉ tiêu nhỏ** cho cả 3 màn (Tạo mới / Chi tiết / Chỉnh sửa) theo mẫu bảng thực tế của trường:
  - **Cột “Đơn vị phụ trách” cho từng chỉ tiêu nhỏ** (dropdown), lưu theo dòng (`sheet.rows[i].owner`), giữ đúng sau Lưu; đặt sau các cột số, trước Trạng thái nên **không phá tham chiếu `D`/`E`** của công thức SUM.
  - **Giữ tổng hợp SUM**: banner cha hiển thị **Tổng KH/TH/% + Trạng thái** (SUM các chỉ tiêu nhỏ), live ở Tạo mới (`#kpiCxParentTot`) và Chi tiết/Chỉnh sửa (`#kpiSumPlan/#kpiSumAct/#kpiSumPct/#kpiSumStatus`).
  - **Chi tiết/Chỉnh sửa**: bỏ **dòng “Tổng cộng” ở cuối bảng**, thay bằng **banner cha ở đầu bảng** (đồng nhất Tạo mới); **Đơn vị + Loại đánh giá** của chỉ tiêu to chuyển lên khối thông tin (`dxUnit`/`dxEval`); **Xóa chỉ tiêu** thành nút footer (`#kpiDxDelete`).
  - Sửa lỗi biểu tượng banner: `icn('barChart')` không có trong bộ ICONS (đúng tên là `barChart3`) khiến banner in ra chữ **“undefined”** → đổi sang `icn('target')` (biểu tượng bia/đích, hợp với “chỉ tiêu”).
- **Turn 18 (đã kiểm, 13/13 PASS + hồi quy 18/18 + 8/8 + 13/13)** — **cập nhật Tổng quan (và Báo cáo) khớp mô hình dữ liệu quản lý chỉ tiêu mới**, đặc biệt trạng thái **“Ngừng thực hiện” (`STOPPED`)**:
  - **Thẻ KPI Tổng quan**: thêm thẻ thứ 6 **“Ngừng thực hiện”** (icon pause, `data-status="STOPPED"`, phụ đề “Tạm ngừng theo dõi”); lưới `.kpi-kpis` đổi `repeat(5,1fr)` → `repeat(6,1fr)` (vẫn co về 3 cột ≤1100px, 2 cột ≤768px). `counts()` bổ sung khóa `STOPPED`.
  - **“Việc cần làm”**: bỏ qua chỉ tiêu đã ngừng (`if(evaluate(ind)==='STOPPED')return;`) — chỉ tiêu ngừng không còn hiện dưới dạng Quá hạn/Chưa đạt/Sắp đến hạn.
  - **“Tiến độ theo người phụ trách”**: loại chỉ tiêu ngừng khỏi mẫu số tỉ lệ (`evaluate(i)!=='NA' && evaluate(i)!=='STOPPED'`) — không kéo tụt % của đơn vị vì chỉ tiêu đã tạm ngừng.
  - **Báo cáo**: dải tóm tắt trạng thái cũng thêm thẻ **“Ngừng TH”** (6 thẻ) để tổng các thẻ khớp `Tổng` khi có chỉ tiêu ngừng.
  - Tổng chỉ tiêu vẫn đếm cả chỉ tiêu ngừng (không loại khỏi `total`). Chỉ tiêu HMI (`so-do-dieu-khien-thiet-bi-dien.html`) giữ nguyên byte (sha256 `664fbb8e…`).
- **Turn 17 (đã kiểm, 13/13 + 18/18 PASS)** — **nút Xóa/Ngừng thực hiện ở màn Chi tiết + trạng thái “Ngừng thực hiện”**:
  - Footer màn **Chi tiết** thêm **“Xóa chỉ tiêu”** (đỏ) và **“Ngừng thực hiện”** (chuyển thành **“Tiếp tục thực hiện”** khi đã ngừng) — chỉ hiện với người có quyền. Mỗi nút bật **popup xác nhận nội bộ** (`kpiConfirm`, nay nhận nhãn/biểu tượng nút tùy biến).
  - **Trạng thái mới `STOPPED` = “Ngừng thực hiện”** (icon pause) trong registry `ST`; `evaluate()` trả `STOPPED` khi `ind.stopped`; thêm vào **tab trạng thái** và **bộ lọc** ở Quản lý chỉ tiêu, và `xStatusLabel`. Ngừng/Tiếp tục có ghi **Lịch sử** (Ngừng/Tiếp tục). Chip trạng thái hiện ở tiêu đề Chi tiết và ở danh sách.
- **Turn 16 (đã kiểm, 18/18 + 8/8 PASS)** — **hợp nhất Chi tiết/Chỉnh sửa dùng chung bảng nhiều nhóm như Tạo mới**. Một **chỉ tiêu** giờ chứa **nhiều Nhóm chỉ tiêu**, mỗi nhóm gồm các **Chỉ tiêu con** — tất cả nằm trong `sheet.rows` (mỗi dòng có `level:'to'|'nho'`, tên lưu ở `label`). 3 màn dùng chung `viewCreateXls`/`createCellTd`/`bindCreateXls` theo `CX_MODE`=`create|edit|view`:
  - **Tạo mới**: `openCreateSheet` (create). **Lưu → MỘT chỉ tiêu** (mã=Mã, tên=Tên ở khối trên), `sheet.rows` giữ nguyên cây nhóm/con; giá trị tổng = SUM các dòng con (`cxOverall`).
  - **Chi tiết (view)**: `openDetail` (chỉ tiêu dạng số) nạp chỉ tiêu vào `NEWROWS/NEWHEAD/NEWX` (`cxLoadFromInd`), render read-only: tiêu đề “Chi tiết chỉ tiêu” + chip Trạng thái/Deadline, khối thông tin khóa, bảng nhiều nhóm chỉ đọc, **Lịch sử cập nhật**, nút **Quay lại / Chỉnh sửa**.
  - **Chỉnh sửa (edit)**: mở khóa toàn bộ (giống Tạo mới) — fx, Thêm cột tính, +Nhóm chỉ tiêu, +Chỉ tiêu, kéo–thả; footer **Hủy / Xóa chỉ tiêu / Lưu tất cả**; Lưu cập nhật chỉ tiêu + ghi **Lịch sử (Sửa)**; Hủy hoàn tác về xem.
  - Chỉ tiêu **checklist/Có-Không/Mốc/Tham chiếu** vẫn dùng **trang Chi tiết cũ** (không đổi). Chỉ tiêu cũ chưa có nhóm → hiển thị phẳng (mỗi dòng là 1 chỉ tiêu con); chỉ tiêu chỉ có tổng (không có dòng con) → tự tạo 1 dòng con giữ nguyên số liệu.
- **Turn 15 (đã kiểm, 8/8 PASS)** — **kéo–thả đổi vị trí bằng ô số thứ tự (STT)** ở bảng Tạo mới: ô STT là tay cầm kéo (`.rownum.xdrag`, con trỏ grab). Kéo một **Chỉ tiêu con** → di chuyển đúng dòng đó; kéo một **Nhóm chỉ tiêu** → di chuyển **cả cụm** (nhóm + tất cả con của nó) theo. Logic `createMoveRow` giữ nguyên cấu trúc nhóm (không chèn cụm vào giữa nhóm khác — snap theo ranh giới nhóm). **Sửa lỗi tiềm ẩn:** ô đang gõ dở sẽ **blur (ghi) trước khi kéo** để tránh commit nhầm vào chỉ số dòng đã đổi sau khi sắp xếp.
- **Turn 14 (đã kiểm, 24/24 PASS)** — chỉnh màn Tạo mới **khớp mockup của trường**: khôi phục khối trên **Mã chỉ tiêu\* · Tên chỉ tiêu\* · Người phụ trách chính\* · Deadline · Ghi chú**; đổi nút thành **“+ Nhóm chỉ tiêu” / “+ Chỉ tiêu”**; thêm **cột “Thời hạn” (date) cho từng Chỉ tiêu con** (dòng **Nhóm** để trống Thời hạn + Đơn vị phụ trách); tên nhóm in đậm (bỏ badge). Lưu: mỗi **Nhóm chỉ tiêu** → 1 chỉ tiêu; **nhóm đầu** dùng Mã/Tên ở khối trên (nếu nhóm chưa có tên), các nhóm sau tự sinh mã. Thời hạn/Đơn vị phụ trách lưu theo từng con và hiển thị ở **Chi tiết/Chỉnh sửa** (thêm cột “Thời hạn” vào bảng thành phần; hạn lưu dạng ISO, hiển thị dd/mm/yyyy). **Sửa lỗi:** `openCreateSheet()` (nút “+ Tạo mới chỉ tiêu” thật) trước tạo 3 dòng đều là “chỉ tiêu” (không có nhóm mặc định) → đã sửa để mặc định là **1 Nhóm chỉ tiêu + 2 Chỉ tiêu** ngay khi mở, không cần bấm “+ Nhóm chỉ tiêu”.
- **Turn 13 (đã kiểm, 22/22 PASS)** — màn Tạo mới thành **cây cha–con nhiều tầng trong bảng** (theo bảng mẫu I, II…): mỗi dòng có cấp **to/nhỏ**; nút **“+ Chỉ tiêu to” / “+ Chỉ tiêu nhỏ”**; **chỉ tiêu to = SUM các con** (ô read-only `.xsumcell`, live); **Lưu → mỗi chỉ tiêu to thành 1 chỉ tiêu** (test: 2 chỉ tiêu to → 2 chỉ tiêu, con gắn đúng cha, KH/TH tổng đúng: 2.581.632+1.722.914 = 4.304.546…); header rút gọn còn Người phụ trách/Deadline/Ghi chú mặc định (bỏ Mã/Tên); Chi tiết/Chỉnh sửa vẫn PASS (banner + cột phụ trách + không có dòng tổng cuối).
- Test tự động (Playwright, các bộ đều PASS): Công việc 10 dòng (trước & sau khi dùng KPI), HMI mở không lỗi 404; nhóm Chỉ tiêu và **nhóm “Chỉ tiêu điều hành” đã biến mất khỏi sidebar**; **Hủy đóng được popup dù có dữ liệu chưa lưu**; trang Quản lý gộp + **tab trạng thái** lọc đúng; **Loại chỉ tiêu**: 11 loại hệ thống, thêm/sửa/xóa, chặn xóa loại hệ thống; **mọi chỉ tiêu đều MANUAL**; cập nhật MANUAL (2205: 62 ≥ 60 → Đạt) + audit; **deadline** (chip Quá hạn/Còn N ngày); phân quyền giáo viên; responsive rows→cards không tràn ngang; **không lỗi console** (ngoài font Google/favicon bị chặn trong sandbox).

## Ghép vào app thật
`window.KPI` bọc mock in-memory `INDICATORS`; thay bằng API (`GET /kpi/indicators`, `GET /kpi/{code}`, `POST /kpi/{code}/result`) khi có backend. Metadata (`dataSource`, `evaluationType`, `dataType`, `plan`, `riskBand`, `assignedTeacher`) map thẳng sang schema chỉ tiêu; `evaluate()` giữ nguyên làm rule engine phía client hoặc chuyển về server.
