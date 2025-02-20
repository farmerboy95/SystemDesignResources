# DOM là gì?

DOM (Document Object Model) là một cấu trúc dữ liệu biểu diễn các thành phần của một trang web. Nó cung cấp một cách tiêu chuẩn để truy cập, thay đổi, thêm hoặc xóa các phần tử HTML.

Trình duyệt tiếp nhận file HTML như một file văn bản bình thường và chuyển đổi nó thành cây DOM, mà sau đó trình duyệt sử dụng để hiển thị trang web.

```
HTML -> DOM -> UI -> User
```

DOM có một số mục đích chính:

1. **Truy cập**: Bạn có thể truy cập bất kỳ phần tử nào trên trang web bằng cách sử dụng JavaScript.
2. **Thay đổi**: Bạn có thể thay đổi nội dung của bất kỳ phần tử nào trên trang web.

Với cấu trúc cây, mỗi phần tử và văn bản trên trang web được biểu diễn bằng một node trong cây.

Ví dụ, xét trang web sau:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>My webpage</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <p>
            Hello, <br />
            How are you?
        </p>
    </body>
</html>
```

Cây DOM tương ứng sẽ như sau:

```
html
├── head
│   └── title
│       └── "My webpage"
└── body
    ├── h1
    │   └── "Hello World!"
    └── p
        ├── "Hello, "
        └── br
        └── "How are you?"
```
