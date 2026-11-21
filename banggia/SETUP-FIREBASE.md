# Kết nối bảng giá với Firebase

Ứng dụng dùng Firebase Authentication (Google) và Cloud Firestore. GitHub Pages chỉ lưu HTML/JS, không cần GitHub token khi xuất bản bảng giá.

1. Vào https://console.firebase.google.com/ tạo một Firebase project. Tạo Web App và sao chép các trường `apiKey`, `authDomain`, `projectId`, `appId` vào `banggia/firebase-config.js`. Đây là cấu hình web công khai, không phải mật khẩu.
2. Firebase Console → Authentication → Sign-in method → bật Google. Trong Authentication → Settings → Authorized domains, thêm `adamavaadam.github.io` (không nhập `https://` hoặc đường dẫn).
3. Firebase Console → Firestore Database → tạo database ở chế độ production (không chọn test mode). Không bật quyền ghi công khai.
4. Mở `/banggia/admin.html`, đăng nhập Google. Trang sẽ hiện Firebase Auth UID, sao chép UID (không phải email). Trong Firestore → Rules, dán nội dung `banggia/firestore.rules` rồi thay `REPLACE_WITH_ADMIN_FIREBASE_UID` bằng UID đó, nhấn Publish.
5. Quay lại Admin, dán bảng giá, lọc, tích chọn, xem trước, nhấn Xuất bản. Trang khách `/banggia/` đọc tài liệu `priceLists/current` bằng thời gian thực.

## Bảo mật
- Chỉ một tài khoản Google có UID được cấu hình mới có quyền ghi. Email/tên miền Google không tự cho quyền.
- Quy tắc chỉ cho phép đúng một tài liệu giá công khai; dữ liệu xuất bản chỉ gồm `items: [{ten,gia}]` và thời điểm cập nhật.
- Mỗi bảng có tối đa 1.500 dòng; Firestore còn giới hạn kích thước một tài liệu 1 MiB. Bảng quá lớn cần chia lô bằng phương án khác.
- Người dùng công khai có thể đọc dữ liệu đã xuất bản, kể cả qua API; không đưa ghi chú hoặc giá nhạy cảm vào tài liệu công khai.
- Trường cấu hình Firebase không phải secret; quyền truy cập do Firestore Rules quyết định.
