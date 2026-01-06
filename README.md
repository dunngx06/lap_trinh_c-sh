# BÁO CÁO GIẢNG DẠY CHI TIẾT
# CHƯƠNG 2: LẬP TRÌNH VỚI C#

**Nguồn:** Trường Đại học Kinh tế Quốc dân  
**Khoa:** Công nghệ Thông tin và Kinh tế số  
**Giảng viên:** Phạm Thảo (thaop@neu.edu.vn)

---

## MỤC LỤC

1. [Giới Thiệu Tổng Quan](#1-giới-thiệu-tổng-quan)
2. [Khai Báo Biến và Kiểu Dữ Liệu](#2-khai-báo-biến-và-kiểu-dữ-liệu)
3. [Vào Ra Dữ Liệu Console](#3-vào-ra-dữ-liệu-console)
4. [Chuyển Đổi Kiểu Dữ Liệu](#4-chuyển-đổi-kiểu-dữ-liệu)
5. [Cú Pháp Lựa Chọn](#5-cú-pháp-lựa-chọn)
6. [Cú Pháp Lặp](#6-cú-pháp-lặp)
7. [Làm Việc Với Mảng](#7-làm-việc-với-mảng)
8. [Debug và Bắt Lỗi](#8-debug-và-bắt-lỗi)
9. [Phương Thức và Tham Số](#9-phương-thức-và-tham-số)
10. [Bài Tập Thực Hành](#10-bài-tập-thực-hành)

---

## 1. GIỚI THIỆU TỔNG QUAN

### 1.1 Mục Tiêu Học Tập

Sau khi hoàn thành chương này, sinh viên sẽ:
- Hiểu và sử dụng được các kiểu dữ liệu cơ bản trong C#
- Thực hiện được các thao tác nhập/xuất dữ liệu qua Console
- Sử dụng thành thạo các cấu trúc điều khiển (if, switch, vòng lặp)
- Làm việc được với mảng một chiều
- Biết cách debug và xử lý lỗi trong chương trình
- Xây dựng và sử dụng được các phương thức (hàm)

### 1.2 Namespace System

**Namespace** (Không gian tên) là cách tổ chức và nhóm các lớp (class) liên quan trong C#.

```csharp
using System;  // Khai báo sử dụng namespace System
```

**Giải thích:** 
- `System` là namespace cốt lõi chứa các lớp cơ bản như `Console`, `String`, `Int32`,...
- Khi viết `using System;`, ta có thể sử dụng trực tiếp `Console.WriteLine()` thay vì `System.Console.WriteLine()`

---

## 2. KHAI BÁO BIẾN VÀ KIỂU DỮ LIỆU

### 2.1 Cú Pháp Khai Báo Biến

**Cú pháp:**
```csharp
KieuDuLieu tenBien = giaTri;
```

**Quy tắc quan trọng:** Mỗi biến PHẢI được gán giá trị trước khi sử dụng.

### 2.2 Các Kiểu Dữ Liệu Cơ Bản

| Kiểu Dữ Liệu | Mô Tả | Ví Dụ |
|--------------|-------|-------|
| `int` (Int32) | Số nguyên 32-bit | `int tuoi = 25;` |
| `long` | Số nguyên 64-bit | `long danSo = 90000000L;` |
| `float` | Số thực 32-bit | `float diemTB = 8.5F;` |
| `double` | Số thực 64-bit | `double pi = 3.14159;` |
| `string` | Chuỗi ký tự | `string ten = "Nguyen Van A";` |
| `bool` | Giá trị logic | `bool dangHoatDong = true;` |
| `DateTime` | Ngày giờ | `DateTime homNay = DateTime.Now;` |
| `char` | Ký tự đơn | `char kyTu = 'A';` |

### 2.3 Ví Dụ Minh Họa

```csharp
using System;

class Program
{
    static void Main()
    {
        // Khai báo các kiểu dữ liệu
        string hoTen = "Nguyen Van A";
        int tuoi = 20;
        float diemTrungBinh = 8.5F;  // F là Literal cho float
        bool laSinhVien = true;
        
        // Hiển thị thông tin
        Console.WriteLine("Ho ten: " + hoTen);
        Console.WriteLine("Tuoi: " + tuoi);
        Console.WriteLine("Diem TB: " + diemTrungBinh);
        Console.WriteLine("La sinh vien: " + laSinhVien);
    }
}
```

### 2.4 Literal (Hậu tố kiểu dữ liệu)

**Literal** là ký tự đặc biệt đi kèm giá trị để xác định kiểu dữ liệu:

| Literal | Kiểu | Ví Dụ |
|---------|------|-------|
| `F` hoặc `f` | float | `float soDu = 3.5F;` |
| `D` hoặc `d` | double | `double pi = 3.14D;` |
| `L` hoặc `l` | long | `long soDan = 90000000L;` |
| `M` hoặc `m` | decimal | `decimal tien = 100.50M;` |

---

## 3. VÀO RA DỮ LIỆU CONSOLE

### 3.1 Xuất Dữ Liệu Ra Màn Hình

**Console.WriteLine()** - In ra và xuống dòng:
```csharp
Console.WriteLine("Xin chao!");  // In "Xin chao!" rồi xuống dòng
```

**Console.Write()** - In ra mà không xuống dòng:
```csharp
Console.Write("Ho ten: ");  // Con trỏ vẫn ở cùng dòng
```

### 3.2 Nhập Dữ Liệu Từ Bàn Phím

**Console.ReadLine()** - Đọc một dòng văn bản từ người dùng:
```csharp
string ten = Console.ReadLine();  // Đọc chuỗi người dùng nhập
```

> **Lưu ý:** `Console.ReadLine()` cũng có tác dụng dừng màn hình, chờ người dùng nhấn Enter.

### 3.3 Ví Dụ Hoàn Chỉnh

```csharp
using System;

class Program
{
    static void Main()
    {
        // Nhập họ tên
        Console.Write("Nhap ho ten: ");
        string hoTen = Console.ReadLine();
        
        // Nhập tuổi
        Console.Write("Nhap tuoi: ");
        string tuoiStr = Console.ReadLine();
        int tuoi = int.Parse(tuoiStr);  // Chuyển từ string sang int
        
        // Xuất kết quả
        Console.WriteLine("Xin chao " + hoTen + ", ban " + tuoi + " tuoi.");
        
        // Dừng màn hình
        Console.ReadLine();
    }
}
```

### 3.4 Escape Sequence (Ký tự điều khiển)

**Escape Sequence** là chuỗi ký tự đặc biệt thực hiện chức năng điều khiển:

| Escape Sequence | Chức Năng | Ví Dụ |
|-----------------|-----------|-------|
| `\n` | Xuống dòng mới | `Console.Write("Dong 1\nDong 2");` |
| `\t` | Tab (thụt vào) | `Console.Write("Cot1\tCot2");` |
| `\\` | In dấu \ | `Console.Write("C:\\Users");` |
| `\"` | In dấu " | `Console.Write("\"Hello\"");` |

---

## 4. CHUYỂN ĐỔI KIỂU DỮ LIỆU

### 4.1 Tại Sao Cần Chuyển Đổi?

Khi đọc dữ liệu từ `Console.ReadLine()`, kết quả luôn là **string**. Để thực hiện tính toán, ta cần chuyển sang kiểu số.

### 4.2 Hai Cách Chuyển Đổi

**Cách 1: Sử dụng Parse()**
```csharp
string chuoiSo = "123";
int soNguyen = int.Parse(chuoiSo);      // Kết quả: 123
float soThuc = float.Parse("3.14");     // Kết quả: 3.14
double soDouble = double.Parse("2.718"); // Kết quả: 2.718
```

**Cách 2: Sử dụng Convert**
```csharp
string chuoiSo = "456";
int soNguyen = Convert.ToInt32(chuoiSo);   // Kết quả: 456
float soThuc = Convert.ToSingle("6.28");   // Kết quả: 6.28
double soDouble = Convert.ToDouble("1.618"); // Kết quả: 1.618
```

### 4.3 Bảng Chuyển Đổi Convert

| Phương thức | Chuyển sang |
|-------------|-------------|
| `Convert.ToInt32()` | int |
| `Convert.ToInt64()` | long |
| `Convert.ToSingle()` | float |
| `Convert.ToDouble()` | double |
| `Convert.ToString()` | string |
| `Convert.ToBoolean()` | bool |

### 4.4 Place Holder và String Format

**Place Holder** (Chỗ giữ chỗ) dùng để định dạng chuỗi:

```csharp
string ten = "An";
int tuoi = 20;

// Cách 1: Dùng {0}, {1},...
Console.WriteLine("Ten: {0}, Tuoi: {1}", ten, tuoi);

// Cách 2: String Interpolation (C# 6.0+)
Console.WriteLine($"Ten: {ten}, Tuoi: {tuoi}");
```

**Định dạng số:**
```csharp
double tien = 1234567.89;

Console.WriteLine("{0:N2}", tien);   // 1,234,567.89 (số có phân cách)
Console.WriteLine("{0:C}", tien);    // $1,234,567.89 (tiền tệ)
Console.WriteLine("{0:P}", 0.85);    // 85.00% (phần trăm)
Console.WriteLine("{0:F2}", tien);   // 1234567.89 (2 chữ số thập phân)
```

---

## 5. CÚ PHÁP LỰA CHỌN

### 5.1 Câu Lệnh if

**Cú pháp cơ bản:**
```csharp
if (dieuKien)
{
    // Thực hiện nếu điều kiện ĐÚNG
}
```

**Ví dụ:**
```csharp
int tuoi = 18;

if (tuoi >= 18)
{
    Console.WriteLine("Ban da du tuoi bo phieu.");
}
```

### 5.2 Câu Lệnh if...else

```csharp
if (dieuKien)
{
    // Thực hiện nếu điều kiện ĐÚNG
}
else
{
    // Thực hiện nếu điều kiện SAI
}
```

**Ví dụ:**
```csharp
int diem = 45;

if (diem >= 50)
{
    Console.WriteLine("Dau");
}
else
{
    Console.WriteLine("Truot");
}
```

### 5.3 Câu Lệnh if...else if...else

```csharp
int diem = 75;

if (diem >= 90)
{
    Console.WriteLine("Xuat sac");
}
else if (diem >= 80)
{
    Console.WriteLine("Gioi");
}
else if (diem >= 70)
{
    Console.WriteLine("Kha");
}
else if (diem >= 50)
{
    Console.WriteLine("Trung binh");
}
else
{
    Console.WriteLine("Yeu");
}
```

### 5.4 Câu Lệnh switch

**Cú pháp:**
```csharp
switch (bienKiemTra)
{
    case giaTriA:
        // Thực hiện khi bienKiemTra == giaTriA
        break;
    case giaTriB:
        // Thực hiện khi bienKiemTra == giaTriB
        break;
    default:
        // Thực hiện khi không khớp case nào
        break;
}
```

**Ví dụ - Chọn menu:**
```csharp
Console.Write("Chon chuc nang (1-3): ");
int luaChon = int.Parse(Console.ReadLine());

switch (luaChon)
{
    case 1:
        Console.WriteLine("Ban da chon: Xem thong tin");
        break;
    case 2:
        Console.WriteLine("Ban da chon: Giao dich");
        break;
    case 3:
        Console.WriteLine("Tam biet!");
        break;
    default:
        Console.WriteLine("Lua chon khong hop le!");
        break;
}
```

---

## 6. CÚ PHÁP LẶP

### 6.1 Vòng Lặp for

**Cú pháp:**
```csharp
for (khởiTạo; điềuKiện; bướcNhảy)
{
    // Khối lệnh lặp
}
```

**Ví dụ - In số từ 1 đến 10:**
```csharp
for (int i = 1; i <= 10; i++)
{
    Console.WriteLine("So: " + i);
}
```

**Giải thích:**
- `int i = 1`: Khởi tạo biến đếm i = 1
- `i <= 10`: Tiếp tục lặp khi i <= 10
- `i++`: Sau mỗi vòng, tăng i lên 1

### 6.2 Vòng Lặp while

**Cú pháp:**
```csharp
while (điềuKiện)
{
    // Khối lệnh lặp
}
```

**Ví dụ - Đếm ngược:**
```csharp
int dem = 5;
while (dem > 0)
{
    Console.WriteLine(dem);
    dem--;  // Giảm dem đi 1
}
Console.WriteLine("Het gio!");
```

### 6.3 Vòng Lặp do...while

**Đặc điểm:** Thực hiện ÍT NHẤT 1 lần trước khi kiểm tra điều kiện.

```csharp
int luaChon;
do
{
    Console.WriteLine("1. Xem thong tin");
    Console.WriteLine("2. Giao dich");
    Console.WriteLine("3. Thoat");
    Console.Write("Chon: ");
    luaChon = int.Parse(Console.ReadLine());
    
} while (luaChon != 3);  // Lặp cho đến khi chọn 3
```

### 6.4 Vòng Lặp foreach

**Dùng để duyệt qua các phần tử trong mảng hoặc collection:**

```csharp
int[] mangSo = { 10, 20, 30, 40, 50 };

foreach (int so in mangSo)
{
    Console.WriteLine(so);
}
```

**So sánh for và foreach:**
```csharp
string[] tenSinhVien = { "An", "Binh", "Cuong" };

// Cách 1: Dùng for
for (int i = 0; i < tenSinhVien.Length; i++)
{
    Console.WriteLine(tenSinhVien[i]);
}

// Cách 2: Dùng foreach (gọn hơn)
foreach (string ten in tenSinhVien)
{
    Console.WriteLine(ten);
}
```

---

## 7. LÀM VIỆC VỚI MẢNG

### 7.1 Khái Niệm Mảng

**Mảng (Array)** là cấu trúc dữ liệu lưu trữ nhiều phần tử CÙNG KIỂU trong bộ nhớ liên tiếp.

### 7.2 Khai Báo Mảng

**Cách 1: Khai báo rồi khởi tạo sau**
```csharp
int[] mangSo = new int[5];  // Mảng 5 phần tử, mặc định = 0
mangSo[0] = 10;
mangSo[1] = 20;
```

**Cách 2: Khai báo và khởi tạo cùng lúc**
```csharp
int[] mangSo = { 10, 20, 30, 40, 50 };
```

**Cách 3: Khai báo với new và khởi tạo**
```csharp
int[] mangSo = new int[] { 10, 20, 30, 40, 50 };
```

### 7.3 Truy Cập Phần Tử Mảng

**Lưu ý:** Chỉ số mảng bắt đầu từ 0.

```csharp
int[] diem = { 8, 9, 7, 10, 6 };

Console.WriteLine(diem[0]);  // In ra 8 (phần tử đầu tiên)
Console.WriteLine(diem[4]);  // In ra 6 (phần tử cuối cùng)
Console.WriteLine(diem.Length);  // In ra 5 (số phần tử)
```

### 7.4 Duyệt Mảng

```csharp
int[] mangSo = { 1, 3, 5, 7, 9 };

// Dùng for
Console.WriteLine("Danh sach cac so:");
for (int i = 0; i < mangSo.Length; i++)
{
    Console.WriteLine($"Phan tu [{i}] = {mangSo[i]}");
}
```

### 7.5 Ví Dụ - Tính Tổng Mảng

```csharp
int[] mangSo = { 10, 20, 30, 40, 50 };
int tong = 0;

for (int i = 0; i < mangSo.Length; i++)
{
    tong += mangSo[i];  // tong = tong + mangSo[i]
}

Console.WriteLine("Tong cac phan tu: " + tong);  // Kết quả: 150
```

### 7.6 Ví Dụ - Tìm Số Lớn Nhất

```csharp
int[] mangSo = { 45, 23, 78, 12, 56 };
int max = mangSo[0];  // Giả sử phần tử đầu là lớn nhất

for (int i = 1; i < mangSo.Length; i++)
{
    if (mangSo[i] > max)
    {
        max = mangSo[i];
    }
}

Console.WriteLine("So lon nhat: " + max);  // Kết quả: 78
```

---

## 8. DEBUG VÀ BẮT LỖI

### 8.1 Debug trong Visual Studio

**Các phím tắt quan trọng:**

| Phím | Chức Năng |
|------|-----------|
| `F9` | Đặt/Bỏ Break Point (Điểm dừng) |
| `F5` | Chạy Debug |
| `F10` | Step Over - Chạy qua 1 lệnh |
| `F11` | Step Into - Đi vào chi tiết hàm |

**Giải thích:**
- **Break Point** là điểm mà chương trình sẽ dừng lại, cho phép ta xem giá trị các biến
- **Step Over (F10)**: Thực thi hàm như một bước duy nhất
- **Step Into (F11)**: Đi vào bên trong hàm để debug từng dòng

### 8.2 Xử Lý Ngoại Lệ (Exception)

**Try-Catch** dùng để bắt và xử lý lỗi:

```csharp
try
{
    // Khối lệnh có thể gây lỗi
    Console.Write("Nhap so: ");
    int so = int.Parse(Console.ReadLine());
    Console.WriteLine("So ban nhap: " + so);
}
catch (FormatException)
{
    // Xử lý khi nhập không phải số
    Console.WriteLine("Loi: Ban phai nhap so!");
}
catch (Exception ex)
{
    // Xử lý các lỗi khác
    Console.WriteLine("Loi: " + ex.Message);
}
```

### 8.3 Ví Dụ Xử Lý Lỗi Chia Cho 0

```csharp
try
{
    Console.Write("Nhap so bi chia: ");
    int a = int.Parse(Console.ReadLine());
    
    Console.Write("Nhap so chia: ");
    int b = int.Parse(Console.ReadLine());
    
    int ketQua = a / b;
    Console.WriteLine($"Ket qua: {a} / {b} = {ketQua}");
}
catch (DivideByZeroException)
{
    Console.WriteLine("Loi: Khong the chia cho 0!");
}
catch (FormatException)
{
    Console.WriteLine("Loi: Vui long nhap so nguyen!");
}
```

---

## 9. PHƯƠNG THỨC VÀ THAM SỐ

### 9.1 Khái Niệm Phương Thức

**Phương thức (Method)** hay còn gọi là **Hàm (Function)** là khối mã thực hiện một nhiệm vụ cụ thể, có thể được gọi lại nhiều lần.

### 9.2 Cú Pháp Khai Báo

```csharp
static KieuTraVe TenPhuongThuc(ThamSo1, ThamSo2, ...)
{
    // Thân hàm
    return giaTriTraVe;  // Nếu có kiểu trả về
}
```

### 9.3 Ví Dụ - Hàm Không Trả Về Giá Trị (void)

```csharp
static void ChaoMung(string ten)
{
    Console.WriteLine("Xin chao, " + ten + "!");
}

// Gọi hàm
static void Main()
{
    ChaoMung("An");    // In ra: Xin chao, An!
    ChaoMung("Binh");  // In ra: Xin chao, Binh!
}
```

### 9.4 Ví Dụ - Hàm Có Trả Về Giá Trị

```csharp
static int TinhTong(int a, int b)
{
    return a + b;
}

static void Main()
{
    int ketQua = TinhTong(5, 3);
    Console.WriteLine("Tong: " + ketQua);  // In ra: Tong: 8
}
```

### 9.5 Tham Trị và Tham Chiếu

**Tham Trị (Pass by Value):** Truyền bản sao của giá trị, không ảnh hưởng biến gốc.

```csharp
static void Tang(int so)
{
    so = so + 10;  // Chỉ thay đổi bản sao
}

static void Main()
{
    int x = 5;
    Tang(x);
    Console.WriteLine(x);  // Vẫn là 5
}
```

**Tham Chiếu (Pass by Reference):** Dùng từ khóa `ref`, thay đổi trực tiếp biến gốc.

```csharp
static void Tang(ref int so)
{
    so = so + 10;  // Thay đổi biến gốc
}

static void Main()
{
    int x = 5;
    Tang(ref x);
    Console.WriteLine(x);  // Bây giờ là 15
}
```

### 9.6 Ví Dụ - Hàm Tính Tiền Lãi

```csharp
static float TinhTienLai(float soTienVay, int soNamVay, ref float laiSuat)
{
    // Xác định lãi suất theo số tiền vay
    if (soTienVay < 5000)
        laiSuat = 0;
    else if (soTienVay < 100000)
        laiSuat = 10.5f;
    else if (soTienVay < 250000)
        laiSuat = 10f;
    else if (soTienVay < 500000)
        laiSuat = 9.5f;
    else
        laiSuat = 9f;
    
    // Tính tiền lãi
    float tienLai = (soTienVay * soNamVay * laiSuat) / 100;
    return tienLai;
}

static void Main()
{
    float laiSuat = 0;
    float tienLai = TinhTienLai(200000, 3, ref laiSuat);
    
    Console.WriteLine($"Lai suat: {laiSuat}%");
    Console.WriteLine($"Tien lai: {tienLai}");
}
```

---

## 10. BÀI TẬP THỰC HÀNH

### Bài Tập 1: Tính Tổng Mảng

**Yêu cầu:** Xây dựng hàm tính tổng các phần tử trong mảng.

```csharp
static int TinhTong(int[] mang)
{
    int tong = 0;
    for (int i = 0; i < mang.Length; i++)
    {
        tong += mang[i];
    }
    return tong;
}

static void Main()
{
    int[] danhSach = { 10, 20, 30, 40, 50 };
    int ketQua = TinhTong(danhSach);
    Console.WriteLine("Tong: " + ketQua);  // Kết quả: 150
}
```

---

### Bài Tập 2: Sắp Xếp Mảng Ký Tự

**Yêu cầu:** Sắp xếp mảng ký tự theo thứ tự ABC.

```csharp
static void SapXep(char[] mang)
{
    for (int i = 0; i < mang.Length - 1; i++)
    {
        for (int j = i + 1; j < mang.Length; j++)
        {
            if (mang[i] > mang[j])
            {
                // Hoán đổi vị trí
                char tam = mang[i];
                mang[i] = mang[j];
                mang[j] = tam;
            }
        }
    }
}

static void Main()
{
    char[] kyTu = { 'D', 'B', 'A', 'E', 'C' };
    SapXep(kyTu);
    
    Console.Write("Sau khi sap xep: ");
    foreach (char c in kyTu)
    {
        Console.Write(c + " ");
    }
    // Kết quả: A B C D E
}
```

---

### Bài Tập 3: Kiểm Tra Số Tồn Tại

**Yêu cầu:** Kiểm tra một số có tồn tại trong mảng không.

```csharp
static bool KiemTraTonTai(int[] mang, int soCanTim)
{
    for (int i = 0; i < mang.Length; i++)
    {
        if (mang[i] == soCanTim)
        {
            return true;
        }
    }
    return false;
}

static void Main()
{
    int[] danhSach = { 5, 10, 15, 20, 25 };
    
    Console.Write("Nhap so can tim: ");
    int so = int.Parse(Console.ReadLine());
    
    if (KiemTraTonTai(danhSach, so))
    {
        Console.WriteLine($"So {so} TON TAI trong mang.");
    }
    else
    {
        Console.WriteLine($"So {so} KHONG TON TAI trong mang.");
    }
}
```

---

### Bài Tập 4: Đếm Số Lần Xuất Hiện

**Yêu cầu:** Đếm số lần xuất hiện của một số trong mảng.

```csharp
static int DemSoLan(int[] mang, int soCanDem)
{
    int dem = 0;
    for (int i = 0; i < mang.Length; i++)
    {
        if (mang[i] == soCanDem)
        {
            dem++;
        }
    }
    return dem;
}

static void Main()
{
    int[] danhSach = { 1, 3, 5, 3, 7, 3, 8, 7, 3 };
    
    Console.Write("Nhap so can dem: ");
    int so = int.Parse(Console.ReadLine());
    
    int soLan = DemSoLan(danhSach, so);
    Console.WriteLine($"So {so} xuat hien {soLan} lan.");
}
```

---

### Bài Tập 5: Thống Kê Tất Cả Số Trong Mảng

**Yêu cầu:** In ra danh sách các số và số lần xuất hiện của chúng.

```csharp
static void ThongKe(int[] mang)
{
    bool[] daDem = new bool[mang.Length];
    
    Console.WriteLine("So\tSo lan xuat hien");
    Console.WriteLine("---\t-----------------");
    
    for (int i = 0; i < mang.Length; i++)
    {
        if (!daDem[i])
        {
            int dem = 1;
            for (int j = i + 1; j < mang.Length; j++)
            {
                if (mang[j] == mang[i])
                {
                    dem++;
                    daDem[j] = true;
                }
            }
            Console.WriteLine($"{mang[i]}\t{dem}");
        }
    }
}

static void Main()
{
    int[] danhSach = { 1, 3, 5, 6, 7, 3, 6, 7, 8, 7 };
    ThongKe(danhSach);
}
```

**Kết quả:**
```
So      So lan xuat hien
---     -----------------
1       1
3       2
5       1
6       2
7       3
8       1
```

---

### Bài Tập 6: Xóa Phần Tử Tại Vị Trí

**Yêu cầu:** Xóa phần tử tại vị trí index, dồn các phần tử bên phải về.

```csharp
static int[] XoaTaiViTri(int[] mang, int viTri)
{
    if (viTri < 0 || viTri >= mang.Length)
    {
        Console.WriteLine("Vi tri khong hop le!");
        return mang;
    }
    
    // Dồn các phần tử bên phải về trái
    for (int i = viTri; i < mang.Length - 1; i++)
    {
        mang[i] = mang[i + 1];
    }
    
    // Gán phần tử cuối = 0
    mang[mang.Length - 1] = 0;
    
    return mang;
}

static void Main()
{
    int[] danhSach = { 1, 2, 3, 4 };
    
    Console.WriteLine("Truoc khi xoa: " + string.Join(", ", danhSach));
    
    XoaTaiViTri(danhSach, 2);  // Xóa tại vị trí 2
    
    Console.WriteLine("Sau khi xoa: " + string.Join(", ", danhSach));
}
```

**Kết quả:**
```
Truoc khi xoa: 1, 2, 3, 4
Sau khi xoa: 1, 2, 4, 0
```

---

### Bài Tập 7: Hợp Hai Mảng (Union)

**Yêu cầu:** Trả về hợp của 2 mảng, mỗi phần tử chỉ xuất hiện 1 lần.

```csharp
static int[] HopMang(int[] a, int[] b)
{
    // Tạo mảng tạm đủ lớn
    int[] tam = new int[a.Length + b.Length];
    int dem = 0;
    
    // Thêm tất cả phần tử từ mảng a
    for (int i = 0; i < a.Length; i++)
    {
        tam[dem++] = a[i];
    }
    
    // Thêm phần tử từ mảng b nếu chưa có
    for (int i = 0; i < b.Length; i++)
    {
        bool daCo = false;
        for (int j = 0; j < dem; j++)
        {
            if (tam[j] == b[i])
            {
                daCo = true;
                break;
            }
        }
        if (!daCo)
        {
            tam[dem++] = b[i];
        }
    }
    
    // Tạo mảng kết quả đúng kích thước
    int[] ketQua = new int[dem];
    for (int i = 0; i < dem; i++)
    {
        ketQua[i] = tam[i];
    }
    
    return ketQua;
}

static void Main()
{
    int[] a = { 1, 2, 3, 4 };
    int[] b = { 2, 3, 5, 6 };
    
    int[] hop = HopMang(a, b);
    
    Console.WriteLine("Hop cua 2 mang: " + string.Join(", ", hop));
}
```

**Kết quả:**
```
Hop cua 2 mang: 1, 2, 3, 4, 5, 6
```

---

### Bài Tập 8: Giao Hai Mảng (Intersect)

**Yêu cầu:** Trả về các phần tử xuất hiện trong CẢ HAI mảng.

```csharp
static int[] GiaoMang(int[] a, int[] b)
{
    int[] tam = new int[Math.Min(a.Length, b.Length)];
    int dem = 0;
    
    for (int i = 0; i < a.Length; i++)
    {
        for (int j = 0; j < b.Length; j++)
        {
            if (a[i] == b[j])
            {
                // Kiểm tra đã thêm chưa
                bool daCo = false;
                for (int k = 0; k < dem; k++)
                {
                    if (tam[k] == a[i])
                    {
                        daCo = true;
                        break;
                    }
                }
                if (!daCo)
                {
                    tam[dem++] = a[i];
                }
                break;
            }
        }
    }
    
    int[] ketQua = new int[dem];
    for (int i = 0; i < dem; i++)
    {
        ketQua[i] = tam[i];
    }
    
    return ketQua;
}

static void Main()
{
    int[] a = { 1, 2, 3, 4 };
    int[] b = { 2, 3, 5, 6 };
    
    int[] giao = GiaoMang(a, b);
    
    Console.WriteLine("Giao cua 2 mang: " + string.Join(", ", giao));
}
```

**Kết quả:**
```
Giao cua 2 mang: 2, 3
```

---

## TỔNG KẾT

Chương này đã trình bày các kiến thức nền tảng quan trọng trong lập trình C#:

| Chủ Đề | Nội Dung Chính |
|--------|----------------|
| **Khai báo biến** | Các kiểu dữ liệu cơ bản, literal, quy tắc đặt tên |
| **Vào ra Console** | WriteLine, Write, ReadLine, Escape Sequence |
| **Chuyển đổi kiểu** | Parse(), Convert, Place Holder, Format |
| **Cú pháp lựa chọn** | if, if-else, if-else if, switch-case |
| **Cú pháp lặp** | for, while, do-while, foreach |
| **Mảng** | Khai báo, truy cập, duyệt, các thao tác cơ bản |
| **Debug** | Break Point, F10, F11, Try-Catch |
| **Phương thức** | Khai báo hàm, tham trị, tham chiếu (ref) |

---

*Báo cáo được hệ thống lại từ bài giảng của ThS. Phạm Thảo - Trường Đại học Kinh tế Quốc dân*
