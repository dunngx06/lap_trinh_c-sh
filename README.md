# BÁO CÁO PHÂN TÍCH TỔNG THỂ DỰ ÁN QUẢN LÝ HỌC PHẦN (QLHP)

> **Mô tả**: Đây là báo cáo toàn diện, mổ xẻ từng trang (Page), từng lớp xử lý (Code-behind) để giúp bạn nắm được luồng hoạt động từ trên xuống dưới của dự án. 
> Báo cáo này đặc biệt hữu ích nếu bạn cần trình bày tiểu luận, bảo vệ đồ án trước giảng viên, hoặc hướng dẫn cho thành viên khác trong nhóm.

---

## 1. TỔNG QUAN KIẾN TRÚC & CÔNG NGHỆ CHUNG
- **Nền tảng framework:** ASP.NET Web Forms (chạy trên .NET Framework 4.7.2). Giúp phát triển nhanh các tính năng nhờ cơ chế Component Events (giống như lập trình WinForms truyền thống).
- **Ngôn ngữ:** C# cho xử lý Logic (Backend), HTML/CSS/Bootstrap/jQuery cho Giao Diện.
- **Cơ sở dữ liệu:** SQL Server (sử dụng thư viện ADO.NET thuần `System.Data.SqlClient` với các class cốt lõi: `SqlConnection`, `SqlCommand`, `SqlDataAdapter`, `SqlDataReader`).
- **An toàn bảo mật:** Tất cả các thao tác với CSDL đều tuân thủ việc dùng tham số hóa (Parameterized Queries - `@` variables) thay vì nối chuỗi tĩnh, do đó đã **tránh được hoàn toàn rủi ro SQL Injection**.

---

## 2. PHÂN TÍCH CẤU HÌNH VÀ GLOBAL FILES
- **`Web.config`**: Lưu chuỗi cấu hình `ConnectionStrings` duy nhất với tên `QLHP_CTH`. Việc tập trung kết nối SQL tại đây giúp dự án dễ dàng triển khai, clone sang máy của người khác mà chỉ cần sửa đúng 1 dòng (địa chỉ Server name `DUNX\SQLEXPRESS03`).
- **`Global.asax`**: File chịu trách nhiệm khởi động dự án. Thiết lập bộ gộp file (`BundleConfig`) giúp đóng gói CSS/JS lại thành một luồng để trang web load nhẹ nhàng và mượt mà hơn.
- **`Site.Master` & `Site.Mobile.Master`**: Giao diện chủ đạo (Master Page) cung cấp Thanh điều hướng (Navbar) và Footer dùng chung. Bất cứ trang con nào (`.aspx`) kế thừa giao diện từ đây đều sẽ có chung bộ Menu thống nhất.
- **`ViewSwitcher.ascx`**: Web User Control chịu trách nhiệm kiểm tra xem thiết bị người dùng đang hiển thị Web bằng Mobile hay Desktop để điều hướng Layout tự động thích ứng với màn hình.

---

## 3. PHÂN TÍCH CÁC TRANG CHỨC NĂNG CỐT LÕI (CORE PAGES)

### 3.1. Trang Tìm Kiếm & Quản Lý Tổng (`SearchCourse.aspx`)
Trang này chiếm dung lượng code lớn nhất và thực hiện đa nhiệm nhất (tìm kiếm nhiều bộ lọc, CRUD trực tiếp).
- **Mảng Hiển Thị (Read)**: Quản lý danh sách toàn bộ các môn học qua một `GridView` (`gvHocPhan`). Cung cấp sẵn cơ chế phân trang tự động khi hàm `gvHocPhan_PageIndexChanging` đè lại `PageIndex`.
- **Mảng Lọc Dữ Liệu (Search & Filter)**: Câu lệnh `WHERE` trong hàm `LoadData()` được xây dựng cực kì thông minh, kết hợp cả tìm text `LIKE` và lọc Combobox trực tiếp (Khoa/Viện, Trình Độ) qua toán tử logic kết hợp ngắn (`@khoa = '' OR Khoa_VienQL = @khoa`). Tự động tải danh sách các Khoa động từ CSDL nhờ `SELECT DISTINCT`.
- **Thêm/Sửa (Create/Update)**: Được tối ưu giao diện bằng Panel ẩn/hiện (`pnlForm`). 
  - Khởi động hàm `FillForm`, map dữ liệu ngược vào Input box, khoá tự động trường Mã Môn (`ReadOnly`), lưu ID tạm vào thẻ ngầm `hfEditID` để phân biệt là đang **Sửa** hay đang **Thêm mới**.
  - `btnSave_Click()` sẽ dựa vào xem `hfEditID` trống hay có data để phát sinh quyết định đẩy mã lệnh `INSERT` hoặc `UPDATE` tương ứng.
- **Xoá (Delete)**: Bẫy xử lý lỗi thông minh (Hàm `DeleteHocPhan`) hiểu được cấu trúc Liên kết mạnh ở Database. Nó xoá các ràng buộc con ở bảng phân công phụ (`ChuongTrinh_ChiTiet`) trước khi tiến hành chém dữ liệu cha ở bảng chính (`HocPhan`), điều này giúp tránh được lỗi khoá ngoại Foreign Key (giải quyết 1 vấn đề phổ biến nhất của sinh viên đồ án).

### 3.2. Trang Form "Đơn Nhiệm" (`CreateProgram.aspx`)
Trang chuyên biệt để hỗ trợ nhập mới một học phần độc lập, và có thể "mượn lại" để làm Form Sửa đổi.
- **Điều kiện Sửa đổi**: Hệ thống kiểm tra tham số đường dẫn `Request.QueryString["id"]`. Nếu tồn tại, trang tự khoá lại thẻ Input chứa ID (`txtID.Enabled = false`) và kích hoạt hàm nạp dữ liệu cũ (`LoadData`) lên màn hình cho người dùng chỉnh sửa.
- **Giải pháp SQL Tối ưu hoá**: Ở nút Save, thay vì dùng if-else phức tạp bằng Code C#. Code đã ủy thác toàn bộ quyết định gánh nặng cho SQL Server bằng lệnh: `IF EXISTS (SELECT 1 FROM HocPhan...) UPDATE ... ELSE INSERT ...`. Server tự chẩn đoán record mà Thêm hoặc Lưu đè, chỉ bằng 1 lần mở kết nối duy nhất.

### 3.3. Trang Quản Lý Mảng Chương Trình Đào Tạo (`ProgramList.aspx`)
Màn hình chịu trách nhiệm xử lý các Mối quan hệ N-N (nhiều nhiều) giữa `Chương_Trình` và `Học_Phần` thông qua bảng ánh xạ `ChuongTrinh_ChiTiet`.
- **Thống Kê (Aggregations)**: Ở lưới danh sách bên ngoài `Repeater` (`rptPrograms`), code áp dụng kỹ thuật gộp hàm kết nối nâng cao SQL bao gồm `LEFT JOIN` và nhóm `GROUP BY` cùng bộ đếm `COUNT(Số môn)` và rải tổng cộng `SUM(Số tín chỉ)`. Xử lý thống kê gọn gàng trên cùng 1 lần request.
- **Trải Nghiệm Modal Popup Động**: Điểm sáng giá trị nhất của trang này là khi xem danh sách môn học của 1 Chương trình cụ thể. Dữ liệu được trích xuất bằng `btnLoadDetail_Click`, đẩy xuống GridView `gvDetail` ngầm và tự bật Popup lên giữa màn hình thông qua cơ chế chèn script tĩnh `ShowModal = "true"` giao tiếp từ Server => HTML Client. Trải nghiệm người dùng không hề bị chớp nháy loạn xạ giữa 2 trang khác nhau.

### 3.4. Trang Đề Cương Chi Tiết Môn Học (`DescriptionCourses.aspx`)
Trang view chuẩn mực để sinh viên / giảng viên in ấn, tra cứu mô tả môn.
- Chỉ dùng đúng `SqlDataReader` (Công cụ bọc dữ liệu đọc cực nhanh 1 chiều nhưng không trích xuất lưu lại vào Datatable tốn RAM) để tải 1 bản ghi Học Phần duy nhất.
- Ánh xạ kết hợp với các Function format định dạng để báo cáo trông trịnh trọng hơn như: In Hoa (ToUpper) và Fix chiều dài tín chỉ chuẩn Form với thêm số 0 đầu chuỗi (`.PadLeft(2, '0')`).
- Dùng Literal (`litMoTa`) linh hoạt hỗ trợ việc render các định dạng văn bản HTML (nếu có nội dung xuống dòng/gạch chéo) tốt hơn là Control truyền thống.

---

## 4. ĐÁNH GIÁ VÀ ĐỀ XUẤT CẢI TIẾN DÀNH CHO BẠN
Nếu nhóm của bạn dự định tiếp tục trau chuốt mã nguồn để nhận điểm số cao hơn nữa, bạn có thể thực hiện những đề xuất sau đây:

1. **Chuẩn Hoá Tầng Dữ Liệu (Layered Architecture)**: 
   Code logic kết nối SQL hiện ở dạng Code-Behind lồng ghép trực tiếp với Giao Diện tạo thành mã khổng lồ (điển hình ở trang `SearchCourse.aspx`). Nếu gom các lệnh tạo/đóng kết nối `SqlConnection` thành một class dùng chung tên là `SqlHelper` (File Class `.cs` nằm ngoài ứng dụng) sẽ giúp code sạch sẽ hơn gấp 3 lần và tuân thủ mô hình Design Pattern mạnh hơn.
2. **Quản Lý Báo Cáo Lỗi Chuyên Nghiệp Hơn**: 
   Hiện tại nếu SQL gặp vấn đề kết nối, hệ thống rẽ vào nhánh `catch (Exception ex)` và phun thẳng Alert hộp thoại của Javascript chứa mã lỗi đó ra màn hình (`ex.Message`). Hộp này vừa là rủi ro rò rỉ cấu trúc DB nếu hacker thấy, lại mang rủi ro vỡ HTML layout nếu mã lỗi là ký tự không hợp lệ. Bạn nên cho thông báo ra một biến Label màu đỏ (`lblMsg.Text`) giấu đi, hoặc điều hướng chuyển thẳng tới trang báo lỗi `Error.aspx` có sẵn.
3. **Cơ chế Modal cũ**: Viết script thủ công đẩy `ShowModal="true"` xuống client vẫn ổn đối với học tập. Tương lai nên tìm hiểu đến AJAX/WebMethods hoặc sử dụng ScriptManager tích hợp sẵn của Web Forms để UpdatePanel thao tác các Popup mượt mà hơn.
