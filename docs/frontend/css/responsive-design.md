# Thiết kế responsive với CSS

Trong bài này, ta sẽ tìm hiểu cách thiết kế responsive với CSS. Responsive design là một phần quan trọng của thiết kế web, giúp trang web hiển thị đẹp trên mọi thiết bị. Ta có ví dụ sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas:
                "header header header"
                "main aside nav"
                "footer footer footer";
            padding: 1em;
            gap: 10px;
        }
        header {
            grid-area: header;
        }
        main {
            grid-area: main;
        }
        aside {
            grid-area: aside;
        }
        footer {
            grid-area: footer;
        }
        body > * {
            border: 5px solid lightblue;
            padding: 5px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
    <nav>
        <p>Navigation content</p>
    </nav>
    <main>
        <h2>Main Heading</h2>
        <p>Main content paragraph</p>
    </main>
    <aside>
        <p>Aside content</p>
    </aside>
    <footer>
        <p>Footer content</p>
    </footer>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta có một layout giống như layout trong phần Grid layout. Ta có một phần header, main, aside, nav và footer. Ta sử dụng `grid-template-areas` để xác định vị trí của các phần. Ta cũng sử dụng `grid-template-columns` và `grid-template-rows` để xác định kích thước của các cột và hàng. Ta cũng sử dụng `gap` để tạo khoảng cách giữa các phần. Mỗi phần có viền xanh nhạt và padding 5px.

Vấn đề ở đây là layout này không hợp lý lắm khi màn hình nhỏ lại, ví dụ như màn hình điện thoại, bề ngang sẽ nhỏ hơn.

Đầu tiên, ta cần meta tag. Ta sẽ dùng `<meta name="viewport" content="width=device-width, initial-scale=1.0">` để nói rằng viewport sẽ thay đổi, và chiều rộng của nó sẽ bằng chiều rộng của thiết bị.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas:
                "header header header"
                "main aside nav"
                "footer footer footer";
            padding: 1em;
            gap: 10px;
        }
        header {
            grid-area: header;
        }
        main {
            grid-area: main;
        }
        aside {
            grid-area: aside;
        }
        footer {
            grid-area: footer;
        }
        body > * {
            border: 5px solid lightblue;
            padding: 5px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
    <nav>
        <p>Navigation content</p>
    </nav>
    <main>
        <h2>Main Heading</h2>
        <p>Main content paragraph</p>
    </main>
    <aside>
        <p>Aside content</p>
    </aside>
    <footer>
        <p>Footer content</p>
    </footer>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Giờ là lúc để viết responsive CSS. Ta sẽ cần media query, đây là cách để chạy một đoạn CSS dựa trên một điều kiện nào đó, thường là kích thước nhỏ nhất hay kích thước lớn nhất của màn hình, nhưng có rất nhiều điều kiện khác nữa, ta sẽ chỉ tập trung vào điều kiện kích thước màn hình thôi. Ví dụ ta sẽ viết `@media (max-width: 600px) {}` để chạy CSS trong đó khi chiều rộng màn hình nhỏ hơn 600px. Khi này ta sẽ muốn có 1 cột và 5 hàng, lần lượt cho các phần.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas:
                "header header header"
                "main aside nav"
                "footer footer footer";
            padding: 1em;
            gap: 10px;
        }
        header {
            grid-area: header;
        }
        main {
            grid-area: main;
        }
        aside {
            grid-area: aside;
        }
        footer {
            grid-area: footer;
        }
        body > * {
            border: 5px solid lightblue;
            padding: 5px;
        }
        @media (max-width: 600px) {
            body {
                grid-template-columns: 1fr;
                grid-template-rows: 1fr 1fr 3fr 1fr 1fr;
                grid-template-areas:
                    "header"
                    "nav"
                    "main"
                    "aside"
                    "footer";
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
    <nav>
        <p>Navigation content</p>
    </nav>
    <main>
        <h2>Main Heading</h2>
        <p>Main content paragraph</p>
    </main>
    <aside>
        <p>Aside content</p>
    </aside>
    <footer>
        <p>Footer content</p>
    </footer>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Khi thu nhỏ màn hình lại bạn sẽ thấy layout sẽ có 1 cột và 5 hàng.

Tất nhiên ta có thể thêm nhiều media query cho các điều kiện khác nhau. Ví dụ khi `max-width: 400px` ta muốn giấu đi aside, và nhớ rằng vẫn phải thêm body vào, không thì nó sẽ dùng giá trị mặc định.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas:
                "header header header"
                "main aside nav"
                "footer footer footer";
            padding: 1em;
            gap: 10px;
        }
        header {
            grid-area: header;
        }
        main {
            grid-area: main;
        }
        aside {
            grid-area: aside;
        }
        footer {
            grid-area: footer;
        }
        body > * {
            border: 5px solid lightblue;
            padding: 5px;
        }
        @media (max-width: 600px) {
            body {
                grid-template-columns: 1fr;
                grid-template-rows: 1fr 1fr 3fr 1fr 1fr;
                grid-template-areas:
                    "header"
                    "nav"
                    "main"
                    "aside"
                    "footer";
            }
        }
        @media (max-width: 400px) {
            body {
                grid-template-rows: 1fr 1fr 3fr 1fr;
                grid-template-areas:
                    "header"
                    "nav"
                    "main"
                    "footer";
            }
            aside {
                display: none;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
    <nav>
        <p>Navigation content</p>
    </nav>
    <main>
        <h2>Main Heading</h2>
        <p>Main content paragraph</p>
    </main>
    <aside>
        <p>Aside content</p>
    </aside>
    <footer>
        <p>Footer content</p>
    </footer>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Như vậy khi chiều rộng màn hình nhỏ hơn 400px, aside sẽ bị giấu đi.

Cuối cùng ta có một khái niệm gọi là Mobile First Design. Đó nghĩa là ta sẽ viết CSS cho các màn hình nhỏ trước, rồi dùng media query để hỗ trợ các màn hình lớn hơn. Điều này ngược lại với thứ ta làm nãy giờ, nghĩa là thực hiện viết CSS cho màn hình lớn, rồi dùng media query để hỗ trợ màn hình nhỏ.

Có hai lợi ích của Mobile First Design. Một là khi nó được tối ưu cho điện thoại, nó sẽ scale một cách tự nhiên cho các loại màn hình lớn, tốt hơn là khi scale ngược lại. Hai là điện thoại ngày càng phổ biến, thị phần lớn hơn máy tính, nên sẽ có lý khi thiết kế cho điện thoại trước.

Ta có thể thực hiện bằng cách đưa CSS cho màn hình nhỏ lên thành mặc định, và đưa CSS màn hình lớn vào media query. Ví dụ,

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 1fr;
            grid-template-rows: 1fr 1fr 3fr 1fr 1fr;
            grid-template-areas:
                "header"
                "nav"
                "main"
                "aside"
                "footer";
            
            padding: 1em;
            gap: 10px;
        }
        header {
            grid-area: header;
        }
        main {
            grid-area: main;
        }
        aside {
            grid-area: aside;
        }
        footer {
            grid-area: footer;
        }
        body > * {
            border: 5px solid lightblue;
            padding: 5px;
        }
        @media (min-width: 800px) {
            body {
                grid-template-columns: 3fr 1fr 1fr;
                grid-template-rows: 1fr 3fr 1fr;
                grid-template-areas:
                    "header header header"
                    "main aside nav"
                    "footer footer footer";
            }
        }
        @media (max-width: 400px) {
            body {
                grid-template-rows: 1fr 1fr 3fr 1fr;
                grid-template-areas:
                    "header"
                    "nav"
                    "main"
                    "footer";
            }
            aside {
                display: none;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
    <nav>
        <p>Navigation content</p>
    </nav>
    <main>
        <h2>Main Heading</h2>
        <p>Main content paragraph</p>
    </main>
    <aside>
        <p>Aside content</p>
    </aside>
    <footer>
        <p>Footer content</p>
    </footer>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Kết quả không thay đổi gì, chỉ là cách viết CSS thôi.
