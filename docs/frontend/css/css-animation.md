# Hoạt ảnh trong CSS

Trong bài này ta sẽ đi vào cách tạo hoạt ảnh với CSS thuần túy. Ta có ví dụ sau, với một div đơn giản hình chữ nhật xanh nhạt.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

## transition

`transition` sẽ cho phép chuyển đổi từ giá trị thuộc tính này sang giá trị thuộc tính khác. Ví dụ, ta có thể chuyển đổi chiều rộng thành một chiều rộng lớn hơn và sẽ có một hiệu ứng mượt mà giữa các giá trị này. Ta sẽ thử chuyển đổi khi div được hover nhé.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
        }
        div:hover {
            width: 50%;
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Khi di chuột vào div, ta thấy chiều rộng tăng lên, khi bỏ chuột ra thì chiều rộng về như cũ, nhưng không có hiệu ứng nào. Ta sẽ thêm vào div các thuộc tính `transition-property` và `transition-duration` để tạo hiệu ứng.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            transition-property: width;
            transition-duration: 1s;
        }
        div:hover {
            width: 50%;
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Giờ khi di chuột vào nó đã có hiệu ứng chuyển đổi mượt mà trong 1 giây. Ta cũng có thể thay đổi tốc độ chuyển đổi, nghĩa là đều đều hay lúc nhanh lúc chậm, bằng cách thêm vào `transition-timing-function`, mặc định của nó là `ease`, ta có thể thử với `linear`, ta sẽ thấy tốc độ chuyển đổi đều hơn.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            transition-property: width;
            transition-duration: 1s;
            transition-timing-function: linear;
        }
        div:hover {
            width: 50%;
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta sẽ thấy tốc độ chuyển đổi đều hơn. Ta có thêm một thuộc tính nữa là `transition-delay`, nó sẽ cho phép ta đặt thời gian trễ trước khi chuyển đổi bắt đầu, mặc định là 0s, ta sẽ thử với 1s.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            transition-property: width;
            transition-duration: 1s;
            transition-timing-function: linear;
            transition-delay: 1s;
        }
        div:hover {
            width: 50%;
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Cuối cùng, ta có thể dùng cả 4 thuộc tính trong một dòng với `transition`. Ví dụ, `transition: width 1s linear 1s;` sẽ tạo hiệu ứng chuyển đổi chiều rộng trong 1 giây, đều đều, và có 1 giây trễ trước khi bắt đầu.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            /* transition-property: width;
            transition-duration: 1s;
            transition-timing-function: linear;
            transition-delay: 1s; */
            transition: width 1s linear 1s;
        }
        div:hover {
            width: 50%;
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta mới chỉ đổi chiều rộng mà thôi, nếu ta muốn đổi thuộc tính khác thì sao, ví dụ thêm `transform: rotate(180deg);` vào div khi hover thì sao? Có 2 cách đổi cho nhiều thuộc tính, cách 1 là dùng `all` trong `transition`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            /* transition-property: width;
            transition-duration: 1s;
            transition-timing-function: linear;
            transition-delay: 1s; */
            transition: all 1s linear 1s;
        }
        div:hover {
            width: 50%;
            transform: rotate(180deg);
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Cách 2 là ghi thêm một chuyển đổi nữa vào `transition` cách nhau bởi dấu phẩy, ví dụ `transition: width 1s linear 1s, transform 1s ease 0s;`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            /* transition-property: width;
            transition-duration: 1s;
            transition-timing-function: linear;
            transition-delay: 1s; */
            transition: width 1s linear 1s, transform 1s ease 0s;
        }
        div:hover {
            width: 50%;
            transform: rotate(180deg);
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

## animation

Đôi khi ta cần kiểm soát chuyển đổi nhiều hơn và thêm một số tính năng, ta cần dùng `animation`. Đầu tiên ta cần tạo một keyframe, nó sẽ chứa các trạng thái của chuyển đổi, ta sẽ tạo một keyframe đơn giản, ta sẽ muốn di chuyển nó sang phải 200px, rồi xuống dưới 300px, ta đạt được điều này với hàm `translate` của `transform`.

```css
@keyframes move {
    50% {
        transform: translate(200px, 0);
    }
    100% {
        transform: translate(200px, 300px);
    }
}
```

Số phần trăm nghĩa là ở X% thời gian chuyển đổi, ta sẽ có trạng thái như thế nào. Sau đó trong div, ta định nghĩa `animation-name: move`, `animation-duration: 3s`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            animation-name: move;
            animation-duration: 3s;
        }
        @keyframes move {
            50% {
                transform: translate(200px, 0);
            }
            100% {
                transform: translate(200px, 300px);
            }
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Khi reload thì ta thấy nó sẽ di chuyển sang phải 200px, rồi xuống dưới 300px, rồi quay về vị trí cũ. Ta muốn nó ở nguyên đó, không quay lại, ta sẽ thêm `animation-fill-mode: forwards`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            animation-name: move;
            animation-duration: 3s;
            animation-fill-mode: forwards;
        }
        @keyframes move {
            50% {
                transform: translate(200px, 0);
            }
            100% {
                transform: translate(200px, 300px);
            }
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ngoài ra ta có thể dùng các thuộc tính khác, ví dụ `animation-direction: reverse` để chuyển đổi ngược lại, `animation-iteration-count: 3` để lặp 3 lần, hoặc `infinite` để lặp vô hạn. Ta sẽ thêm luôn thuộc tính `animation-play-state: running` để biểu thị rằng nó đang chạy, ta có thể dùng `paused` để tạm dừng.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            animation-name: move;
            animation-duration: 3s;
            animation-fill-mode: forwards;
            animation-direction: reverse;
            animation-iteration-count: 3;
            animation-play-state: running;
        }
        @keyframes move {
            50% {
                transform: translate(200px, 0);
            }
            100% {
                transform: translate(200px, 300px);
            }
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta cũng có thể dùng `animation-timing-function` để thay đổi tốc độ chuyển đổi, ví dụ `linear`, `ease`, `ease-in`, `ease-out`, `ease-in-out`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            animation-name: move;
            animation-duration: 3s;
            animation-fill-mode: forwards;
            animation-direction: reverse;
            animation-iteration-count: 3;
            animation-play-state: running;
            animation-timing-function: linear;
        }
        @keyframes move {
            50% {
                transform: translate(200px, 0);
            }
            100% {
                transform: translate(200px, 300px);
            }
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ta cũng có thể dùng `cubic-bezier` để tạo tốc độ chuyển đổi theo ý muốn. Về mặt toán học, `cubic-bezier` sẽ tạo ra một đường cong từ điểm (0, 0) đến điểm (1, 1) với 2 điểm điều chỉnh, điểm đầu tiên là điểm điều chỉnh tốc độ chuyển đổi ở đầu, điểm thứ hai là điểm điều chỉnh tốc độ chuyển đổi ở cuối, hai điểm này là các tham số trong hàm. Đường càng dốc thì tốc độ càng nhanh, đường càng ngang thì tốc độ càng chậm. Bạn có thể dùng dev tool hay các tool trên mạng để tạo `cubic-bezier` cho mình.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            animation-name: move;
            animation-duration: 3s;
            animation-fill-mode: forwards;
            animation-direction: reverse;
            animation-iteration-count: 3;
            animation-play-state: running;
            animation-timing-function: cubic-bezier(0.42, 0, 0.58, 1);
        }
        @keyframes move {
            50% {
                transform: translate(200px, 0);
            }
            100% {
                transform: translate(200px, 300px);
            }
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Ngoài ra ta còn có hàm `steps`, nghĩa là có bao nhiêu bước từ điểm này đến điểm kia, ví dụ `steps(3)` sẽ chuyển đổi trong 3 bước.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            animation-name: move;
            animation-duration: 3s;
            animation-fill-mode: forwards;
            animation-direction: reverse;
            animation-iteration-count: 3;
            animation-play-state: running;
            animation-timing-function: steps(3);
        }
        @keyframes move {
            50% {
                transform: translate(200px, 0);
            }
            100% {
                transform: translate(200px, 300px);
            }
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Cuối cùng, ta có một thuộc tính tổng hợp `animation` để ghi cả 7 thuộc tính trong một dòng, ví dụ `animation: move 3s forwards reverse 3 linear steps(3);`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            /* animation-name: move;
            animation-duration: 3s;
            animation-fill-mode: forwards;
            animation-direction: reverse;
            animation-iteration-count: 3;
            animation-play-state: running;
            animation-timing-function: steps(3); */
            animation: move 3s forwards reverse 3 running steps(3);
        }
        @keyframes move {
            50% {
                transform: translate(200px, 0);
            }
            100% {
                transform: translate(200px, 300px);
            }
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>

Với keyframe ta có `from` và `to` để thay thế cho 0% và 100%, ví dụ

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        div {
            background-color: lightblue;
            width: 30%;
            height: 6em;
            border-radius: 12px;
            padding: 12px;
            margin: 4em;
            /* animation-name: move;
            animation-duration: 3s;
            animation-fill-mode: forwards;
            animation-direction: reverse;
            animation-iteration-count: 3;
            animation-play-state: running;
            animation-timing-function: steps(3); */
            animation: move 3s forwards reverse 3 running steps(3);
        }
        @keyframes move {
            from {
                transform: translate(0, 0);
            }
            50% {
                transform: translate(200px, 0);
            }
            to {
                transform: translate(200px, 300px);
            }
        }
    </style>
</head>
<body>
    <div>Animation</div>
</body>
</html>
```
<apprun-play style="height:600px" hide_button="true"></apprun-play>