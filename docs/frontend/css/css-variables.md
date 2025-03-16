# Biến trong CSS

Trong bài này, ta sẽ cùng tìm hiểu biến trong CSS (hay được biết đến nhiều hơn với tên gọi Custom Properties). Đây là cách để gán giá trị cho một biến và sử dụng nó trong CSS, giúp giảm lặp code và dễ dàng thay đổi giá trị của biến. Lưu ý bài này chỉ nói về biến trong CSS được hỗ trợ bởi chính CSS, không phải biến trong CSS preprocessor như SASS hay LESS.

Ta có ví dụ sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        h1 {
            color: #00334C;
        }
        h2 {
            padding: 12px;
            color: white;
        }
        main {
            display: flex;
            border: 5px solid black;
            gap: 12px;
            width: 412px;
        }
        section {
            flex-basis: 200px;
            flex-shrink: 0;
            flex-grow: 0;
            background-color: #00334C;
        }
    </style>
</head>
<body>
    <header>
        <h1>CSS Variables</h1>
    </header>
    <main>
        <section>
            <h2>Section 1</h2>
        </section>
        <section>
            <h2>Section 2</h2>
        </section>
    </main>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta có một header và hai section, mỗi section có một h2. Ta thấy rằng màu của header và section giống nhau. Ta sẽ muốn đặt biến cho màu này. Ngoài ra `width: 412px` có vẻ ảo, nó chính là tổng của 2 lần `flex-basis: 200px` và `gap: 12px`, ta sẽ muốn tính toán giá trị này.

Ta sẽ tạo biến bằng cách đưa chúng vào trong pseudo class `:root`, lý do là vì ta muốn biến này có thể sử dụng ở bất kỳ đâu trong CSS. Bạn cũng có thể để biến vào `html` hay bất kỳ rule set nào khác, nhưng việc này không phổ biến lắm. Ta sẽ dùng `--` để bắt đầu tên biến, ví dụ `--main-color: #00334C`. Khi dùng ta chỉ cần viết `var(--main-color)` thay vì `#00334C`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        :root {
            --main-color: #00334C;
        }
        h1 {
            color: var(--main-color);
        }
        h2 {
            padding: 12px;
            color: white;
        }
        main {
            display: flex;
            border: 5px solid black;
            gap: 12px;
            width: 412px;
        }
        section {
            flex-basis: 200px;
            flex-shrink: 0;
            flex-grow: 0;
            background-color: var(--main-color);
        }
    </style>
</head>
<body>
    <header>
        <h1>CSS Variables</h1>
    </header>
    <main>
        <section>
            <h2>Section 1</h2>
        </section>
        <section>
            <h2>Section 2</h2>
        </section>
    </main>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Với `width`, ta sẽ tạo `--section-size: 200px` để thay cho `flex-basis`, tiếp đến là `--flex-gap: 12px` để thay cho `gap`, ta sẽ muốn tính lại `width` với `calc(var(--section-size) * 2 + var(--flex-gap))`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        :root {
            --main-color: #00334C;
            --section-size: 200px;
            --flex-gap: 12px;
        }
        h1 {
            color: var(--main-color);
        }
        h2 {
            padding: 12px;
            color: white;
        }
        main {
            display: flex;
            border: 5px solid black;
            gap: var(--flex-gap);
            width: calc(var(--section-size) * 2 + var(--flex-gap));
        }
        section {
            flex-basis: var(--section-size);
            flex-shrink: 0;
            flex-grow: 0;
            background-color: var(--main-color);
        }
    </style>
</head>
<body>
    <header>
        <h1>CSS Variables</h1>
    </header>
    <main>
        <section>
            <h2>Section 1</h2>
        </section>
        <section>
            <h2>Section 2</h2>
        </section>
    </main>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Thêm một vấn đề nữa, nếu ta đổi `:root` thành `main`, thì biến sẽ chỉ có thể sử dụng trong `main` thôi, phần `header` sẽ không bị ảnh hưởng, giả sử ta đổi màu `--main-color` thành đỏ thì màu của `header` sẽ không đổi.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        main {
            --main-color: red;
            --section-size: 200px;
            --flex-gap: 12px;
        }
        h1 {
            color: var(--main-color);
        }
        h2 {
            padding: 12px;
            color: white;
        }
        main {
            display: flex;
            border: 5px solid black;
            gap: var(--flex-gap);
            width: calc(var(--section-size) * 2 + var(--flex-gap));
        }
        section {
            flex-basis: var(--section-size);
            flex-shrink: 0;
            flex-grow: 0;
            background-color: var(--main-color);
        }
    </style>
</head>
<body>
    <header>
        <h1>CSS Variables</h1>
    </header>
    <main>
        <section>
            <h2>Section 1</h2>
        </section>
        <section>
            <h2>Section 2</h2>
        </section>
    </main>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>