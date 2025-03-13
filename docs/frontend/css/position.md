# Position trong CSS

Trong bài này ta sẽ đi tìm hiểu về thuộc tính `position` trong CSS và cách mà nó đặt vị trí cho các element trên một trang web. Ta có ví dụ sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy một thẻ paragraph nằm trong khung màu xanh nhạt, một section chứa một khung chữ nhật xanh lá tên là box và một paragraph dài nằm trong một khung màu vàng. Khung xanh lá nhìn hơi lệch so với hai khung còn lại. Khung nào cũng có viền đen dày 2px.

Theo mặc định, tất cả các element này được sắp xếp vị trí theo kiểu static. Ta có thể thay đổi cách position này.

Bắt đầu với `fixed`, ta thử với `box` xem sao.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: fixed;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

`fixed` nghĩa là nó sẽ ra khỏi flow của trang web, các element tiếp sau sẽ được đôn lên thế chỗ. Ta sẽ thấy box che đi một phần của paragraph dài. Ta có thể dùng `top` để xác định khoảng cách của box từ cạnh trên trình duyệt xuống.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: fixed;
            top: 10px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Bạn cũng có thể scroll xuống để thấy rằng box vẫn ở nguyên vị trí vì nó đã được cố định (fixed) vào viewport. Ta cũng có thể dùng `left` để xác định khoảng cách từ cạnh trái của trình duyệt sang.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: fixed;
            top: 10px;
            left: 200px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ngoài ra ta cũng có các thuộc tính `right` và `bottom` hoạt động tương tự, ví dụ đổi `top` thành `bottom`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: fixed;
            bottom: 10px;
            left: 200px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Nhìn chung, ta nên tránh dùng `position: fixed` vì nó có thể che đi một số các element khác, tạo ra trải nghiệm người dùng không tốt. Tuy nhiên nó vẫn hữu ích trong một số trường hợp như menu điều hướng.

Tiếp theo là `relative`. Nó khá đơn giản, nó sẽ giữ vị trí của element đó trong flow của trang web, nhưng ta có thể dùng `top`, `right`, `bottom`, `left` để di chuyển nó so với vị trí mặc định. Ví dụ ta bỏ hết `bottom` và `left` ở trên và đổi `position` thành `relative`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: relative;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy nó về lại y như ban đầu ví dụ. Giờ ta thêm lại `bottom` và `left` vào `#box`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: relative;
            bottom: 10px;
            left: 200px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Như vậy nó sẽ di chuyển box lên 10px và qua phải 200px so với vị trí mặc định. Ta cũng có thể thử với `top` và `left`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: relative;
            top: 20px;
            left: 100px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Nó sẽ chạy xuống 20px và qua phải 100px so với vị trí mặc định. Ta cũng có thể sử dụng giá trị âm ở đây, ví dụ `left: -100px`, nhưng mà bạn có thể làm với số dương với `right`, nghĩa là dùng `right: 100px` thay vì `left: -100px`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: relative;
            top: 20px;
            left: -100px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Khi scroll thì ta thấy `position: relative` không bị nằm ngoài flow của trang web.

Tiếp đến là `sticky`. Đây là kiểu kết hợp giữa `relative` và `fixed`. Khi ta scroll xuống thì element sticky sẽ giữ nguyên vị trí. Ví dụ:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: sticky;
            top: 20px;
            left: 100px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Nó sẽ xuống dưới 20px và sang phải 100px so với vị trí ban đầu, khi scroll xuống thì nó cách cạnh trên viewport 20px. Bạn có thể dùng `sticky` cho menu điều hướng nhưng hãy cẩn thận để không che mất các element khác. Chú ý là các trình duyệt cũ có thể không hỗ trợ `sticky`.

Tiếp đến là `absolute`. Trong phần lớn trường hợp, nó hoạt động không khác gì `fixed`, nhưng khi scroll xuống thì nó không trôi theo trang.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            position: absolute;
            top: 20px;
            left: 100px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Điểm khác là, nếu element cha của `absolute` là một element `relative` gì đó, hoặc bất cứ tổ tiên nào là `relative`, thì cái `absolute` này sẽ xác định vị trí dựa vào element `relative` gần nhất. Ta thấy box nằm trong một `<section>`, ta có thể cho section này `relative` để thử nghiệm.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        section {
            position: relative;
        }
        #box {
            position: absolute;
            top: 20px;
            left: 100px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Giờ ta thấy box sẽ nằm trong section, cách cạnh trái của section 100px và cách cạnh trên của section 20px.

Box bây giờ nằm trên paragraph khá khó chịu phải không? Nếu ta không muốn nó nằm trên paragraph thì sao? Ta muốn các element khác nằm trên nó. Ta có thể dùng `z-index` để xác định thứ tự của các element. Theo mặc định thì `z-index` của các element là 0, chỉ số này có nghĩa là "ta muốn element nằm ở chiều cao x". Ta sẽ thử set `z-index` của paragraph dài là `2` nhé.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        section {
            position: relative;
        }
        #box {
            position: absolute;
            top: 20px;
            left: 100px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            z-index: 2;
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ủa? Không có gì thay đổi à? Lý do liên quan đến một thứ gọi là stacking context. Phần này sẽ được nói đến trong một bài khác, còn bây giờ ta chỉ cần biết rằng ta phải tạo một stacking context mỗi khi dùng `z-index`. Mà stacking context lại không tồn tại trong static element. Có nhiều cách tạo stacking context, cách dễ nhất là set `position: relative`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        section {
            position: relative;
        }
        #box {
            position: absolute;
            top: 20px;
            left: 100px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            position: relative;
            z-index: 2;
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ngon, giờ box đã nằm dưới paragraph dài. Nếu bạn không tin nó vẫn còn nằm đó, ta có thể thử set `top: -10px` cho box để nó lộ ra một chút.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        section {
            position: relative;
        }
        #box {
            position: absolute;
            top: -10px;
            left: 100px;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            position: relative;
            z-index: 2;
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Có một thuộc tính khá đáng chú ý là `float`. Ta tạm về lại như ban đầu nhé.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

`float` khá tương đồng với `position`, với khả năng bốc element ra ngoài flow của trang web. Nhưng nó cho phép element tham gia flow với các element khác. Khó hiểu thật, thử phát xem. Ta set `float: left` cho box.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            background-color: lightblue;
        }
        #box {
            float: left;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta có thể thấy là box giờ nằm trên paragraph nhưng chữ trong paragraph lại tránh box ra. `float` có nghĩa là mang cái element này và đẩy nó về một hướng của thẻ chứa nó. Trong trường hợp này, nó bị đẩy về bên trái, và cho phép các element tiếp theo wrap (bọc, hay tránh nó) khi có chỗ trống ở phía bên kia. Đây là công cụ cực kỳ mạnh mẽ, đặc biệt là khi tạo những thứ kiểu như blog, bạn muốn có hình ảnh gì đó nhưng không muốn cái hình chiếm hết chiều rộng trang web. Bạn có thể `float` chữ quanh cái hình đó và tạo ra một UI khá nghệ. Ta cũng có thể thực hiện với nhiều element cùng lúc, ví dụ như set `float: right` cho paragraph trên cùng.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            float: right;
            background-color: lightblue;
        }
        #box {
            float: left;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Giờ ta thấy chữ đã wrap cả hai float element rồi. Cuối cùng, đôi khi ta muốn element bỏ qua float, ta có thể sử dụng thuộc tính `clear`, nghĩa là cái element clear sẽ nằm dưới mấy thứ float trái phải ở trên, nên ta có thể dùng `clear: both` cho paragraph dài.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            float: right;
            background-color: lightblue;
        }
        #box {
            float: left;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            clear: both;
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta cũng có thể clear một bên, ví dụ bên phải với `clear: right`, và paragraph dài vẫn sẽ wrap box. Ta cũng có thể `clear: left`, nhưng do cái element bên trái nó dài hơn nên dùng nó trong trường hợp này không khác gì `clear: both`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            font-size: 1.75rem;
            margin: 1.5rem;
            padding: 1rem;
            border: 2px solid black;
        }
        body > p {
            float: right;
            background-color: lightblue;
        }
        #box {
            float: left;
            width: 15rem;
            height: 10rem;
            border: 2px solid black;
            background-color: lightgreen;
        }
        #long {
            clear: right;
            background-color: lightyellow;
        }
    </style>
</head>
<body>
    <p>I am a paragraph!</p>
    <section>
        <div id="box">Box</div>
        <p id="long">
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
            This is a very long paragraph with a lot of text.
        </p>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>