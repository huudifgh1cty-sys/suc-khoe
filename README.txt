GAN KHỎE — Google Drive 2-Way Sync
===================================

Gói này gồm:
- index.html: ứng dụng GAN KHỎE hiện tại, có đăng nhập Google và đồng bộ dữ liệu với Google Drive.
- README.txt: hướng dẫn cấu hình Google Cloud và chạy ứng dụng.

1. CẤU HÌNH GOOGLE CLOUD
------------------------
A. Tạo Project
- Mở Google Cloud Console.
- Tạo project mới, ví dụ: GAN KHOE.

B. Bật Google Drive API
- Vào APIs & Services / Library.
- Tìm "Google Drive API".
- Chọn Enable.

C. Cấu hình OAuth
- Vào Google Auth Platform / Branding (tên menu có thể khác tùy giao diện).
- Tạo/cấu hình OAuth consent screen.
- App name: GAN KHỎE.
- Chọn email hỗ trợ của bạn.
- Scope ứng dụng sử dụng:
  https://www.googleapis.com/auth/drive.appdata

D. Tạo OAuth Client ID
- Vào Google Auth Platform / Clients hoặc APIs & Services / Credentials.
- Create Client.
- Application type: Web application.
- Đặt tên: GAN KHOE Web.
- Thêm Authorized JavaScript origins.

Ví dụ chạy local bằng:
  http://localhost:5500

Hãy thêm:
  http://localhost
  http://localhost:5500

Nếu bạn chạy bằng port khác, thêm đúng origin có port đó.

Khi triển khai thật:
- Dùng HTTPS.
- Thêm đúng origin website của bạn, ví dụ:
  https://gankhoe.example.com

Không đưa Client Secret vào index.html.

2. ĐIỀN CLIENT ID
-----------------
Mở index.html và tìm:

const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_OAUTH_CLIENT_ID.apps.googleusercontent.com';

Thay phần bên trong dấu nháy bằng Client ID Web application Google cấp cho bạn.

Ví dụ:
const GOOGLE_CLIENT_ID = '1234567890-abc123def456.apps.googleusercontent.com';

3. CHẠY LOCAL
-------------
Không nên mở file bằng file:///...

Bạn có thể dùng VS Code + Live Server hoặc một HTTP server.

Ví dụ với Python:
  python -m http.server 5500

Sau đó mở:
  http://localhost:5500

Origin này phải nằm trong Authorized JavaScript origins.

4. CÁCH HOẠT ĐỘNG
-----------------
- Người dùng bấm "Đăng nhập Google".
- Chọn tài khoản Google.
- App xin quyền Drive appdata.
- Dữ liệu GAN KHỎE được lưu thành:
  gankhoe-data-v1.json
  trong appDataFolder của ứng dụng.

- Khi dữ liệu thay đổi, app lưu local và đẩy dữ liệu lên Drive.
- App kiểm tra thay đổi từ Drive định kỳ khoảng 3 giây khi trang đang mở.
- Nếu thiết bị khác có dữ liệu mới hơn, app tải dữ liệu đó xuống và cập nhật giao diện.

5. ĐỒNG BỘ NHIỀU THIẾT BỊ
--------------------------
Thiết bị A:
- Mở ứng dụng.
- Đăng nhập cùng tài khoản Google.
- Sửa dữ liệu.
- Dữ liệu được lưu lên Drive.

Thiết bị B:
- Mở cùng ứng dụng.
- Đăng nhập cùng tài khoản Google.
- Dữ liệu được lấy từ Drive.

Nếu cả hai thiết bị cùng sửa trong cùng một thời điểm, phiên bản hiện tại dùng updatedAt để ưu tiên dữ liệu có thời điểm cập nhật mới hơn. Đây là cơ chế chống ghi đè cơ bản, không phải hệ thống conflict-resolution cấp máy chủ.

6. LƯU Ý OAUTH / TRIỂN KHAI THẬT
--------------------------------
- Chỉ dùng domain/origin mà bạn sở hữu hoặc được phép sử dụng.
- Production nên dùng HTTPS.
- Nếu ứng dụng công khai cho nhiều người dùng, hãy kiểm tra yêu cầu xác minh/branding của Google.
- Không chia sẻ Client Secret.
- Client ID web có thể xuất hiện trong mã frontend; Client Secret thì không.

7. NẾU GẶP LỖI
--------------
"origin_mismatch":
- Kiểm tra URL hiện tại trên trình duyệt.
- Thêm chính xác scheme + host + port vào Authorized JavaScript origins.

"Google Drive API has not been used":
- Kiểm tra Drive API đã được Enable trong đúng project chưa.

"Access blocked":
- Kiểm tra OAuth consent/branding và tài khoản được phép thử ứng dụng.
- Kiểm tra scope Drive appdata.

"Không đồng bộ":
- Kiểm tra đã điền GOOGLE_CLIENT_ID chưa.
- Kiểm tra mạng.
- Mở Developer Tools (F12) > Console để xem lỗi.

8. PHẠM VI QUYỀN
----------------
Ứng dụng hiện dùng:
https://www.googleapis.com/auth/drive.appdata

Scope này dành cho dữ liệu cấu hình riêng của ứng dụng trong Google Drive; dữ liệu không được thiết kế để xuất hiện như một file thông thường trong My Drive.

9. TÀI LIỆU GOOGLE
------------------
Google Drive API:
https://developers.google.com/workspace/drive/api

OAuth 2.0:
https://developers.google.com/identity/protocols/oauth2

Google Identity Services:
https://developers.google.com/identity/gsi/web
