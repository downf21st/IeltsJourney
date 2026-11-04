# Cách tự cập nhật website khi có bài mới

Hướng dẫn này giúp vợ thêm bài mà vẫn giữ nguyên các bài và bài làm cũ. Mỗi lần thêm bài, chỉ cần thêm tệp bài mới và sửa mục lục cùng trang bài trước. Không xóa thư mục website và không thay toàn bộ mã nguồn.

## 1. Hiểu ba kiểu thay đổi

- **THÊM**: chép tệp mới vào thư mục đã chỉ định. Các tệp cũ vẫn giữ nguyên.
- **CẬP NHẬT**: chỉ sửa đúng tệp được nêu. Tạo bản sao trước khi sửa để có thể quay lại.
- **GIỮ NGUYÊN**: không sửa tệp đó.

Thầy sẽ luôn gửi danh sách này cùng mỗi bài mới.

## 2. Thứ tự mở bài trong website

Mỗi bài mới cần đủ bốn phần:

1. Tệp bài học mới, ví dụ `lessons/0003-present-simple-continuous.html`.
2. Một dòng bài đang khóa trong `lessons/index.html`.
3. ID của bài trước để website biết khi nào mở bài mới.
4. iểm tra tiến độ ngay trong tệp bài mới, để người học không thể mở nội dung bằng cách gõ thẳng URL.

Mục lục đã có sẵn hàm mở bài chung. Khi thêm Bài 04, Bài 05…, không cần viết lại hàm đó; chỉ thêm đúng thông tin vào dòng bài đang khóa.

## 3. Thêm dòng bài đang khóa vào mục lục

Trong `lessons/index.html`, tìm nhóm bài đúng chặng và thêm một `<li>` cho bài mới. Ví dụ, Bài 03 chỉ mở sau khi Bài 02 đạt:

```html
<!-- Bài 03 sẽ được mở sau khi Bài 02 hoàn thành -->
<li>
  <span class="soon"
    data-href="0003-present-simple-continuous.html"
    data-requires="lesson02-present-simple">
    <span class="badge soon">Bài 03</span>
    Phân biệt Present Simple và Present Continuous
    <span class="lcount">sắp tới</span>
  </span>
</li>
```

Hai thuộc tính cần điền chính xác:

- `data-href`: tên đường dẫn của tệp bài học mới. Tệp phải tồn tại trước khi thêm đường dẫn.
- `data-requires`: ID bài ngay trước đó, ví dụ `lesson02-present-simple`.

Khi ID tiên quyết xuất hiện trong `completedLessonIds` ở trình duyệt, hàm chung sẽ đổi `span.soon` thành liên kết `<a>), đổi badge từ `soon`sang`now`, rồi hiện “Tiếp tục →”. Không thêm sẵn một liên kết ẩn bên cạnh `span\`; hai phần tử đó dễ cùng hiện do CSS.

## 4. Tạo tệp bài học mới

1. Sao chép một bài gần nhất, ví dụ `lessons/0002-present-simple.html`.
2. Đổi tên bản sao theo số bài và chủ đề, ví dụ `0003-present-simple-continuous.html`.
3. Đổi tiêu đề, nội dung, câu hỏi, bài tập và các đường dẫn điều hướng.
4. Giữ lại hai asset dùng chung:

```html
<link rel="stylesheet" href="lesson.css">
<script src="lesson-enhance.js"></script>
```

5. Đổi ID bài trước trong phần kiểm tra khóa. Mẫu của Bài 03:

```js
// Bài 03 chỉ hiện khi tiến độ có ID hoàn thành của Bài 02.
const PREVIOUS_LESSON_ID = "lesson02-present-simple";

function unlockLessonIfReady(progress) {
  const completedIds = progress.completedLessonIds || [];
  const isUnlocked = completedIds.includes(PREVIOUS_LESSON_ID);

  document.getElementById("locked-message").hidden = isUnlocked;
  document.getElementById("lesson").hidden = !isUnlocked;
  return isUnlocked;
}

function initializeLesson() {
  const progress = loadProgress();
  if (!unlockLessonIfReady(progress)) return;

  // Đặt phần còn lại của logic bài học ở dưới đây.
}
```

Giữ cấu trúc ID nhất quán:

- Bài 01: `lesson01-present-continuous`
- Bài 02: `lesson02-present-simple`
- Bài 03: `lesson03-<chu-de>`

## 5. Cho bài mới ghi nhận hoàn thành

Trong hàm tự chấm bài, chỉ thêm ID bài hiện tại sau khi bài làm đạt tiêu chí đã đặt ra. Ví dụ với Bài 03:

```js
// Chỉ tăng tiến độ nếu bài đạt yêu cầu.
if (passed && !completedIds.includes("lesson03-present-simple-continuous")) {
  completedIds.push("lesson03-present-simple-continuous");
}

progress.completedLessonIds = completedIds;
localStorage.setItem(PROGRESS_KEY, JSON.stringify(progress));
```

Không ghi sẵn ID hoàn thành vào HTML hay mục lục. `localStorage` lưu tiến độ riêng trong trình duyệt của mỗi người, nên việc cập nhật source không làm bài của người khác tự hoàn thành.

## 6. Nối từ bài trước sang bài mới

Trong bài trước, đổi nút “Sau” từ phần tử không có đường dẫn thật sang liên kết:

```html
<a href="0003-present-simple-continuous.html">Sau →</a>
```

Trang bài mới vẫn phải có bước kiểm tra ID tiên quyết ở mục 4. Vì vậy, nếu người học mở đường dẫn trước khi đạt bài trước, trang mới chỉ hiện lời nhắc quay lại.

## 7. Danh sách tệp mỗi lần thêm bài

Ví dụ khi thêm Bài 03:

| Thao tác | Tệp | Lý do |
| --- | --- | --- |
| THÊM | `lessons/0003-present-simple-continuous.html` | Nội dung bài mới, bài tập và tự chấm. |
| CẬP NHẬT | `lessons/index.html` | Thêm dòng khóa có `data-href` và `data-requires`. |
| CẬP NHẬT | `lessons/0002-present-simple.html` | Nối nút “Sau” tới Bài 03. |
| GIỮ NGUYÊN | `lessons/lesson.css`, `lessons/lesson-enhance.js` | Dùng lại giao diện và quiz chung. |

## 8. Cập nhật bằng File Explorer

1. Tạo bản sao thư mục website trước khi sửa, ví dụ `ielts-backup`.
2. Tải gói cập nhật thầy gửi và giải nén vào một thư mục tạm riêng.
3. Đọc bảng THÊM / CẬP NHẬT / GIỮ NGUYÊN đi kèm.
4. Chép tệp **THÊM** vào đúng thư mục trong website.
5. Với tệp **CẬP NHẬT**, chỉ chép đè đúng các tệp được nêu. Nếu muốn tự sửa tay, mở tệp trong editor và chỉ áp dụng các đoạn thay đổi được hướng dẫn.
6. Giữ các tệp **GIỮ NGUYÊN** như cũ.
7. Mở mục lục bằng Live Server. Khi thay đổi HTML/CSS/JS mà trình duyệt còn hiện bản cũ, tải lại bằng **Ctrl + F5**.

Gói cập nhật nhỏ chỉ chứa tệp cần thêm hoặc sửa. `english-foundation-source.zip` là bản đầy đủ để dự phòng; không cần giải nén đè lên thư mục đang học.

## 9. Khi website đã xuất bản

Sau khi cập nhật các tệp trên máy, cần triển khai phiên bản mới lên dịch vụ hosting để website online nhận thay đổi. Tiến độ vẫn riêng theo trình duyệt; muốn đồng bộ nhiều thiết bị hoặc nhiều người học thì cần đăng nhập và cơ sở dữ liệu.

## Việc thầy gửi cùng mỗi bài mới

Thầy sẽ nêu rõ tệp nào **THÊM / CẬP NHẬT / GIỮ NGUYÊN**, chỉ ra đoạn cần sửa khi vợ muốn tự cập nhật bằng tay, và gửi gói source thay đổi nhỏ cùng bản đầy đủ dự phòng.