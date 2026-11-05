# Báo cáo: Rà soát và Xử lý An toàn Thông tin Dự án SmartCampus

---

## Phần 1 – Nhận diện rủi ro và xử lý sự cố

1. **Tình huống 1:** 
   * **Tên loại tấn công:** Kết hợp giữa **Phishing** và **Typosquatting** (giả mạo tên miền sai chính tả).
   * **Chi tiết giả mạo:** Đường link `https://githuh-security.com/verify` sử dụng chữ `githuh` thay vì `github` nhằm đánh lừa người dùng đánh cắp mật khẩu.

2. **Tình huống 2:** 
   * **Tên cạm bẫy:** Mã độc ngụy trang phần mềm crack (Malicious Software / Infostealer).
   * **Lý do nguy hiểm:** Yêu cầu "tắt Windows Defender" nhằm vô hiệu hóa tường lửa/diệt virus, tạo điều kiện cho mã độc đánh cắp cookie, mật khẩu và quyền kiểm soát hệ thống.

3. **Đối chiếu với Quy tắc 5 KHÔNG:**
   * **Tình huống 1:** Áp dụng điều **KHÔNG click vào các đường link lạ / không rõ nguồn gốc**.
   * **Tình huống 2:** Áp dụng điều **KHÔNG tải và sử dụng phần mềm bẻ khóa (crack) trôi nổi**.

4. **4 Bước Cấp cứu khi đã lỡ chạy file crack:**
   * **Bước 1:** Ngắt kết nối mạng ngay lập tức (Rút dây mạng / Tắt Wi-Fi) để chặn mã độc truyền dữ liệu về C2 Server.
   * **Bước 2:** Cô lập thiết bị, dùng thiết bị sạch khác để đổi toàn bộ mật khẩu tài khoản quan trọng.
   * **Bước 3:** Đăng xuất từ xa tất cả các phiên làm việc hiện tại (Log out of all sessions).
   * **Bước 4:** Quét virus toàn diện hoặc cài lại hệ điều hành (Format/Reinstall OS) để làm sạch hệ thống.

---

## Phần 2 – Chuyển sang Password Manager

1. **Rủi ro khi dùng chung mật khẩu:** 
   * Nếu email nhóm bị lộ mật khẩu, kẻ tấn công dễ dàng xâm nhập vào GitHub tổ chức và Vercel deploy. 
   * **Tên kiểu tấn công:** **Credential Stuffing** (Nhồi thông tin xác thực) hoặc Lợi dụng liên kết yếu nhất.

2. **Nguyên lý Zero-Knowledge Architecture của Bitwarden:**
   * Dữ liệu được mã hóa cục bộ trên thiết bị người dùng bằng Master Password trước khi đẩy lên đám mây. Máy chủ chỉ lưu bản mã hóa và hoàn toàn không có khóa giải mã. Do đó, kể cả khi máy chủ bị hack, dữ liệu thu được vẫn là các chuỗi mã hóa vô nghĩa.

3. **Thông số cấu hình Password Generator:**
| Thông số | Giá trị cấu hình |
| :--- | :--- |
| **Độ dài (Length)** | 16 - 20 ký tự |
| **Chữ hoa (A–Z)** | Bật (Có) |
| **Chữ thường (a–z)** | Bật (Có) |
| **Chữ số (0–9)** | Bật (Có) |
| **Ký tự đặc biệt (!@#$...)** | Bật (Có) |

---

## Phần 3 – Kích hoạt 2FA cho tài khoản GitHub tổ chức

1. **Vì sao nên dùng Authenticator App thay vì SMS OTP:**
   * SMS OTP gửi dạng văn bản thuần túy, dễ bị tấn công **SIM Swapping** hoặc nghe lén sóng mạng (SS7 Attack).
   * **2 nhược điểm của SMS OTP:** Dễ bị chiếm đoạt số điện thoại; Không hoạt động khi mất sóng hoặc đi nước ngoài.

2. **Các bước kích hoạt 2FA bằng Google Authenticator:**
   * Bước 1: Vào GitHub -> **Settings** -> **Password and authentication**.
   * Bước 2: Chọn **Enable two-factor authentication** -> Chọn **Authentication app**.
   * Bước 3: Mở ứng dụng **Google Authenticator** trên điện thoại, quét mã QR hiển thị trên màn hình máy tính.
   * Bước 4: Nhập mã OTP 6 số từ ứng dụng để hoàn tất.

3. **Quản lý 10 mã khôi phục (Recovery Codes):**
   * **Nơi NÊN lưu:** In ra giấy cất két sắt, hoặc lưu file mã hóa trong Password Manager.
   * **Nơi KHÔNG NÊN lưu:** Không chụp màn hình, không lưu file văn bản thô trên Desktop hay Cloud công khai.

---

## Phần 4 – Bảo vệ Secrets trước khi push code

### 1. Chỉ định 2 giá trị Secrets đang bị hardcode:
* `API_KEY = "sk-proj-abc123456789"`
* `DB_PASSWORD = "SmartCampus@2026"`

### 2. Đoạn code `config.js` chuẩn hóa qua `process.env`:
```javascript
const API_KEY = process.env.API_KEY;
const DB_PASSWORD = process.env.DB_PASSWORD;

function connectDatabase() {
  return connect(DB_PASSWORD);
}

module.exports = { connectDatabase };