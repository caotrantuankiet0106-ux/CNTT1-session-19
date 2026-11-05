# Báo cáo: Quy trình cấp cứu và Xử lý mã độc Infostealer

---

## 1. Báo cáo phân tích (IPO)
* **Input (Dấu hiệu nhiễm):** Mất quyền kiểm soát tài khoản liên tục (Facebook, Gmail), phiên làm việc bị đá văng ra, xuất hiện thông báo đổi bảo mật trái phép.
* **Process (Các bước xử lý):**
  1. Ngắt kết nối mạng ngay lập tức để chặn mã độc gửi dữ liệu về C2 Server.
  2. Cô lập thiết bị, dùng thiết bị sạch khác để đổi mật khẩu.
  3. Đăng xuất toàn bộ phiên làm việc cũ từ xa (Log out all sessions).
  4. Quét virus toàn diện hoặc cài lại hệ điều hành (Format OS).
* **Output (Hệ thống sạch):** Tài khoản được bảo vệ an toàn bằng mật khẩu mới và 2FA; thiết bị sạch mã độc.

---

## 2. Triển khai quy trình cấp cứu chi tiết

### Bước 1: Đổi mật khẩu từ thiết bị khác (Clean Device)
* Tuyệt đối không dùng máy tính đang nghi ngờ nhiễm mã độc. 
* Sử dụng điện thoại hoặc máy tính sạch khác để đăng nhập vào trang quản trị tài khoản, tiến hành **Thay đổi mật khẩu** và chọn ngay **"Đăng xuất khỏi tất cả các thiết bị"**.

### Bước 2: Thu hồi quyền truy cập (Revoke Token) trên GitHub
1. Truy cập tài khoản GitHub cá nhân -> Vào **Settings**.
2. Chọn **Developer settings** ở menu trái -> Chọn **Personal access tokens** (hoặc **OAuth Apps**).
3. Kiểm tra danh sách các token đang hoạt động và nhấn **Revoke** (Thu hồi) toàn bộ token hoặc các token nghi ngờ để cắt đứt quyền truy cập mã nguồn của mã độc.