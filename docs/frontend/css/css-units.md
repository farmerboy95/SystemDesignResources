# Các loại đơn vị trong CSS

## Đơn vị độ dài

Đơn vị độ dài thường được sử dụng để xác định chiều rộng, chiều cao, kích thước font, margin và nhiều thứ khác.

- `px`: Pixel, đơn vị độ dài cơ bản nhất, là loại độ dài tuyệt đối, nghĩa là nó không thay đổi khi các thiết lập môi trường thay đổi. Có nhiều loại đơn vị tuyệt đối khác như `in` (inch), `cm` (centimeter), `mm` (milimeter), `pt` (point), `pc` (pica), nhưng `px` là phổ biến nhất trong số đó. Tuy nhiên các đơn vị tuyệt đối không được sử dụng nhiều lắm. Người ta hay dùng các đơn vị tương đối hơn, nghĩa là các đơn vị có giá trị phụ thuộc vào thứ gì đó khác.
- `em`: Đơn vị tương đối, dựa vào kích thước font. Nếu dùng `em` để set kích thước font, nó sẽ dựa vào kích thước font của thẻ cha.
- `rem`: Tương tự như `em`, nhưng nó dựa vào kích thước font của thẻ root (`html`), mặc định là khoảng 16 pixel, nhưng ta có thể thay đổi được.
- `vw` và `vh`: Đơn vị tương đối, dựa vào chiều rộng và chiều cao của viewport. `1vw` tương đương với 1% chiều rộng của viewport, `1vh` tương đương với 1% chiều cao của viewport.
- `ch`: Đơn vị tương đối, dựa vào kích thước của ký tự `0` trong font hiện tại, giúp ta chọn chiều rộng dựa theo số ký tự trong đoạn văn.
- `%`: Đơn vị tương đối, dựa vào giá trị của thuộc tính cha. Ví dụ, nếu ta set `width: 50%` cho một thẻ con, nó sẽ chiếm 50% chiều rộng của thẻ cha. Tuy nhiên với một số thuộc tính, `%` sẽ hoạt động hơi khác một chút.

### Chọn đơn vị nào cho chuẩn

#### Chiều rộng và chiều cao

- `%`: Giúp giá trị tương đối với thẻ cha, ví dụ như muốn chiều rộng bằng nửa chiều rộng thẻ cha.
- `vw` và `vh`: Giúp giá trị tương đối với viewport, ví dụ như khi muốn tạo website chỉ có một trang mà không cần scroll.
- `ch`: Dùng cho độ dài paragraph, vì nó dựa vào kích thước của ký tự `0`. Ví dụ như trường hợp paragraph dài hơn 70 ký tự sẽ khá khó đọc, nên ta có thể set nó tầm khoảng 50 đến 70 để dễ đọc hơn.
- `rem`: Nếu cần giá trị gần với giá trị tuyệt đối, hãy dùng `rem`.
- `px`: Dùng khi cần giá trị cố định, không thay đổi, nhưng ảnh hưởng tới accessibility.

#### Margin và padding

- `rem`: Gần với giá trị tuyệt đối.
- `em`: Dựa vào kích thước font của thẻ cha.
- `px`: Cẩn thận khi dùng, giá trị cố định, không thay đổi.

#### Viền và bóng

- `px`: Thích hợp vì ta thường không muốn chúng thay đổi khi môi trường thay đổi.
- `em` và `rem`: Cũng được nhưng đôi khi làm viền và bóng quá to.

#### Kích thước font

- `rem`: Dễ dàng thay đổi kích thước font toàn bộ trang.
- `em`: Cũng được nhưng đôi khi khó theo dõi kích thước vì nó phụ thuộc vào thẻ cha.
- `px`: Lựa chọn cuối cùng, không nên dùng nếu không cần thiết.

## Đơn vị màu sắc

- Tên màu: Ví dụ như `red`, `blue`, ... Dễ đọc nhưng màu có thể khá chói.
- Mã hex: Ví dụ như `#ff0000`, `#0000ff`, ... Hai ký tự đầu là lượng màu đỏ, hai ký tự tiếp theo là lượng màu xanh, hai ký tự cuối là lượng màu xanh lá cây.
- Mã RGB: Ví dụ như `rgb(255, 0, 0)`, `rgb(0, 0, 255)`, ... Cũng giống như mã hex nhưng ở hệ thập phân. Ngoài ra còn có RGBA, giống như RGB nhưng có thêm một giá trị alpha để chỉ độ trong suốt. Ví dụ `rgba(255, 0, 0, 0.5)` sẽ là màu đỏ nhưng chỉ độ trong suốt 50%.
- Mã HSL: Ví dụ như `hsl(0, 100%, 50%)`, `hsl(240, 100%, 50%)`, ... HSL là một hệ màu khác, dễ hiểu hơn RGB, với `h` là hue (màu), `s` là saturation (độ bão hòa), `l` là lightness (độ sáng). Cũng có HSLA, giống như HSL nhưng có thêm một giá trị alpha.

## Ví dụ

Ta có code HTML sau:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        #parent {
            width: 75%;
            border: 2px solid black;
        }
        #red {
            width: 50%;
            background-color: red;
        }
        #blue {
            width: 50vw;
            background-color: blue;
        }
        #green {
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ở đây ta có một thẻ `<div>` có ID là `parent`, chứa các dải màu đỏ, xanh da trời và xanh lá có độ dài khác nhau, xuống dưới ta có một thẻ `<p>` dài. Ta cùng xem các đơn vị được sử dụng trong CSS nhé.

Đầu tiên là ID selector `#parent`, có chiều rộng là 75%, nên nó sẽ rộng 75% chiều rộng của thẻ cha, tức là thẻ `<body>`, là rộng gần cả trang. ID selector `#red` có chiều rộng là 50%, nên nó sẽ rộng 50% chiều rộng của thẻ cha, tức là 50% chiều rộng của thẻ `#parent`. Ta sẽ thấy phần màu đỏ rộng bằng đúng một nửa chiều rộng của `#parent`.

ID selector `#blue` có chiều rộng là 50vw, nghĩa là 50% chiều rộng của viewport, tức là 50% chiều rộng của cửa sổ trình duyệt. Khi ta thay đổi kích thước cửa sổ, phần màu xanh sẽ thay đổi theo. ID selector `#green` có chiều rộng là 10rem, nghĩa là 10 lần kích thước font của thẻ html, tức là khoảng 160px.

Giờ ta thử đổi kích thước `#parent` thành 50% xem sao.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        #parent {
            width: 50%;
            border: 2px solid black;
        }
        #red {
            width: 50%;
            background-color: red;
        }
        #blue {
            width: 50vw;
            background-color: blue;
        }
        #green {
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta thấy phần xanh da trời bị tràn ra ngoài hình chữ nhật chứa ba màu, vì 50vw to hơn chiều rộng của thẻ cha. Màu đỏ thì ngược lại, vẫn giữ nguyên 50% chiều rộng của thẻ cha. Màu xanh lá thì vẫn y nguyên.

Ví dụ ta đổi `#parent` thành 15% thì sao.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        #parent {
            width: 15%;
            border: 2px solid black;
        }
        #red {
            width: 50%;
            background-color: red;
        }
        #blue {
            width: 50vw;
            background-color: blue;
        }
        #green {
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ở đây ta vẫn thấy hai màu xanh da trời và xanh lá giữ nguyên chiều rộng, còn màu đỏ thì bị teo thành một chút, nhưng vẫn giữ 50% chiều rộng của thẻ cha. Màu xanh da trời và xanh lá giờ đã tràn ra ngoài, đó là lý do vì sao với chiều rộng / chiều cao, ta nên dùng `%`.

Giờ xét paragraph nằm dưới, nó có vẻ khá khó đọc. Và nếu chiều rộng màn hình to ra, nó sẽ còn khó đọc hơn, đây không phải trải nghiệm người dùng tốt. Ta cần chỉnh lại kích thước của nó.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            width: 60ch;
        }
        #parent {
            width: 15%;
            border: 2px solid black;
        }
        #red {
            width: 50%;
            background-color: red;
        }
        #blue {
            width: 50vw;
            background-color: blue;
        }
        #green {
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta cho nó độ rộng `60ch`, nghĩa là nó hoàn toàn dựa vào kích thước font và font hiện tại. Nó sẽ làm độ dài đoạn văn không đổi khi ta thay đổi kích thước màn hình, giúp người ta dễ đọc hơn. Tuy nhiên, nếu sử dụng đơn vị khác, ví dụ `50%`, nó sẽ phụ thuộc vào kích thước màn hình. Nếu màn hình to quá thì đoạn văn vẫn sẽ nằm trên một dòng, còn nhỏ quá thì cũng sẽ khó đọc vì có quá nhiều dòng.

Giờ ta nghịch một tí với kích thước font nhé, ta thử set `1em` cho `#red`, `1rem` cho `#blue` và `16px` cho `#green`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            width: 60ch;
        }
        #parent {
            width: 15%;
            border: 2px solid black;
        }
        #red {
            font-size: 1em;
            width: 50%;
            background-color: red;
        }
        #blue {
            font-size: 1rem;
            width: 50vw;
            background-color: blue;
        }
        #green {
            font-size: 16px;
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Không có gì thay đổi cả. Vì mặc định kích thước là 16px rồi, nên `1rem` và `16px` giống nhau, `1em` cũng giống như `16px` vì kích thước font của thẻ cha `#parent` cũng là mặc định khi ta không set.

Ta thử đổi kích thước font của `#parent` xem sao.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        p {
            width: 60ch;
        }
        #parent {
            font-size: 1.5em;
            width: 15%;
            border: 2px solid black;
        }
        #red {
            font-size: 1em;
            width: 50%;
            background-color: red;
        }
        #blue {
            font-size: 1rem;
            width: 50vw;
            background-color: blue;
        }
        #green {
            font-size: 16px;
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta thấy màu đỏ sẽ to ra vì nó dựa vào kích thước font của `#parent`, nay đã to lên, tầm 24px. Thử cho kích thước font của thẻ `<html>` thành 24px coi có khác biệt gì không. Cho chiều rộng của `#parent` về 50% cho dễ nhìn.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        html {
            font-size: 24px;
        }
        p {
            width: 60ch;
        }
        #parent {
            font-size: 1.5em;
            width: 50%;
            border: 2px solid black;
        }
        #red {
            font-size: 1em;
            width: 50%;
            background-color: red;
        }
        #blue {
            font-size: 1rem;
            width: 50vw;
            background-color: blue;
        }
        #green {
            font-size: 16px;
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ở đây màu đỏ sẽ là `1em` là bằng kích thước font thẻ cha, mà thẻ cha là `1.5em`, nghĩa là 1.5 lần thẻ cha (thẻ `<html>`) là 1.5 x 24 = 36 pixel. Màu xanh da trời sẽ là `1rem`, nghĩa là bằng kích thước font của thẻ `<html>`, nên nó sẽ là 24 pixel. Màu xanh lá sẽ là 16 pixel, vì nó là `16px`. Đây là lý do vì sao không nên dùng `px` cho kích thước font.

Cuối cùng là về màu, ta sẽ cho màu xanh da trời một màu khác, ví dụ `#4B7DAF`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        html {
            font-size: 24px;
        }
        p {
            width: 60ch;
        }
        #parent {
            font-size: 1.5em;
            width: 50%;
            border: 2px solid black;
        }
        #red {
            font-size: 1em;
            width: 50%;
            background-color: red;
        }
        #blue {
            font-size: 1rem;
            width: 50vw;
            background-color: #4B7DAF;
        }
        #green {
            font-size: 16px;
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Ta sẽ thấy màu xanh da trời đổi thành một màu xanh nhạt hơn. Nhưng cách này có vẻ khá tối nghĩa, người ta hay dùng RGB hơn, cụ thể là `rgb(75, 125, 175)`. Mỗi tham số có giá trị từ 0 đến 255.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        html {
            font-size: 24px;
        }
        p {
            width: 60ch;
        }
        #parent {
            font-size: 1.5em;
            width: 50%;
            border: 2px solid black;
        }
        #red {
            font-size: 1em;
            width: 50%;
            background-color: red;
        }
        #blue {
            font-size: 1rem;
            width: 50vw;
            background-color: rgb(75, 125, 175);
        }
        #green {
            font-size: 16px;
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Không có gì thay đổi cả, vẫn là màu đó nhưng nhìn dễ hiểu hơn thôi. Ta cũng có thể cho thêm giá trị alpha vào, ví dụ như 0.5 sẽ làm nó trong suốt 50%, hay 0 sẽ làm nó hoàn toàn trong suốt.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        html {
            font-size: 24px;
        }
        p {
            width: 60ch;
        }
        #parent {
            font-size: 1.5em;
            width: 50%;
            border: 2px solid black;
        }
        #red {
            font-size: 1em;
            width: 50%;
            background-color: red;
        }
        #blue {
            font-size: 1rem;
            width: 50vw;
            background-color: rgba(75, 125, 175, 0.5);
        }
        #green {
            font-size: 16px;
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Với màu HSL, ta có thể dùng `hsl(210, 40%, 49%, 0.5)` để ra màu y như vậy. Mình sẽ để Try the Code cho bạn thử nghiệm.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <style>
        html {
            font-size: 24px;
        }
        p {
            width: 60ch;
        }
        #parent {
            font-size: 1.5em;
            width: 50%;
            border: 2px solid black;
        }
        #red {
            font-size: 1em;
            width: 50%;
            background-color: red;
        }
        #blue {
            font-size: 1rem;
            width: 50vw;
            background-color: hsl(210, 40%, 49%, 0.5);
        }
        #green {
            font-size: 16px;
            width: 10rem;
            background-color: green;
        }
        div {
            margin: 10px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="parent">
        <div id="red">Red</div>
        <div id="blue">Blue</div>
        <div id="green">Green</div>
    </div>
    <p>
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
        This is a very long paragraph.
    </p>
</body>
</html>
```
<apprun-play style="height:300px"></apprun-play>