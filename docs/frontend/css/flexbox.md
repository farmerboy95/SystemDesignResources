# Flexbox

## Tổng quan

Trong bài này ta sẽ đi tìm hiểu về CSS Flexbox, nó là một công cụ mạnh mẽ để xây dựng layout responsive, nghĩa là layout sẽ tự thay đổi kích thước dựa trên kích thước màn hình, tạo ra trải nghiệm người dùng tốt hơn mà không phụ thuộc vào thiết bị.

Ta có ví dụ sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ở đây ta có một section cao 90% chiều cao của viewport, có viền chấm, chứa 3 div con, mỗi div có một màu và chiều cao khác nhau, xếp từ trên xuống dưới, lần lượt là đỏ, xanh lá, xanh lam. Mỗi div con có viền đen, padding, màu chữ trắng, kích thước chữ to, đậm.

Flexbox cho phép ta tạo flexbox container rồi đến các flexbox item trong cái container đó. Để tạo container ta dùng `display: flex`. Ta sẽ dùng nó lên section trước. Ta dùng `display: flex` trên bất cứ cái gì quây quanh cái flex item, nghĩa là thứ ta muốn tạo layout. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Sau khi set, cả 3 div con đều trở thành flex item và sẽ thay đổi cách các item này được hiển thị. Lưu ý rằng, chỉ có các thẻ con trực tiếp mới trở thành flex item, các thẻ con của thẻ con sẽ không trở thành flex item (vì các thẻ này chỉ là thẻ con của flex item nên sẽ không được xem là flex item).

Giờ bạn sẽ thấy các div này được xếp theo chiều ngang và không chiếm toàn bộ khung section nữa, chúng chỉ chiếm chiều rộng của nội dung. Đây là mặc định của Flexbox, nhưng tất nhiên ta có rất nhiều cách để tùy chỉnh layout này.

## flex-direction

Thuộc tính `flex-direction` quy định hướng của các flex item trong flex container. Mặc định của nó là `row`, nghĩa là các flex item sẽ được xếp theo chiều ngang. Khi ta đặt `flex-direction: column` thì các flex item sẽ được xếp theo chiều dọc.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: column;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Đây là layout theo chiều dọc và chỉ có một element trên mỗi dòng, các element này giờ đã có khoảng trống để chiều rộng to ra đến hết container. Ta cũng có thể dùng `flex-direction: column-reverse` để xếp các flex item theo chiều dọc từ dưới lên trên (từ cuối section). Tương tự với `row-reverse` để xếp các flex item theo chiều ngang từ phải qua trái (từ bên phải section).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: column-reverse;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row-reverse;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta cũng cần hiểu `flex-direction` ở đây chính là trục chính (main axis) của flex container, nghĩa là trục mà các flex item sẽ được xếp theo. Nếu ta đặt `flex-direction: row` thì trục chính sẽ là trục Ox (ngang), còn `flex-direction: column` thì trục chính sẽ là trục Oy (dọc). Trục phụ (cross axis) sẽ là trục vuông góc với trục chính.

## justify-content

Giờ cho `flex-direction` về lại `row` nhé. `justify-content` quy định cách các flex item sẽ được căn chỉnh theo trục chính của flex container. Mặc định của nó là `flex-start`, nghĩa là các flex item sẽ được căn chỉnh về phía bên trái (hoặc phía trên nếu là `column`). Ta có thể hiểu `flex-start` là đầu của trục chính.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: flex-start;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Tương tự, `justify-content: flex-end` sẽ đẩy các div con về phía cuối trục chính.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: flex-end;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta cũng có thể dùng `justify-content: space-around` để tạo ra khoảng trống đều giữa các flex item, hoặc `justify-content: space-between` để tạo ra khoảng trống giữa các flex item nhưng không có khoảng trống ở đầu và cuối.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: space-around;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta có thể kết hợp `justify-content` và `flex-direction` để tạo ra các layout thú vị. Ví dụ, ta muốn trục chính là trục hoành, nhưng muốn đảo ngược thứ tự (xanh lam, xanh lá, đỏ), và các div nằm bên trái, thì ta dùng `flex-direction: row-reverse` và `justify-content: flex-end`. Nếu muốn trục chính là trục tung, ta cũng có thể làm tương tự với `column` hay `column-reverse`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row-reverse;
            justify-content: flex-end;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## align-items

Với trục phụ thì sao? `align-items` quy định cách các flex item sẽ được căn chỉnh theo trục phụ của flex container. Mặc định của nó là `stretch`, nghĩa là đầu của trục phụ. Ví dụ, ta dùng `flex-direction: row`, `justify-content: flex-end` và `align-items: flex-end`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: flex-end;
            align-items: flex-end;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy các div nằm góc dưới phải của section. Ta cũng có thể dùng `align-items: center` để đưa các div về giữa trục phụ, nhưng vẫn nằm cuối trục chính.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: flex-end;
            align-items: center;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Đến đây thì ta có cách đưa các div về giữa cả trục chính và trục phụ, ta dùng `justify-content: center` và `align-items: center`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: center;
            align-items: center;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## gap

Các div nằm giữa section rồi mà có vè như ta muốn chúng có tí khoảng cách. `gap` là thuộc tính của Flexbox cho phép ta quy định khoảng cách giữa các flex item theo trục chính. Nó kiểu gần như `margin` vậy, điểm khác là nó chỉ là khoảng cách giữa các flex item, hơn là việc bạn phải set margin cho từng flex item.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: center;
            align-items: center;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## flex-wrap

Ta có một layout khá đẹp nằm gọn trong section, nhưng chuyện gì sẽ xảy ra nếu ta có quá nhiều flex item? Thử nhân 3 div con lên thành 9 div con xem sao.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            justify-content: center;
            align-items: center;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy chúng bị tràn ra ngoài, thậm chí div màu đỏ đầu tiên còn không thể thấy được. Để giải quyết vấn đề này, ta dùng `flex-wrap`. `flex-wrap` quy định xem các flex item có thể xuống dòng hay không. Mặc định của nó là `nowrap`, nghĩa là các flex item sẽ không xuống dòng. Ta dùng `flex-wrap: wrap` để cho phép các flex item xuống dòng.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Hoặc ta có thể dùng `flex-wrap: wrap-reverse` để xuống dòng từ dưới lên trên (trong trường hợp này gọi là lên dòng thì chuẩn hơn).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            flex-direction: row;
            flex-wrap: wrap-reverse;
            justify-content: center;
            align-items: center;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## flex-flow

Ta có thể kết hợp `flex-direction` và `flex-wrap` thành một thuộc tính duy nhất là `flex-flow`. `flex-flow` nhận 2 giá trị, đầu tiên là `flex-direction`, thứ hai là `flex-wrap`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## align-content

Giờ ta có các div nằm trên nhiều dòng khác nhau, ta muốn xác định xem khoảng cách giữa các dòng là bao nhiêu. `align-items` chỉ quy định khoảng cách giữa các flex item trên cùng một dòng, còn `align-content` quy định khoảng cách giữa các dòng. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: center;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Giờ các dòng đã thu về giữa section. Bạn cũng có thể dùng `align-content: space-around` hoặc `align-content: space-between` để các dòng cách đều nhau (và cả với biên) và không có khoảng trống ở đầu và cuối.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-around;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## order

Đôi khi ta muốn một flex item nào đó xuất hiện trước hoặc sau các flex item khác. `order` là thuộc tính cho phép ta xác định thứ tự xuất hiện của các flex item. Theo mặc định thì các flex item sẽ xuất hiện theo thứ tự mà chúng xuất hiện trong code HTML. Còn về thuộc tính `order`, mặc định của nó là 0. Ta sẽ thử set `order: -1` cho div màu đỏ.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            order: -1;
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta có thể thấy rằng div màu đỏ xuất hiện trước các div khác, lý do là vì `order` của các div này là -1, trong khi các div khác vẫn là 0. Khi `order` bằng nhau, thứ tự xuất hiện sẽ theo thứ tự xuất hiện trong code HTML.

## flex-grow

`flex-grow` quy định tỉ lệ mà các flex item sẽ mở rộng khi có thêm không gian. Mặc định của nó là 0, nghĩa là các flex item sẽ không mở rộng. Ta sẽ thử set `flex-grow: 1` cho div màu đỏ.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            flex-grow: 1;
            height: 75px;
            background-color: red;
        }
        .green {
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy rằng div đỏ trên hàng thứ hai sẽ mở rộng ra nếu có thêm không gian. Hàng đầu tiên thì không còn không gian trống nào nữa. Nếu ta set `flex-grow: 1` cho div xanh lá thì sẽ thế nào?

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            flex-grow: 1;
            height: 75px;
            background-color: red;
        }
        .green {
            flex-grow: 1;
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy rằng cả div màu đỏ và xanh lá đều mở rộng ra khi có thêm không gian. Nếu cả 2 đều có `flex-grow: 1` thì chúng sẽ mở rộng ra cùng một tỉ lệ. Đây chính là lý do mà nó không có đơn vị, vì nó theo tỉ lệ. Nếu ta set `flex-grow: 2` cho div xanh lá, thì div xanh lá sẽ mở rộng ra gấp đôi so với div màu đỏ.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            flex-grow: 1;
            height: 75px;
            background-color: red;
        }
        .green {
            flex-grow: 2;
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## flex-shrink

`flex-shrink` quy định xem flex item có thể co lại hay không. Mặc định của nó là 1, nghĩa là các flex item có thể co lại. Ta sẽ thử set `width: 100vw` cho div xanh lá và `flex-shrink: 1` cho nó luôn.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            width: 100vw;
            flex-shrink: 1;
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Lý ra div xanh lá sẽ chiếm hết không gian trình duyệt nhưng nó lại chỉ chiếm không gian section thôi. Nếu ta set `flex-shrink: 0` cho div xanh lá thì nó sẽ không co lại nữa.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            width: 100vw;
            flex-shrink: 0;
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## flex-basis

`flex-basis` quy định kích thước của flex item theo trục chính, nghĩa là khi `flex-direction: row`, `flex-basis` sẽ giống như `width`, còn khi `flex-direction: column`, `flex-basis` sẽ giống như `height`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            flex-basis: 100vw;
            flex-shrink: 0;
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

## flex

Từ `flex-grow`, `flex-shrink` và `flex-basis`, ta có thể viết gọn bằng thuộc tính `flex`. `flex` nhận 3 giá trị, đầu tiên là `flex-grow`, thứ hai là `flex-shrink`, thứ ba là `flex-basis`. Ví dụ, ta có thể viết `flex: 1 0 200px` cho div xanh lá.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            display: flex;
            /* flex-direction: row;
            flex-wrap: wrap; */
            flex-flow: row wrap;
            justify-content: center;
            align-items: center;
            align-content: space-between;
            gap: 10px;
            height: 90vh;
            border: 2px dotted black;
        }
        .red {
            height: 75px;
            background-color: red;
        }
        .green {
            flex: 1 0 200px;
            height: 125px;
            background-color: green;
        }
        .blue {
            height: 100px;
            background-color: blue;
        }
        div {
            border: 5px solid black;
            padding: 0.5rem;
            color: white;
            font-size: 2rem;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <section>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
        <div class="red">Red</div>
        <div class="green">Green</div>
        <div class="blue">Blue</div>
    </section>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Điều này có nghĩa là, với `flex-grow: 1` thì div xanh lá sẽ mở rộng ra khi có thêm không gian, với `flex-shrink: 0` thì nó sẽ không nhỏ hơn kích thước quy định theo trục chính, nghĩa là chiều rộng của nó sẽ không nhỏ hơn 200px.