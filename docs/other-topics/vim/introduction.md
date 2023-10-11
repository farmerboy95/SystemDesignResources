# Giới thiệu về Vim

## Vim là gì?

**Vim** (hay đầy đủ là **V**i **IM**proved) là một trình soạn thảo văn bản bao gồm gần như tất cả các chức năng của trình soạn thảo `vi` trên Unix, cùng với rất nhiều tính năng mới. Vim rất hữu ích để soạn thảo văn bản, lập trình, vân vân và mây mây.

Tất cả các lệnh trong Vim đều có thể được thực hiện bằng bàn phím. Đây là một lợi thế lớn khi bạn có thể sử dụng Vim trên các thiết bị không có chuột như máy chủ từ xa. Tất nhiên là vẫn có phiên bản cho phép dùng chuột và có giao diện đồ họa nhưng nó không nằm trong phạm vi của series này.

Series về Vim này sẽ dựa trên help file chính thức của Vim tại [https://vimhelp.org/]. Bạn cũng có thể truy cập help file này bằng cách gõ `:help` trong Vim.

Trong series này, chúng ta sẽ chỉ tập trung vào Vim, không phải Vi của Unix. Bạn có thể xem những khác biệt giữa hai chương trình này tại [đây](https://vimhelp.org/vi_diff.txt.html#vi_diff.txt).

## Notation

## Các chế độ

Vim có 7 chế độ cơ bản:

| Chế độ | Mô tả |
| --- | --- |
| Normal mode | Bạn có thể dùng tất cả các lệnh normal trong chế độ này. Đây cũng là chế độ mặc định khi bạn vừa vào Vim. Normal mode cũng thường được gọi là Command mode. |
| Visual mode | Visual mode cũng giống như Normal mode, nhưng các lệnh di chuyển con trỏ sẽ mở rộng / thu hẹp vùng chọn văn bản. Khi một lệnh không liên quan đến di chuyển con trỏ được kích hoạt, hiệu ứng sẽ xảy ra với vùng chọn. Thường bạn sẽ thấy `-- VISUAL --` ở dưới màn hình khi ở chế độ này. |

- Normal mode
- Visual mode
- Select mode
- Insert mode
- Command-line mode
- Ex mode
- Terminal-Job mode

Ngoài ra còn có 7 chế độ khác. Đây là các biến thể của các chế độ cơ bản:

- Operator-pending mode
- Replace mode
- Virtual Replace mode
- Insert Normal mode
- Terminal-Normal mode
- Insert Visual mode
- Insert Select mode

## Chuyển giữa các chế độ

## Nội dung trong cửa sổ

## Các định nghĩa
