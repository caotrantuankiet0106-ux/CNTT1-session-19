# Báo cáo: Đánh giá và Lựa chọn giải pháp Xác thực Đa yếu tố (2FA) tại PixelForge Studio

---

## Phần 1: Phân tích 2 Giải pháp Xác thực

### 1. Phân tích 3 yếu tố xác thực danh tính số
* **Something You Know (Thứ bạn biết):** Mật khẩu (Password) hoặc mã PIN.
* **Something You Have (Thứ bạn có):** Thiết bị phần cứng, số điện thoại, hoặc ứng dụng sinh mã OTP (Authenticator App).
* **Something You Are (Thứ bạn là):** Sinh dữ liệu trắc sinh học (vân tay, khuôn mặt).
* **Áp dụng vào 2FA:** 
  * Mật khẩu đại diện cho yếu tố **Something You Know**.
  * Mã OTP đại diện cho yếu tố **Something You Have**.

### 2. Nguyên lý hoạt động của TOTP (Time-based One-Time Password) khi bật Chế độ máy bay
* Ứng dụng Authenticator (như Google/Microsoft Authenticator) hoạt động hoàn toàn ngoại tuyến (Offline) dựa trên việc kết hợp hai thành phần cốt lõi:
  1. **Khóa bí mật (Shared Secret):** Đã được trao đổi và lưu trữ an toàn trên thiết bị khi người dùng quét mã QR lúc thiết lập ban đầu.
  2. **Bộ đếm thời gian (Timestamp):** Đồng hồ hệ thống trên điện thoại di động (được đồng bộ chuẩn quốc tế UTC).
* **Cơ chế sinh mã:** Cứ mỗi 30 giây, thuật toán toán học (HMAC-SHA-1) sẽ lấy khóa bí mật kết hợp với mốc thời gian hiện tại của điện thoại để băm (hash) và trích xuất ra một đoạn mã 6 số ngẫu nhiên. Do đó, ngay cả khi bật chế độ máy bay (ngắt hoàn toàn Wi-Fi, 4G, SMS), ứng dụng vẫn tự tính toán ra mã chính xác nhờ vào bộ đếm thời gian thực và khóa bí mật có sẵn trong máy.

---

## Phần 2: Lập bảng So sánh Trade-off & Ra quyết định

### 3. Bảng so sánh 4 tiêu chí cốt lõi

| Tiêu chí đánh giá | Giải pháp 1: SMS OTP | Giải pháp 2: TOTP Authenticator |
| :--- | :--- | :--- |
| **1. Chống SIM Swapping & SS7** | **Kém** — Dễ bị chiếm đoạt số điện thoại hoặc nghe lén sóng mạng viễn thông. | **Tuyệt đối** — Khóa bí mật (Shared Secret) lưu an toàn trên thiết bị cá nhân. |
| **2. Tính sẵn sàng Offline / Nước ngoài** | **Kém** — Phụ thuộc sóng di động, mất sóng hoặc ra nước ngoài không nhận được tin nhắn. | **Tuyệt đối** — Hoạt động 100% Offline mọi lúc mọi nơi kể cả trên máy bay. |
| **3. Độ tiện dụng & Chi phí** | Quen thuộc nhưng doanh nghiệp tốn chi phí gửi SMS viễn thông cho từng lượt. | Miễn phí 100%, thao tác mở app lấy mã nhanh chóng, không tốn phí nhà mạng. |
| **4. Rủi ro khi mất điện thoại** | Làm lại SIM là lấy lại được mã, nhưng nguy cơ kẻ xấu trộm SIM chiếm tài khoản rất cao. | Cần sử dụng Bộ Mã Khôi Phục (Backup Codes) hoặc tính năng Cloud Sync đã mã hóa. |

### 4. Kết luận lựa chọn cho PixelForge Studio
* **Giải pháp lựa chọn:** Ban Giám đốc PixelForge Studio **bắt buộc phải áp dụng Giải pháp 2 (TOTP Authenticator)**.
* **Lý do:** 
  1. **Bảo mật tuyệt đối trước tấn công Social Engineering / SIM Swapping:** Tài khoản quản trị chứa mã nguồn game trị giá hàng trăm ngàn USD là mục tiêu tấn công cao cấp, SMS OTP quá dễ bị qua mặt qua các hình thức chiếm đoạt số điện thoại nhà mạng.
  2. **Phù hợp bối cảnh di chuyển toàn cầu:** Lập trình viên thường xuyên đi công tác nước ngoài (GDC) hoặc làm việc trên các chuyến bay dài (không có sóng viễn thông). TOTP hoạt động hoàn toàn ngoại tuyến giúp họ làm việc liên tục không bị gián đoạn.

---

## Phần 3: Quản lý Rủi ro & Backup Codes

### 5. Quản lý Mã khôi phục (Backup / Recovery Codes)
* **Mục đích của bộ 10 Mã khôi phục:** Dùng làm phương án dự phòng khẩn cấp để truy cập tài khoản khi nhân viên bị mất điện thoại, hỏng thiết bị, hoặc ứng dụng Authenticator bị xóa. Mỗi mã chỉ sử dụng được đúng 1 lần duy nhất.
* **2 Vị trí lưu trữ an toàn tuyệt đối:**
  1. In ra giấy và cất giữ trong két sắt cá nhân hoặc tủ khóa bảo mật tại văn phòng.
  2. Lưu trữ dưới dạng tệp đã mã hóa bằng phần mềm quản lý mật khẩu chuyên dụng (như Bitwarden, 1Password) có kích hoạt master password mạnh.
* **1 Sai lầm tai hại cần tránh:** Lưu bộ mã khôi phục dưới dạng ảnh chụp màn hình (Screenshot) hoặc tệp văn bản thô (Notepad, Word) không mã hóa trên chính chiếc điện thoại hoặc để ngoài màn hình Desktop/Cloud công khai, tạo cơ hội cho mã độc Infostealer đánh cắp toàn bộ.