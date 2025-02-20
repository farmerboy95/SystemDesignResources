# Các thẻ meta trong HTML

Trong bài này, ta sẽ nói về thẻ `<head>` trong HTML và các thẻ meta phổ biến mà bạn có thể sử dụng để cung cấp thông tin về trang web của bạn cho trình duyệt và các công cụ tìm kiếm.

Đầu tiên, ta có thẻ `<title>`. Thẻ này chứa tiêu đề của trang web. Tiêu đề này sẽ hiển thị ở thanh tiêu đề của trình duyệt.

## Thẻ `<meta>`

Đối với các metadata khác trong thẻ `<head>`, ta có thẻ `<meta>`. Thẻ này không có thẻ đóng, và thường được sử dụng để cung cấp thông tin về trang web cho trình duyệt và các công cụ tìm kiếm.

Khi muốn cung cấp charset (bảng mã) cho trang web, ta có thể sử dụng thẻ `<meta charset="UTF-8">`. Điều này sẽ giúp trình duyệt hiểu được bảng mã mà trang web sử dụng.

Để cung cấp mô tả cho trang web, ta có thẻ `<meta name="description" content="Mô tả trang web">`. Mô tả này sẽ hiển thị trong kết quả tìm kiếm của công cụ tìm kiếm.

Để biểu thị tác giả của trang web, ta có thẻ `<meta name="author" content="Tên tác giả">`.

Để biểu thị viewport (khung nhìn) của trang web, ta có thẻ `<meta name="viewport" content="width=device-width, initial-scale=1.0">`. Điều này giúp trang web hiển thị đúng trên các thiết bị di động.

## Favicon

Favicon là biểu tượng nhỏ hiển thị ở thanh địa chỉ của trình duyệt. Để thêm favicon, ta cần tạo một file ảnh có kích thước 16x16 hoặc 32x32 và thêm thẻ `<link rel="icon" href="favicon.ico" type="image/x-icon">` vào thẻ `<head>`.

## Thẻ `<base>`

Thẻ `<base>` được sử dụng để xác định URL cơ sở cho tất cả các URL tương đối trong trang web. Thẻ này không có thẻ đóng, và có thể được sử dụng như sau: `<base href="https://example.com/">`.

Sau khi set, tất cả các URL tương đối trong trang web sẽ được hiểu là URL cơ sở kết hợp với URL tương đối đó. Ví dụ, nếu ta có một thẻ `<a href="/about">`, thì URL cuối cùng sẽ là `https://example.com/about`.
