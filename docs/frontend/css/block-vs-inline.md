# Block vs. Inline

Hãy cùng so sánh Block và Inline thông qua ví dụ sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            border: 2px solid black;
        }
        em {
            border: 2px solid blue;
        }
    </style>
</head>
<body>
    <p>I'm a paragraph!</p>
    <p>I'm <em>another</em> paragraph!</p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta có 2 paragraph, một cái bình thường và một cái có thẻ `<em>` bên trong. Thẻ `<p>` sẽ được bọc trong khung đen, còn thẻ `<em>` sẽ được bọc trong khung xanh.

Ta có thể thấy rằng dù dài hay ngắn thì hai thẻ `<p>` này đều có chiều rộng bằng với chiều rộng của trang. Lý do là vì thẻ `<p>` là một block-level element. Một block-level element sẽ chiếm hết chiều rộng của trang và bắt đầu từ một dòng mới. Chính vì thế nên ngay cả khi ta đổi chiều rộng của `<p>`, cái thẻ `<p>` thứ hai vẫn sẽ bắt đầu từ một dòng mới.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            width: 30vw;
            border: 2px solid black;
        }
        em {
            border: 2px solid blue;
        }
    </style>
</head>
<body>
    <p>I'm a paragraph!</p>
    <p>I'm <em>another</em> paragraph!</p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ngược lại, thẻ `<em>` là một inline element. Inline element không bắt dầu từ dòng mới cũng như chiều rộng của nó sẽ bằng với nội dung bên trong. Ta có thể thử bằng cách cho chiều rộng của thẻ `<em>` thật to.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            width: 30vw;
            border: 2px solid black;
        }
        em {
            width: 100vw;
            border: 2px solid blue;
        }
    </style>
</head>
<body>
    <p>I'm a paragraph!</p>
    <p>I'm <em>another</em> paragraph!</p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Không có gì thay đổi cả. Inline element không quan tâm bạn set chiều rộng bằng bao nhiêu, nó chỉ quan tâm đến chiều rộng của nội dung bên trong thôi. Điều này cũng đúng khi bạn set chiều cao cho inline element, nó sẽ không thay đổi dù cho chiều cao có là `100vh`.

Nếu muốn thay đổi điều này, bạn có thể dùng thuộc tính `display`, thử với `<em>` xem, ta sẽ cho nó thành một block-level element.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            width: 30vw;
            border: 2px solid black;
        }
        em {
            display: block;
            width: 100vw;
            height: 100vh;
            border: 2px solid blue;
        }
    </style>
</head>
<body>
    <p>I'm a paragraph!</p>
    <p>I'm <em>another</em> paragraph!</p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Giờ thẻ `<em>` đã rất to rồi, vì nó đã trở thành một block-level element. Thử xóa chiều rộng và chiều cao xem nhé.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            width: 30vw;
            border: 2px solid black;
        }
        em {
            display: block;
            border: 2px solid blue;
        }
    </style>
</head>
<body>
    <p>I'm a paragraph!</p>
    <p>I'm <em>another</em> paragraph!</p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta thấy rằng thẻ `<em>` lúc này giống như paragraph, chỉ là nó không có các margin giống như paragraph thôi.

Tạm xóa `display: block` ra khỏi thẻ `<em>` nhé. Giờ ta muốn hai paragraph nằm trên cùng một dòng thì sao, ta có thể sử dụng `display: inline` cho thẻ `<p>`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            display: inline;
            width: 30vw;
            border: 2px solid black;
        }
        em {
            border: 2px solid blue;
        }
    </style>
</head>
<body>
    <p>I'm a paragraph!</p>
    <p>I'm <em>another</em> paragraph!</p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta thấy rằng hai paragraph đã nằm trên cùng hàng, nhưng với inline thì chiều rộng không có tác dụng nữa. Để set chiều rộng và chiều cao, ta có thể dùng `display: inline-block`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            display: inline-block;
            width: 30vw;
            border: 2px solid black;
        }
        em {
            border: 2px solid blue;
        }
    </style>
</head>
<body>
    <p>I'm a paragraph!</p>
    <p>I'm <em>another</em> paragraph!</p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Hai paragraph nằm trên cùng hàng (inline) và có chiều rộng được set (block).

Các element container thường sẽ là block-level element, trong khi các element nhỏ thường là inline element. Trường hợp có vẻ là duy nhất dùng inline-block là với hình ảnh, vì ta muốn thay đổi kích thước nhưng không muốn nó chiếm hết một dòng.