# Bài giảng: Đóng gói (Encapsulation) trong C#

## Mục lục
1. [Giới thiệu về Đóng gói](#1-giới-thiệu-về-đóng-gói)
2. [Access Modifiers (Phạm vi truy cập)](#2-access-modifiers-phạm-vi-truy-cập)
3. [Fields và Properties](#3-fields-và-properties)
4. [Constructor (Hàm khởi tạo)](#4-constructor-hàm-khởi-tạo)
5. [Methods (Phương thức)](#5-methods-phương-thức)
6. [Ví dụ thực hành từng bước](#6-ví-dụ-thực-hành-từng-bước)
7. [Bài tập nâng cao](#7-bài-tập-nâng-cao)

---

## 1. Giới thiệu về Đóng gói

### 1.1. Đóng gói là gì?

**Đóng gói (Encapsulation)** là một trong bốn tính chất quan trọng của lập trình hướng đối tượng (OOP). Nó cho phép:

- **Gộp dữ liệu và hành vi lại với nhau**: Các thuộc tính (fields) và phương thức (methods) được đặt trong cùng một lớp (class)
- **Che giấu thông tin**: Bảo vệ dữ liệu bên trong đối tượng, không cho bên ngoài truy cập trực tiếp
- **Kiểm soát truy cập**: Quyết định phần nào được công khai, phần nào được giữ kín

### 1.2. Tại sao cần Đóng gói?

**Ví dụ thực tế**: Giống như một chiếc ATM

- Bạn chỉ có thể rút tiền qua màn hình và bàn phím (giao diện công khai)
- Bạn không thể mở máy ATM và lấy tiền trực tiếp (dữ liệu được bảo vệ)
- Máy ATM kiểm tra số dư trước khi cho rút tiền (kiểm soát logic)

**Lợi ích**:
- Bảo mật dữ liệu
- Dễ bảo trì và sửa lỗi
- Tránh sử dụng sai dữ liệu
- Code dễ hiểu và tái sử dụng

---

## 2. Access Modifiers (Phạm vi truy cập)

### 2.1. Các loại Access Modifier

| Modifier | Mô tả | Truy cập từ đâu? |
|----------|-------|------------------|
| `public` | Công khai hoàn toàn | Mọi nơi |
| `private` | Riêng tư (mặc định) | Chỉ trong class |
| `internal` | Nội bộ | Trong cùng project |
| `protected` | Bảo vệ | Trong class và class con |

### 2.2. Ví dụ minh họa

```csharp
class SinhVien
{
    // Private: chỉ dùng trong class này
    private int tuoi = 18;
    
    // Public: ai cũng truy cập được
    public string HoTen = "";
    
    // Internal: truy cập trong cùng project
    internal string MaSV = "";
}
```

**Sử dụng**:
```csharp
SinhVien sv = new SinhVien();
sv.HoTen = "Nguyễn Văn A";  // ✅ OK - public
sv.MaSV = "SV001";          // ✅ OK - internal (cùng project)
sv.tuoi = 20;               // ❌ LỖI - private không truy cập được
```

---

## 3. Fields và Properties

### 3.1. Field là gì?

**Field** là biến được khai báo trong class, thường là `private` để bảo vệ dữ liệu.

```csharp
private int _maSo;      // Field
private string _hoTen;  // Field
```

**Quy ước đặt tên**: Thường bắt đầu bằng dấu gạch dưới `_`

### 3.2. Property là gì?

**Property** là cách để truy cập vào field một cách an toàn, có thể kiểm soát việc đọc/ghi.

#### Cách 1: Property đầy đủ

```csharp
class TaiKhoan
{
    private float _soDu = 0;  // Field
    
    // Property với get/set
    public float SoDu
    {
        get { return _soDu; }      // Đọc giá trị
        set                        // Gán giá trị
        {
            if (value >= 0)        // Kiểm tra điều kiện
                _soDu = value;
            else
                Console.WriteLine("Số dư không thể âm!");
        }
    }
}
```

**Sử dụng**:
```csharp
TaiKhoan tk = new TaiKhoan();
tk.SoDu = 1000;          // Gọi set
Console.WriteLine(tk.SoDu);  // Gọi get
```

#### Cách 2: Auto-Property (Ngắn gọn)

```csharp
public string HoTen { get; set; }  // C# tự tạo field ẩn
public int Tuoi { get; private set; }  // Chỉ đọc từ bên ngoài
```

### 3.3. So sánh Method vs Property

#### Cách cũ: Dùng Methods

```csharp
class Categories
{
    private int _categoryID = 0;
    
    // Get method
    public int CategoryID_Get()
    {
        return _categoryID;
    }
    
    // Set method
    public void CategoryID_Set(int cat)
    {
        _categoryID = cat;
    }
}

// Sử dụng
Categories cat = new Categories(1);
cat.CategoryID_Set(15);           // Gọi như hàm
int id = cat.CategoryID_Get();    // Gọi như hàm
```

#### Cách mới: Dùng Properties (Khuyến nghị)

```csharp
class Categories
{
    private int _categoryID = 0;
    
    public int CategoryID
    {
        get { return _categoryID; }
        set { _categoryID = value; }
    }
}

// Sử dụng
Categories cat = new Categories(1);
cat.CategoryID = 15;              // Gán như biến
int id = cat.CategoryID;          // Đọc như biến
```

---

## 4. Constructor (Hàm khởi tạo)

### 4.1. Constructor là gì?

**Constructor** là hàm đặc biệt được gọi tự động khi tạo đối tượng, dùng để khởi tạo giá trị ban đầu.

**Đặc điểm**:
- Tên giống tên class
- Không có kiểu trả về (không có `void`)
- Có thể có nhiều constructor (overloading)

### 4.2. Các loại Constructor

#### Constructor mặc định

```csharp
class SinhVien
{
    public string HoTen;
    public int Tuoi;
    
    // Constructor không tham số
    public SinhVien()
    {
        HoTen = "Chưa có tên";
        Tuoi = 18;
        Console.WriteLine("Tạo sinh viên mới!");
    }
}

// Sử dụng
SinhVien sv = new SinhVien();  // Gọi constructor
```

#### Constructor có tham số

```csharp
class SinhVien
{
    public string HoTen;
    public int Tuoi;
    
    // Constructor có 2 tham số
    public SinhVien(string ten, int tuoi)
    {
        HoTen = ten;
        Tuoi = tuoi;
    }
}

// Sử dụng
SinhVien sv = new SinhVien("Nguyễn Văn A", 20);
```

#### Overloading Constructor (Nạp chồng)

```csharp
class TaiKhoan
{
    public int MaSo;
    public string HoTen;
    public float SoDu;
    
    // Constructor 1: Không tham số
    public TaiKhoan()
    {
        MaSo = 0;
        HoTen = "";
        SoDu = 0;
    }
    
    // Constructor 2: 1 tham số
    public TaiKhoan(int maSo)
    {
        MaSo = maSo;
        HoTen = "";
        SoDu = 0;
    }
    
    // Constructor 3: 3 tham số
    public TaiKhoan(int maSo, string hoTen, float soDu)
    {
        MaSo = maSo;
        HoTen = hoTen;
        SoDu = soDu;
    }
}

// Sử dụng
TaiKhoan tk1 = new TaiKhoan();
TaiKhoan tk2 = new TaiKhoan(123);
TaiKhoan tk3 = new TaiKhoan(456, "Nguyễn Văn B", 5000000);
```

---

## 5. Methods (Phương thức)

### 5.1. Method là gì?

**Method** là hàm trong class, định nghĩa hành vi của đối tượng.

### 5.2. Cú pháp

```csharp
[access_modifier] [return_type] TenMethod([tham_số])
{
    // Code
}
```

### 5.3. Ví dụ các loại Method

```csharp
class TaiKhoan
{
    private float _soDu = 0;
    
    // Method không trả về
    public void GuiTien(float soTien)
    {
        if (soTien > 0)
        {
            _soDu += soTien;
            Console.WriteLine($"Gửi thành công {soTien}đ");
        }
    }
    
    // Method có trả về
    public bool RutTien(float soTien)
    {
        if (soTien > 0 && soTien <= _soDu)
        {
            _soDu -= soTien;
            return true;  // Rút thành công
        }
        return false;  // Rút thất bại
    }
    
    // Method hiển thị
    public void HienThongTin()
    {
        Console.WriteLine($"Số dư hiện tại: {_soDu}đ");
    }
}
```

---

## 6. Ví dụ thực hành từng bước

### Bước 1: Tạo class Account cơ bản

```csharp
using System;

namespace BankApp
{
    class Account
    {
        // Fields (private)
        private int _maSo;
        private string _hoTen;
        private float _soDu;
        
        // Constructor
        public Account()
        {
            _maSo = 0;
            _hoTen = "";
            _soDu = 0;
        }
    }
}
```

### Bước 2: Thêm Properties

```csharp
class Account
{
    private int _maSo;
    private string _hoTen;
    private float _soDu;
    
    // Properties
    public int MaSo
    {
        get { return _maSo; }
        set { _maSo = value; }
    }
    
    public string HoTen
    {
        get { return _hoTen; }
        set { _hoTen = value; }
    }
    
    public float SoDu
    {
        get { return _soDu; }
        private set { _soDu = value; }  // Chỉ đọc từ bên ngoài
    }
}
```

### Bước 3: Thêm Methods

```csharp
class Account
{
    // ... (Fields và Properties như trên)
    
    // Method nhập dữ liệu
    public void NhapDuLieu()
    {
        Console.Write("Nhập mã số: ");
        _maSo = int.Parse(Console.ReadLine());
        
        Console.Write("Nhập họ tên: ");
        _hoTen = Console.ReadLine();
        
        Console.Write("Nhập số dư ban đầu: ");
        _soDu = float.Parse(Console.ReadLine());
    }
    
    // Method hiển thị
    public void HienDuLieu()
    {
        Console.WriteLine("=== THÔNG TIN TÀI KHOẢN ===");
        Console.WriteLine($"Mã số: {_maSo}");
        Console.WriteLine($"Họ tên: {_hoTen}");
        Console.WriteLine($"Số dư: {_soDu:N0} đ");
        Console.WriteLine("===========================");
    }
    
    // Method gửi tiền
    public void GuiTien(float soTien)
    {
        if (soTien > 0)
        {
            _soDu += soTien;
            Console.WriteLine($"Gửi thành công {soTien:N0}đ");
        }
        else
        {
            Console.WriteLine("Số tiền phải dương!");
        }
    }
    
    // Method rút tiền
    public bool RutTien(float soTien)
    {
        if (soTien <= 0)
        {
            Console.WriteLine("Số tiền phải dương!");
            return false;
        }
        
        if (soTien > _soDu)
        {
            Console.WriteLine("Số dư không đủ!");
            return false;
        }
        
        _soDu -= soTien;
        Console.WriteLine($"Rút thành công {soTien:N0}đ");
        return true;
    }
}
```

### Bước 4: Sử dụng trong Main

```csharp
class Program
{
    static void Main(string[] args)
    {
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        
        // Tạo 1 tài khoản
        Account tk1 = new Account();
        tk1.NhapDuLieu();
        tk1.HienDuLieu();
        
        // Gửi tiền
        tk1.GuiTien(500000);
        tk1.HienDuLieu();
        
        // Rút tiền
        tk1.RutTien(200000);
        tk1.HienDuLieu();
        
        Console.ReadLine();
    }
}
```

### Bước 5: Làm việc với mảng đối tượng

```csharp
class Program
{
    static void Main(string[] args)
    {
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        
        int soLuong = 3;
        Account[] dsTaiKhoan = new Account[soLuong];
        
        // Khởi tạo các đối tượng
        for (int i = 0; i < soLuong; i++)
        {
            dsTaiKhoan[i] = new Account();
        }
        
        // Nhập dữ liệu
        for (int i = 0; i < soLuong; i++)
        {
            Console.WriteLine($"\n--- Nhập tài khoản {i + 1} ---");
            dsTaiKhoan[i].NhapDuLieu();
        }
        
        // Hiển thị dữ liệu
        Console.WriteLine("\n\n=== DANH SÁCH TÀI KHOẢN ===");
        for (int i = 0; i < soLuong; i++)
        {
            Console.WriteLine($"\nTài khoản {i + 1}:");
            dsTaiKhoan[i].HienDuLieu();
        }
        
        Console.ReadLine();
    }
}
```

---

## 7. Bài tập nâng cao

### Bài 1: Class SinhVien

Tạo class `SinhVien` với:
- Fields: maSV, hoTen, diemTB
- Properties: MaSV, HoTen, DiemTB (DiemTB chỉ cho phép từ 0-10)
- Constructor: 2 loại (có tham số và không tham số)
- Methods: NhapDuLieu(), HienThongTin(), XepLoai()

**Gợi ý**:
```csharp
public string XepLoai()
{
    if (_diemTB >= 8) return "Giỏi";
    if (_diemTB >= 6.5) return "Khá";
    if (_diemTB >= 5) return "Trung bình";
    return "Yếu";
}
```

### Bài 2: Class SanPham

Tạo class `SanPham` cho quản lý kho hàng:
- Fields: maSP, tenSP, giaBan, soLuong
- Methods: NhapSanPham(), HienThongTin(), TinhThanhTien(), GiamGia(float phanTram)

### Bài 3: Static Members

Tạo class `KhachHang` với:
- Static field: đếm số lượng khách hàng
- Constructor: tăng bộ đếm mỗi khi tạo khách hàng mới
- Static method: HienSoLuongKhachHang()

**Gợi ý**:
```csharp
class KhachHang
{
    private static int _soLuongKH = 0;
    
    public KhachHang()
    {
        _soLuongKH++;
        Console.WriteLine($"Chào mừng khách hàng thứ {_soLuongKH}!");
    }
    
    public static void HienSoLuong()
    {
        Console.WriteLine($"Tổng số khách hàng: {_soLuongKH}");
    }
}
```

---

## Tổng kết

### Những điểm cần nhớ:

1. **Đóng gói** = Gộp dữ liệu + hành vi + che giấu thông tin
2. **Private** cho fields, **Public** cho properties và methods
3. **Properties** tốt hơn public fields (kiểm soát được)
4. **Constructor** dùng để khởi tạo giá trị ban đầu
5. **Methods** định nghĩa hành vi của đối tượng

### Quy tắc vàng:

✅ **NÊN**:
- Dùng private cho fields
- Dùng properties để truy cập fields
- Kiểm tra dữ liệu trong set của property
- Đặt tên có ý nghĩa

❌ **KHÔNG NÊN**:
- Dùng public cho fields
- Để dữ liệu quan trọng truy cập tự do
- Bỏ qua validation khi gán giá trị

---

**Chúc bạn học tốt! 💪**