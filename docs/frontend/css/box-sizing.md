# Box Sizing

Ta sẽ tìm hiểu về `box-sizing` qua ví dụ sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        #container {
            width: 300px;
            background-color: lightblue;
        }
        #child {
            width: 100%;
            border: 10px solid black;
            padding: 12px;
            background-color: orange;
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Container</h1>
        <div id="child">
            <h2>Child</h2>
        </div>
    </div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta có một container với chiều rộng là 300px, màu nền là xanh. Bên trong container có một thẻ `<h1>` và một thẻ `<div>` với chiều rộng là 100%, border 10px đen, padding 12px, màu nền là cam. Ta có thể thấy `<div>` con nằm tràn ra ngoài container. Lý ra nó phải nằm khít trong container chứ nhỉ? Chiều rộng của nó là 100% mà.

Thực ra cái 100% đó là 100% của nội dung thẻ cha. Nhưng `#child` có viền dày 10px và thêm 12px padding nữa. Mà viền và padding có cả bên trái và phải, nên ta sẽ phải nhân đôi 10px và 12px lên (viền 10px bên trái và 10px bên phải, padding 12px bên trái và 12px bên phải). Điều này khiến cho chiều rộng thực sự của `#child` là 100% + 20px + 24px = 300px + 44px = 344px, to hơn chiều rộng của container là 300px.

Có một số cách để sửa, một trong số đó là dùng hàm `calc()` để tính toán chiều rộng nên có của `#child`, tức là 100% - 20px - 24px.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        #container {
            width: 300px;
            background-color: lightblue;
        }
        #child {
            width: calc(100% - 20px - 24px);
            border: 10px solid black;
            padding: 12px;
            background-color: orange;
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Container</h1>
        <div id="child">
            <h2>Child</h2>
        </div>
    </div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Tuy nhiên cách này khá kỳ cục, vì giá trị tính lại phụ thuộc vào các giá trị khác, nếu bạn đổi padding thì phải đổi lại công thức tính một chút, rất phiền hà. Ta sẽ dùng thuộc tính `box-sizing` để giải quyết vấn đề này. Ta có thể chọn giữa `content-box` (chiều rộng này chỉ bao gồm nội dung, không bao gồm padding và border) và `border-box` (chiều rộng này bao gồm cả padding và border). Mặc định của `box-sizing` là `content-box`. Ta thử với `border-box` xem sao.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        #container {
            width: 300px;
            background-color: lightblue;
        }
        #child {
            box-sizing: border-box;
            width: 100%;
            border: 10px solid black;
            padding: 12px;
            background-color: orange;
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Container</h1>
        <div id="child">
            <h2>Child</h2>
        </div>
    </div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Bây giờ chiều rộng sẽ tương ứng với công thức mà ta dùng khi dùng hàm `calc()`.

Có người thích `content-box`, nhưng cũng có người thích `border-box`. Nhưng bạn sẽ hay thấy rằng ban đầu người ta set `box-sizing` cho cả trang là `border-box` để tránh những vấn đề như trên.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        * {
            box-sizing: border-box;
        }
        #container {
            width: 300px;
            background-color: lightblue;
        }
        #child {
            width: 100%;
            border: 10px solid black;
            padding: 12px;
            background-color: orange;
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Container</h1>
        <div id="child">
            <h2>Child</h2>
        </div>
    </div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>