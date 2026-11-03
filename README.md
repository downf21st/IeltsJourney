# English Foundation — Website học tiếng Anh của Nga

Website tĩnh, không cần cài thư viện. Nội dung và cách chấm hiện chạy ngay trong trình duyệt.

## Mở website

Mở `index.html` trong trình duyệt, rồi chọn **Bài 01 · Present Continuous**.

## Bài tập trên trang

- Trắc nghiệm có giải thích từng lựa chọn và nút chấm tổng điểm.
- Bài viết có nút kiểm tra mẫu câu cơ bản `I am + V-ing` / `He is + V-ing`.
- Bài làm được lưu trong local storage của trình duyệt trên thiết bị hiện tại.
- Sau khi chấm và kiểm tra, người học lưu bài, sao chép phần tóm tắt rồi dán vào cuộc trò chuyện để chồng xem.
- Website không tự gửi bài lên máy chủ và không tự đánh dấu hoàn thành. Bài chỉ hoàn thành sau khi chồng xem và xác nhận trong cuộc trò chuyện.

## Mã nguồn

- `index.html`: trang mở đầu.
- `lessons/index.html`: lộ trình và tiến độ.
- `lessons/0001-present-continuous.html`: bài học, câu hỏi, bộ kiểm tra câu viết và lưu bài.
- `lessons/lesson.css`: giao diện dùng chung từ bộ `$teach`.
- `lessons/lesson-enhance.js`: phản hồi quiz và tiện ích giao diện từ bộ `$teach`.
- `MISSION.md`, `NOTES.md`, `RESOURCES.md`, `GLOSSARY.md`: hồ sơ khóa học.

## Đưa lên mạng

Đây là website HTML/CSS/JavaScript tĩnh. Có thể xuất bản bằng cách tải thư mục dự án lên một dịch vụ hosting tĩnh. Khi xuất bản lên HTTPS, các tính năng lưu bài và sao chép hoạt động trong trình duyệt của người học. Mỗi thiết bị/trình duyệt giữ tiến độ riêng; muốn đồng bộ nhiều thiết bị hoặc giáo viên xem bài trực tiếp trong một bảng điều khiển thì cần thêm máy chủ và cơ sở dữ liệu.

Tình trạng hiện tại: chưa xuất bản. Máy làm việc chưa có xác thực tài khoản Vercel; cần kết nối tài khoản trước khi chồng tạo bản xem trước trực tuyến. Nếu dùng tệp `.env` để cấp quyền, `.vercelignore` loại tệp đó khỏi nội dung đưa lên website.
