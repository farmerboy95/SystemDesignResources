# Stacking Context

Ta sẽ tìm hiểu về stacking context qua ví dụ sau đây:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        #red {
            z-index: 1;
            background-color: red;
        }
        #blue {
            z-index: 2;
            top: 100px;
            left: 150px;
            background-color: blue;
        }
        #green {
            z-index: 3;
            top: 200px;
            left: 40px;
            background-color: green;
        }
        div {
            position: fixed;
            width: 200px;
            height: 200px;
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
        <div id="red">Red</div>
        <div id="blue">Blue</div>
    </section>
    <div id="green">Green</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta có một thẻ `<section>` chứa hai thẻ `<div>` với màu đỏ và màu xanh. Bên ngoài `<section>` có một thẻ `<div>` với màu xanh lá cây. Các thẻ `<div>` có kích thước bằng nhau nhưng thẻ xanh lá lại nằm trên thẻ xanh da trời và thẻ đỏ. `position` của các thẻ `<div>` là `fixed` và thẻ xanh lá và xanh da trời ta có chỉ định `top` và `left` để nó nằm ở vị trí mong muốn từ góc trên bên trái của trình duyệt.

Ở đây ta thấy thuộc tính `z-index`, mỗi thẻ có một giá trị `z-index` khác nhau, thấp nhất là đỏ, cao nhất là xanh lá cây. Có vẻ hợp lý vì ta thấy thẻ có `z-index` cao nhất đang nằm trên còn thẻ có `z-index` thấp hơn thì nằm dưới. Ta có thể đổi `z-index` của thẻ đỏ thành 4 để xem nó thay đổi thế nào.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        #red {
            z-index: 4;
            background-color: red;
        }
        #blue {
            z-index: 2;
            top: 100px;
            left: 150px;
            background-color: blue;
        }
        #green {
            z-index: 3;
            top: 200px;
            left: 40px;
            background-color: green;
        }
        div {
            position: fixed;
            width: 200px;
            height: 200px;
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
        <div id="red">Red</div>
        <div id="blue">Blue</div>
    </section>
    <div id="green">Green</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy thẻ đỏ nằm trên cùng với `z-index` cao nhất trong cả ba. Thôi đổi về 1 lại nhé.

Mỗi khi dùng `position: fixed`, ta tạo ra cái gọi là **stacking context** và `position: fixed` không phải cách duy nhất tạo ra được stacking context, có nhiều cách nữa, như `position: sticky` chẳng hạn. Một stacking context là một tập hợp các element nằm trên chiều sâu của trang web (hay còn gọi là chiều Oz của trục tọa độ Oxyz). Ở đây, mỗi thẻ màu sẽ tạo ra stacking context riêng của nó vì mỗi thẻ đều có `position: fixed`.

`z-index` sẽ cho ta biết rằng nó nằm ở vị trí nào trên trục Oz. Khi mỗi element có stacking context riêng, `z-index` của các stacking context sẽ được so sánh với nhau để tạo ra thứ tự hiển thị. Nhưng chuyện gì sẽ xảy ra khi ta cho `<section>` cũng là một stacking context?

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            position: fixed;
            z-index: 4;
        }
        #red {
            z-index: 1;
            background-color: red;
        }
        #blue {
            z-index: 2;
            top: 100px;
            left: 150px;
            background-color: blue;
        }
        #green {
            z-index: 3;
            top: 200px;
            left: 40px;
            background-color: green;
        }
        div {
            position: fixed;
            width: 200px;
            height: 200px;
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
        <div id="red">Red</div>
        <div id="blue">Blue</div>
    </section>
    <div id="green">Green</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy hai thẻ đỏ và xanh da trời đang nằm trên thẻ xanh lá. Nhưng `z-index` của thẻ xanh lá là 3 mà, nó phải hơn `z-index` của thẻ đỏ và xanh da trời (lần lượt là 1 và 2) chứ? Đó là vì cả `<section>` đang là một stacking context (vì ta muốn thế mà), thẻ xanh lá cũng là một stacking context. Ta gọi chúng là các stacking context cùng bậc vì chúng đều nằm trên root level. `<section>` có `z-index` là 4, thẻ xanh lá có `z-index` là 3, nên thẻ xanh lá sẽ nằm dưới `<section>`, nghĩa là nó nằm dưới mọi thứ trong `<section>`.

Tiếp đến là thẻ đỏ và xanh da trời. Vì chúng nằm chung bậc với nhau nên sẽ được so sánh với nhau, nên thẻ đỏ sẽ nằm dưới. Ta có thể thử cho `z-index` của thẻ xanh da trời thành một số âm.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            position: fixed;
            z-index: 4;
        }
        #red {
            z-index: 1;
            background-color: red;
        }
        #blue {
            z-index: -1;
            top: 100px;
            left: 150px;
            background-color: blue;
        }
        #green {
            z-index: 3;
            top: 200px;
            left: 40px;
            background-color: green;
        }
        div {
            position: fixed;
            width: 200px;
            height: 200px;
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
        <div id="red">Red</div>
        <div id="blue">Blue</div>
    </section>
    <div id="green">Green</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta thấy thẻ xanh da trời đã nằm dưới thẻ đỏ, nhưng nó vẫn nằm trên thẻ xanh lá, đơn giản vì cha của nó là thẻ `<section>` cùng bậc với thẻ xanh lá và `z-index` của `<section>` lớn hơn thẻ xanh lá. Với lý do đó, khi đổi `z-index` của xanh lá thành 5, ta sẽ thấy nó nằm trên 2 thẻ còn lại.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            position: fixed;
            z-index: 4;
        }
        #red {
            z-index: 1;
            background-color: red;
        }
        #blue {
            z-index: -1;
            top: 100px;
            left: 150px;
            background-color: blue;
        }
        #green {
            z-index: 5;
            top: 200px;
            left: 40px;
            background-color: green;
        }
        div {
            position: fixed;
            width: 200px;
            height: 200px;
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
        <div id="red">Red</div>
        <div id="blue">Blue</div>
    </section>
    <div id="green">Green</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Giờ thẻ xanh lá nằm trên hai thẻ còn lại rồi, dù bạn làm gì với `z-index` của hai thẻ còn lại, nó cũng không thể nằm lên trên thẻ xanh lá được. Bạn chỉ có thể đổi `z-index` của `<section>` cho nó lớn hơn thẻ xanh lá mới được thôi. Bạn có thể dùng Try the Code ở dưới để thử nghiệm nhé.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        section {
            position: fixed;
            z-index: 6;
        }
        #red {
            z-index: 1;
            background-color: red;
        }
        #blue {
            z-index: -1;
            top: 100px;
            left: 150px;
            background-color: blue;
        }
        #green {
            z-index: 5;
            top: 200px;
            left: 40px;
            background-color: green;
        }
        div {
            position: fixed;
            width: 200px;
            height: 200px;
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
        <div id="red">Red</div>
        <div id="blue">Blue</div>
    </section>
    <div id="green">Green</div>
</body>
</html>
```
<apprun-play style="height:600px"></apprun-play>