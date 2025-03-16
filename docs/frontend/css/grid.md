# Grid trong CSS

## Tổng quan

Trong phần trước, ta đã có dịp tìm hiểu về cách sử dụng Flexbox để tạo layout cho trang web. Flexbox giúp ta dễ dàng tạo layout theo chiều dọc hoặc chiều ngang, linh hoạt và dễ dàng điều chỉnh. Tuy nhiên, Flexbox không phải là giải pháp tốt nhất cho mọi trường hợp. Đôi khi, ta cần một công cụ mạnh mẽ hơn để tạo layout phức tạp hơn, và đó chính là Grid. Với Grid, ta có thêm quyền điều khiển để biết chính xác vị trí của mỗi phần tử trên trang web, đặc biệt là khi ta có nhiều hàng và cột hơn so với Flexbox. Ta có ví dụ sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Ta có một trang web đơn giản với 4 phần tử: header, main, aside và footer. Mỗi phần tử này có một khung viền màu xanh nhạt, lý do là vì mỗi element là một grid item và cách này sẽ giúp ta nhìn thấy các grid item dễ dàng hơn. Mục tiêu của chúng ta là đổi layout một chút. Cụ thể là đưa phần aside sang bên cạnh main. Tất nhiên ta có thể làm được với Flexbox nhưng sẽ hơi phức tạp hơn chút.

Ta sẽ set `display: grid` cho body.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Tương tự như `display: flex`, `display: grid` sẽ tạo ra một grid container. Mọi phần tử con của grid container sẽ trở thành grid item (chỉ có thẻ con trực tiếp mới thành grid container).

## grid-template-columns

`grid-template-columns` quy định số cột và kích thước của mỗi cột trong grid container. Ta sẽ ghi kích thước của mỗi cột vào đây, cách nhau bởi khoảng trống, ví dụ như `grid-template-columns: 100px 200px 100px` sẽ tạo ra 3 cột với kích thước tương ứng.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 100px 200px 100px;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Như vậy layout này sẽ có 3 cột, có chiều rộng lần lượt là 100px, 200px và 100px. Grid item thứ 4 (footer) sẽ tự động chuyển xuống dòng mới vì không còn chỗ trống nào trong hàng hiện tại.

Ta có thể sử dụng một đơn vị đặc biệt ở đây, gọi là `fr` (fraction). `fr` sẽ chia phần còn lại của grid container thành các phần bằng nhau, ví dụ như `grid-template-columns: 1fr 2fr 1fr` sẽ tạo ra 3 cột với tỉ lệ 1:2:1.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 1fr 2fr 1fr;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Có một vấn đề ở đây là ta sẽ không muốn có một cột nào quá nhỏ. Nếu người dùng có một trình duyệt bé bé và ta có nhiều cột trong trang web, 1 cột rộng 1fr nhưng có cột rộng 10fr, thì cột 1fr sẽ rất nhỏ. Ta cần đảm bảo rằng cột nhỏ nhất cũng không quá nhỏ. Ta sẽ sử dụng `minmax()` để giải quyết vấn đề này. Nó là hàm nhận vào 2 tham số, tham số thứ nhất là giá trị nhỏ nhất, tham số thứ hai là giá trị lớn nhất. Ví dụ như `grid-template-columns: minmax(100px, 1fr) 2fr minmax(100px, 1fr)` sẽ tạo ra 3 cột, cột 1 và cột 3 có chiều rộng tối thiểu là 100px, cột 2 chiếm 2 phần trong 4 phần còn lại.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: minmax(20px, 1fr) 2fr 1fr;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Quay lại với mục tiêu, ta muốn có phần header, tiếp đến là 1 hàng gồm 2 phần, main và aside, cuối cùng là footer, nên ta muốn 2 cột ở đây, 1 cột to hơn hẳn cột còn lại, ta sẽ dùng `grid-template-columns: 3fr 1fr`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

## grid-template-rows

`grid-template-rows` cũng tương tự như `grid-template-columns`, nhưng thay vì quy định số cột và kích thước của mỗi cột, nó quy định số hàng và kích thước của mỗi hàng trong grid container. Ta sẽ muốn có 3 hàng, hàng ở giữa sẽ lớn hơn hai hàng còn lại, ta sẽ dùng `grid-template-rows: 1fr 3fr 1fr`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Có một trick khá hay ho là nếu bạn dùng `1fr 1fr 1fr` (3 cái 1fr) thì bạn có thể dùng `grid-template-rows: repeat(3, 1fr)`.

Thêm một điều nữa, hàng và cột có thể được gọi chung là track. Một hàng hay một cột có thể được gọi là một track.

## grid-column và grid-row

Giờ ta có thể bắt đầu điều chỉnh vị trí các grid item. Bắt đầu với header. Ta muốn header chiếm toàn bộ hàng đầu tiên, nghĩa là với hàng thì là hàng đầu tiên, còn cột thì là cả hai cột. Có nhiều cách thực hiện, ta sẽ bắt đầu với `grid-column-start`, nghĩa là nó bắt đầu từ cột nào, mà "cột" ở đây là cái ngăn cách giữa các cột, bắt đầu từ 1 (là cái ngăn cách giữa cột đầu tiên và biên trái). Ta sẽ kết thúc ở cột 3 (cái ngăn cách giữa cột 2 và biên phải). Ta sẽ dùng `grid-column-start: 1` và `grid-column-end: 3`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
        }
        header {
            grid-column-start: 1;
            grid-column-end: 3;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Thêm 1 cách nữa là chỉ dùng `grid-column-start: span 2`, nghĩa là chiếm 2 cột.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
        }
        header {
            grid-column-start: span 2;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Một cách nữa là dùng `grid-column: 1 / 3`, nghĩa là bắt đầu từ cột 1 và kết thúc ở cột 3.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
        }
        header {
            grid-column: 1 / 3;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Tương tự với `grid-column`, ta cũng có `grid-row`, ví dụ nếu muốn đặt header từ đường ngang ngăn cách thư 2 thì

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: 3fr 1fr;
            grid-template-rows: 1fr 3fr 1fr;
        }
        header {
            grid-column: 1 / 3;
            grid-row: 2;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Mấy cái số 1, 2, 3 gì đấy kiểu khá khó nhớ đúng không? Ta có thể đặt cho mỗi đường ngăn cách một cái tên và dùng lại trong grid. Ví dụ với cột, ta sẽ đặt `grid-template-columns: [left] 3fr [middle] 1fr [right]`, nghĩa là cột ngăn cách đầu tiên sẽ có tên là `left`, cột ngăn cách thứ hai sẽ có tên là `middle`, cột ngăn cách cuối cùng sẽ có tên là `right`. Từ đây trong `grid-column` ta có thể dùng `grid-column: left / right`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
        }
        header {
            grid-column: left / right;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

## grid-template-areas

Ta còn một thuộc tính mạnh hơn để quản lý vị trí của các grid item, đó là `grid-template-areas`. Với `grid-template-areas`, ta có thể đặt tên cho mỗi grid item và sắp xếp chúng theo tên. Ví dụ, ta muốn header chiếm cả hàng đầu tiên, main và aside chiếm hàng thứ hai, footer chiếm hàng cuối cùng, ta sẽ dùng `grid-template-areas: "header header" "main aside" "footer footer"`. Mỗi dòng trong chuỗi đại diện cho một hàng, mỗi cột trong dòng đại diện cho một cột, và tên của grid item sẽ được đặt ở đây.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas: 
                "header header"
                "main aside"
                "footer footer";
        }
        header {
            grid-column: left / right;
        }
        body > * {
            border: 5px solid lightblue;
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Hình như chưa có thay đổi gì, nhưng bạn sẽ thấy trong phần tiếp theo đây.

## grid-area

Bạn có thể dùng tên trong `grid-template-areas` để đặt vị trí cho grid item. Ví dụ, ta muốn header chiếm cả hàng đầu tiên, ta sẽ dùng `grid-area: header` thay vì `grid-column: left / right`. Ta có thể đặt aside, main và footer luôn.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas: 
                "header header"
                "main aside"
                "footer footer";
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
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Đây thực sự là một công cụ rất tiện lợi. Đôi khi bạn muốn đổi aside sang phải thì chỉ cần đổi `grid-template-areas` một chút thành `grid-template-areas: "header header" "aside main" "footer footer";` là xong, quá tiện. Tất nhiên là phải chỉnh độ rộng một tí rồi, nhưng vẫn tiện hơn nhiều so với việc dùng Flexbox.

## gap

`gap` sẽ giúp ta tạo ra khoảng cách giữa các hàng và cột, lưu ý là dùng trong grid container nhé. Ví dụ, ta muốn có khoảng cách 10px giữa các hàng và cột, ta sẽ dùng `gap: 10px`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas: 
                "header header"
                "main aside"
                "footer footer";
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
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

## justify-items, align-items và place-items

`justify-items` và `align-items` sẽ giúp ta căn chỉnh vị trí của các grid item trong grid container. `justify-items` sẽ căn chỉnh theo chiều ngang, `align-items` sẽ căn chỉnh theo chiều dọc. Ví dụ với `justify-items`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas: 
                "header header"
                "main aside"
                "footer footer";
            gap: 10px;
            justify-items: center;
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
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Nếu ta dùng `justify-items: center` và `align-items: center` thì các grid item sẽ được căn giữa cả chiều ngang và chiều dọc.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas: 
                "header header"
                "main aside"
                "footer footer";
            gap: 10px;
            justify-items: center;
            align-items: center;
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
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Ngoài ra nếu `justify-items` và `align-items` chỉnh cùng một giá trị, ta có thể dùng `place-items` để viết ngắn gọn hơn. Ví dụ, `place-items: center` sẽ tương đương với `justify-items: center` và `align-items: center`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas: 
                "header header"
                "main aside"
                "footer footer";
            gap: 10px;
            place-items: center;
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
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

## justify-self, align-self và place-self

Tương tự với 3 thuộc tính trên, ta có thể dùng `justify-self`, `align-self` và `place-self` để căn chỉnh vị trí của từng grid item. Ví dụ, ta muốn header căn giữa cả chiều ngang và chiều dọc, ta sẽ dùng `justify-self: center` và `align-self: center`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas: 
                "header header"
                "main aside"
                "footer footer";
            gap: 10px;
        }
        header {
            grid-area: header;
        }
        main {
            justify-self: center;
            align-self: center;
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
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

`place-self` cũng tương tự với `place-items`, nếu `justify-self` và `align-self` giống nhau, ta có thể dùng `place-self` để viết ngắn gọn hơn.

## Grid lồng nhau

Ta cũng có thể lồng flexbox vào grid, lồng grid vào grid hay lồng grid vào flexbox đều được. Ví dụ ta cho main thành flex container.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        body {
            display: grid;
            grid-template-columns: [left] 3fr [middle] 1fr [right];
            grid-template-rows: 1fr 3fr 1fr;
            grid-template-areas: 
                "header header"
                "main aside"
                "footer footer";
            gap: 10px;
        }
        header {
            grid-area: header;
        }
        main {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
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
        }
    </style>
</head>
<body>
    <header>
        <h1>Top Level Heading</h1>
    </header>
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

Ta có thể thấy nội dung trong main giờ đã căn giữa cả chiều ngang và chiều dọc.

## Kết luận

Ta có thể dùng grid với các layout 2 chiều, và flexbox cho layout 1 chiều. Nếu muốn dùng flexbox cho layout 2 chiều, bạn cần phải cẩn thận hơn vì `flex-wrap` sẽ không có nhiều quyền kiểm soát như wrap sẽ xảy ra khi nào và element nào sẽ nằm trên dòng nào.