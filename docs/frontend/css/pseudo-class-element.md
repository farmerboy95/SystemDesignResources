# Pseudo Class và Pseudo Element

## Định nghĩa

Pseudo Class là một bổ sung dành cho CSS selector để chọn các phần tử dựa trên trạng thái của phần tử đó trên cây DOM. Ví dụ, ta có thể chọn các input đang bị disable.

Pseudo Element cũng tương tự, nhưng cho phép ta chọn một số phần nhất định của phần tử. Ví dụ, ta có thể chọn chữ cái đầu tiên hoặc dòng đầu tiên của một đoạn văn (paragraph).

## Ví dụ

### Pseudo Class

Ta có trang web sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        /* Fill your style here */
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <p>First paragraph in this example</p>
    <p>Second paragraph in this example</p>

    <label for="input">Input: </label>
    <input type="text" id="input">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Trong ví dụ này ta sẽ có một header h1 chứa text "Hello World", một dòng chữ "Go to Homepage" với một link đến trang chủ, hai dòng tiếp theo chứa text "First paragraph in this example" và "Second paragraph in this example", cuối cùng là một input text với một label nằm trước.

Đầu tiên, hãy đi thay đổi màu của chữ "Homepage" trong dòng "Go to Homepage" xem sao. Ta sẽ chọn nó với pseudo class `:link`, có thể thêm tag `a` vào trước đó cho chắc. Lưu ý là pseudo class `:link` chỉ hoạt động khi link chưa được click.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        a:link {
            color: red;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <p>First paragraph in this example</p>
    <p>Second paragraph in this example</p>

    <label for="input">Input: </label>
    <input type="text" id="input">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Nếu bạn chưa click vào link đó, nó sẽ có màu đỏ. Ngược lại, nó sẽ không có thay đổi gì. Đó là vì pseudo class `:link` chỉ hoạt động khi link chưa được click, pseudo class này sẽ không chọn link đã được click. Ở đây ta có thể dùng pseudo class `:visited` để chọn link đã được click.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <p>First paragraph in this example</p>
    <p>Second paragraph in this example</p>

    <label for="input">Input: </label>
    <input type="text" id="input">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Tiếp theo ta sẽ nghịch với input một chút, ta muốn chọn input khi nó đang được focus, nghĩa là khi người ta sử dụng cái input đó, có thể là khi click vào, hoặc tab vào, hoặc với các công nghệ hỗ trợ khác. Ta sẽ dùng pseudo class `:focus` để chọn input đó. Thông thường khi focus, viền (outline) của input sẽ đậm lên, ta sẽ làm cho nó đậm lên tí nữa

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        input:focus {
            outline: 2px solid blue;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <p>First paragraph in this example</p>
    <p>Second paragraph in this example</p>

    <label for="input">Input: </label>
    <input type="text" id="input">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Giờ khi bạn focus vào input, viền của nó sẽ thành màu xanh đậm.

Tiếp theo, ta muốn đổi viền khi input bị không hợp lệ, tùy vào cái mà ta set trong HTML, ví dụ như độ dài input chỉ là 5 khi `minlength="6"` chẳng hạn. Ta sẽ dùng pseudo class `:invalid` để chọn input đó, làm nó đậm hơn một chút

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <p>First paragraph in this example</p>
    <p>Second paragraph in this example</p>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ok, khi này thì nếu bạn nhập ít hơn 6 ký tự vào input, nó sẽ có viền màu đỏ đậm. Nhưng cái này trông có vẻ hơi kỳ, vì bạn đang nhập dữ liệu mà nó lại có màu đỏ, chỉ vì chưa đủ số ký tự cần thiết, nghĩa là chưa nhập xong ấy. Ta không muốn việc này xảy ra với người dùng. Lý do vì sao nhỉ, hãy xem lại CSS của chùng ta.

```css
input:focus {
    outline: 2px solid blue;
}
input:invalid {
    outline: 2px solid red;
}
...
```

Ở trên cùng ta có `:focus` để làm viền có màu xanh đậm, ở dưỡi thì ta có `:invalid` để làm viền có màu đỏ đậm. Khi đang nhập mà nhập ít hơn 6 ký tự, nó sẽ thỏa cả focus và invalid. CSS sẽ ưu tiên chọn cái nào xuất hiện sau, nghĩa là cái invalid sẽ được ưu tiên hơn nếu cả hai đều thỏa mãn. Ta có thể đảo vị trí của hai selector để giải quyết nhưng có cách tốt hơn. Ta muốn tránh việc phụ thuộc vào thứ tự xuất hiện của selector, nên cách giải quyết ở đây là thêm một cái pseudo class nữa. Ta sẽ nối thêm vào `:invalid` một pseudo class để chọn những input không focus, với `:not(:focus)`. Đúng rồi, ta có thể nối tiếp nhiều pseudo class với nhau như một bộ lọc, lọc ra một số thứ sau pseudo class trước đó.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <p>First paragraph in this example</p>
    <p>Second paragraph in this example</p>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Giờ bạn sẽ chỉ nhìn thấy màu đỏ khi input không hợp lệ và không focus.

Tiếp theo ta có thể chọn element dựa trên vị trí của nó trong cây DOM. Ta muộn chọn các thẻ paragraph, nhưng chỉ chọn thẻ đầu tiên. Ta sẽ dùng pseudo element `:first-of-type` để chọn thẻ đầu tiên. Cho nó lên đầu style nhé. Biến nó thành màu cam đi.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p:first-of-type {
            color: orange;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <p>First paragraph in this example</p>
    <p>Second paragraph in this example</p>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Lưu ý `:first-of-type` chỉ chọn thẻ đầu tiên với các thẻ có chung parent, nghĩa là nó sẽ không chọn thẻ đầu tiên của toàn bộ trang web, mà chỉ chọn thẻ đầu tiên của các thẻ con của parent của nó. Ví dụ ta bọc hai thẻ `<p>` ở sau vào một thẻ `<div>` thì thẻ `<p>` đầu tiên trong đó sẽ chuyển thành màu cam do nó là thẻ `<p>` đầu tiên của thẻ `<div>` (thẻ cha)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p:first-of-type {
            color: orange;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Có `:first-of-type` thì ta cũng có thể có `:last-of-type`, thử thay vào xem sao

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p:last-of-type {
            color: orange;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta thấy thẻ chứa link và thẻ thứ 2 trong `<div>` chuyển thành cam, điều này đúng vì thẻ chứa link là thẻ `<p>` cuối cùng trong `<body>`, còn thẻ thứ 2 trong `<div>` là thẻ `<p>` cuối cùng trong `<div>`.

Ngoài ra ta có thể dùng `:nth-of-type()` để chọn thẻ theo vị trí cụ thể trên cây DOM. Ví dụ, muốn giống như `:first-of-type`, ta dùng `:nth-of-type(1)`, muốn chọn thẻ thứ 2, ta dùng `:nth-of-type(2)`. Ta còn có thể dùng `:nth-of-type(even)` hoặc `:nth-of-type(odd)` để chọn thẻ chẵn hoặc lẻ. Hoặc điên hơn thì có thể dùng `:nth-of-type(2n+1)` để chọn thẻ lẻ, `:nth-of-type(2n)` để chọn thẻ chẵn. Mình sẽ để bạn tự mày mò với code dưới đây, bạn có thể bấm vào Try the code để thử.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p:nth-of-type(2) {
            color: orange;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

Ta có một pseudo class nữa tương tự `:nth-of-type` nhưng hơi khác một chút là `:first-child`, thử xem sao

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p:first-child {
            color: purple;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ủa, sao chỉ có thẻ `<p>` trong `<div>` là đổi màu? Vì `:first-child` chỉ chọn thẻ đầu tiên của parent của nó, không quan trọng thẻ đó là thẻ gì. Ở đây, trong thẻ `<body>`, thẻ con đầu tiên của nó là `<h1>`, không phải `<p>`, nên thẻ có link không được chọn. Ngoài ra ta cũng có `:last-child`, `:nth-child()` nữa, hoạt động tương tự cái trước đó. Mình sẽ để code ở dưới để các bạn thử nghiệm nhé.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p:nth-child(1) {
            color: purple;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

Ngoài ra ta có rất nhiều loại Pseudo class khác, các bạn có thể tự tìm hiểu nhưng ở đây ta đã trải nghiệm một số pseudo class phổ biến nhất rồi.

### Pseudo Element

Ta tiếp tục với ví dụ trước đó. Với Pseudo element thì ta dùng hai dấu `:` thay vì một dấu `:` như Pseudo class. Ví dụ, ta muốn chọn chữ cái đầu tiên của thẻ `<p>`, ta sẽ dùng `::first-letter`. Ta sẽ làm chữ cái đầu tiên của thẻ `<p>` to lên.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p::first-letter {
            font-size: 2em;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ngoài ra ta còn có `::first-line` để chọn dòng đầu tiên, nhưng ở đây thẻ nào cũng có một dòng nên ta sẽ không dùng.

Với pseudo element, ta rất hay sử dụng hai pseudo element `::before` và `::after` để thêm nội dung vào trước và sau một phần tử. Ví dụ, ta muốn thêm dấu `->` vào trước mỗi thẻ `<p>`, ta sẽ làm như sau

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p::before {
            content: "->";
        }
        p::first-letter {
            font-size: 2em;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Dấu gạch có vẻ to. Lý do là vì cái `::first-letter` sau đó đã làm cho chữ cái đầu tiên của thẻ, ở đây là dẫu gạch nằm trong `::before`, to lên. Tiếp theo ta dùng pseudo element `::after` để thêm một dấu chấm than vào sau mỗi thẻ `<p>`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p::before {
            content: "->";
        }
        p::after {
            content: "!";
        }
        p::first-letter {
            font-size: 2em;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a>
    </p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta thấy rằng có một khoảng trống nhỏ nằm sau cái link, đây là do trong thẻ `<p>` chứa link, ta xuống dòng sau khi viết link, nên nó sẽ có một whitespace ở đây (do nhiều whitespace sẽ tụ lại thành một khoảng trống duy nhất). Đây là một trong những lần hiếm hoi mà whitespace này xuất hiện. Muốn nó mất đi thì ta chỉ cần mang thẻ đóng của link lên ngay sau thẻ đóng của thẻ `<a>`. Mình cũng để Try the Code ở dưới để các bạn thử.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p::before {
            content: "->";
        }
        p::after {
            content: "!";
        }
        p::first-letter {
            font-size: 2em;
        }
        input:focus {
            outline: 2px solid blue;
        }
        input:invalid:not(:focus) {
            outline: 2px solid red;
        }
        a:link {
            color: red;
        }
        a:visited {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Hello World</h1>
    <p>
        Go to
        <a href="/">Homepage</a></p>

    <div>
        <p>First paragraph in this example</p>
        <p>Second paragraph in this example</p>
    </div>

    <label for="input">Input: </label>
    <input type="text" id="input" minlength="6">
</body>
</html>
```
<apprun-play style="height:300px"></apprun-play>