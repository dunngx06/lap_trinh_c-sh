# Báo Cáo Phân Tích Mã Nguồn: Trang Chi Tiết Học Phần (DescriptionCourses)

> **Thông tin file**
> - Giao diện Frontend: `DescriptionCourses.aspx`
> - Lớp xử lý Backend (C#): `DescriptionCourses.aspx.cs`
> - Cấu trúc: ASP.NET Web Forms

---

## 1. Mục Đích Của Trang
Trang `DescriptionCourses` được thiết kế để hiển thị **"Đề cương chi tiết học phần"** của một môn học cụ thể. Giao diện được mô phỏng theo định dạng của một văn bản hành chính/tờ trình chuẩn (bao gồm tiêu đề Quốc hiệu, thông tin cơ bản về môn học, cấu trúc thời lượng giảng dạy, danh sách giảng viên dự kiến và quy định khóa học).

---

## 2. Phân Tích Giao Diện (`DescriptionCourses.aspx`)
Trang giao diện sử dụng HTML kết hợp với các Web Control của ASP.NET và kế thừa giao diện dùng chung từ `Site.Master`.

### 2.1 Cấu Trúc CSS & Layout
- **`.khung-de-cuong`**: Định dạng khung chứa giống một tờ giấy A4 (border, box-shadow, padding) với background trắng, đặt căn giữa trang để người dùng dễ dàng theo dõi hoặc in ấn.
- **Vùng dữ liệu tĩnh**: Phần lớn văn bản như Tiêu đề trường (ĐH Kinh tế Quốc dân), cấu trúc giờ tự học, danh sách giảng viên mẫu và nội quy được hardcode (code cứng) bằng HTML thuần.

### 2.2 Các Controls Tiếp Nhận Dữ Liệu Động
Các thẻ có thuộc tính `runat="server"` đóng vai trò là "biến" trên giao diện để nhận dữ liệu từ Controller (Code-behind) đổ xuống:
- `<asp:Label ID="lblTenHocPhanTieuDe">`: Tiêu đề môn học in hoa nổi bật.
- `<asp:Label ID="lblTenVn" ...>` tới `lblKhoaQl`: Lần lượt tiếp nhận các thông tin cơ bản của môn học.
- `<asp:Literal ID="litMoTa">`: Sử dụng Literal để in xuất văn bản thô (hoặc nội dung chứa HTML) ra trình duyệt mà không bọc thêm bất kỳ thẻ `<span>` nào dư thừa như Label.
- `<asp:Button ID="btnQuayLai">`: Nút quay lại trang danh sách.

---

## 3. Phân Tích Mã Xử Lý Logic (`DescriptionCourses.aspx.cs`)
File này chịu trách nhiệm nhận tham số từ người dùng, truy vấn cơ sở dữ liệu và ánh xạ dữ liệu lên các Controls giao diện.

### 3.1 Khởi Tạo & Bắt Đầu Quy Trình
```csharp
string strConn = ConfigurationManager.ConnectionStrings["QLHP_CTH"].ConnectionString;
```
Ứng dụng sử dụng lớp `ConfigurationManager` để tự động đọc chuỗi kết nối (`ConnectionString`) từ file `Web.config`. Điều này giúp quản lý tập trung thông tin đăng nhập CSDL thay vì rải rác trong từng file.

### 3.2 Sự Kiện `Page_Load`
```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!IsPostBack)
    {
        string maHp = Request.QueryString["ID"];
        if (!string.IsNullOrEmpty(maHp))
        {
            TaiDuLieuDeCuong(maHp);
        }
    }
}
```
- Cơ chế `IsPostBack`: Đảm bảo quy trình truy vấn dữ liệu chỉ chạy **một lần duy nhất** khi trang được load mới hoàn toàn.
- `Request.QueryString["ID"]`: Trích xuất mã học phần (Ví dụ `IT101`) được truyền qua URL parameters.

### 3.3 Hàm Truy Vấn Database `TaiDuLieuDeCuong(string maHp)`
Quy trình giao tiếp với SQL Server diễn ra qua các bước chuẩn của ADO.NET:

1. **Khởi tạo và Quản lý kết nối**:
   ```csharp
   using (SqlConnection conn = new SqlConnection(strConn))
   ```
   Khối `using` tự động lo việc gọi hàm `Dispose()` để đóng kết nối tới Database ngay khi xử lý xong khối lệnh, giải quyết vấn đề rò rỉ tài nguyên mạng và RAM.

2. **Viết câu lệnh và Tạo tham số an toàn**:
   ```csharp
   string truyVan = "SELECT * FROM HocPhan WHERE IDHocPhan = @MaHP";
   SqlCommand lenh = new SqlCommand(truyVan, conn);
   lenh.Parameters.AddWithValue("@MaHP", maHp);
   ```
   Tham số `@MaHP` hoạt động như một Parameterized Query, đây là lá chắn tuyệt đối bảo vệ hệ thống khỏi tấn công chèn mã SQL (SQL Injection).

3. **Truyền tải dữ liệu vào Giao diện (DataReader)**:
   ```csharp
   SqlDataReader doc = lenh.ExecuteReader(); 
   if (doc.Read())
   {
       lblTenHocPhanTieuDe.Text = doc["TenHocPhan"].ToString().ToUpper();
       lblStc.Text = doc["SoTinChi"].ToString().PadLeft(2, '0');
       // ...
   }
   ```
   - Lớp `SqlDataReader` được dùng vì đây là phương thức cực kỳ nhẹ và nhanh (forward-only) phù hợp với việc chỉ bóc ra 1 dòng dữ liệu.
   - Khi tìm thấy bản ghi `doc.Read()`, hàm bắt đầu ánh xạ dữ liệu (Mapping).
   - Sử dụng các Function bổ trợ như `.ToUpper()` (In hoa) và `.PadLeft(2, '0')` (Chèn số 0 vào trước số tín chỉ có 1 chữ số, ví dụ `3 -> 03`) để làm mượt dữ liệu hiển thị.

4. **Xử Lý Lỗi (Error Handling)**:
   ```csharp
   catch (Exception ex)
   {
       Response.Write("<script>alert('Lỗi: " + ex.Message + "');</script>");
   }
   ```
   Mọi lỗi phát sinh (đứt mạng kết nối DB, sai cú pháp SQL...) được bắt vào khối `catch` và phản hồi lại người dùng bằng một Alert báo lỗi trực tiếp trên trình duyệt. Tuy nhiên `Response.Write` script trực tiếp ở thời điểm này là cách làm cũ, có rủi ro làm hỏng dàn trang HTML.

### 3.4 Điều Hướng Nút
```csharp
protected void btnQuayLai_Click(object sender, EventArgs e)
{
    Response.Redirect("ProgramList.aspx");
}
```
Khi người dùng bấm nút "Quay lại danh sách", hàm này điều hướng Server phản hồi lệnh chuyển trang đến `ProgramList.aspx`.
