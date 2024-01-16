# Cơ bản về CSS

**Lưu ý: Trong bài viết có các đoạn code HTML, bạn có thể bấm Try the Code để thay đổi code và xem kết quả.**

## CSS là gì?

CSS là viết tắt của **C**ascading **S**tyle **S**heets. CSS là một ngôn ngữ định dạng được sử dụng để định dạng các tài liệu HTML, XML (hoặc các ngôn ngữ dạng XML như SVG, MathML hay XHTML). CSS được sử dụng để mô tả cách các phần tử được hiển thị trên màn hình, trên giấy, hoặc trên các phương tiện khác.

## Các loại CSS

Có 3 loại CSS chính:

1. **User Agent Stylesheets**: Đây là các stylesheet được cài đặt sẵn trong trình duyệt, ví dụ như các kích thước font mặc định cho các thẻ heading.
2. **User Stylesheets**: Đây là các stylesheet mà người dùng mong muốn sử dụng được lưu trong trình duyệt. Các stylesheet này có độ ưu tiên cao hơn các User Agent Stylesheets.
3. **Author Stylesheets**: Đây là các stylesheet được tác giả của website định nghĩa (là code CSS mà ta sẽ viết ấy). Các stylesheet này có độ ưu tiên cao nhất (trong trường hợp `!important` không được sử dụng).

Như vậy ta thấy trong hầu hết các trường hợp, code CSS mà ta viết sẽ có độ ưu tiên cao nhất.

## Khai báo CSS

CSS đơn giản là tập hợp rất nhiều các khai báo, trong đó mỗi khai báo sẽ là một cặp property-value theo cú pháp `property: value;` (nghĩa là tên đặc tính, đến một dấu hai chấm, rồi đến giá trị, cuối cùng là dấu chấm phẩy) diễn tả cách mà một phần tử HTML sẽ được định dạng. Ví dụ, để đổi màu chữ của một phần tử HTML thành màu đỏ, ta sẽ viết như sau:

```css
color: red;
```

Tiếp theo, ta cần cho CSS biết phần tử nào ta sẽ muốn định dạng. Ta thực hiện bằng cách cho khai báo vào thứ mà ta gọi là một **ruleset**. Ruleset gồm hai thành phần chính, đó là **selector** và **declaration block (khối khai báo)**. Selector sẽ quyết định các phần tử nào sẽ bị ảnh hưởng bởi ruleset. Ví dụ ta có ruleset như sau:
    
```css
h1, p {
  color: red;
  margin: 10px;
}
```

Ở đây, ta đang chọn (select) tất cả các thẻ `h1` và `p` để định dạng. Các tuỳ chọn trong selector sẽ được nói đến trong một bài viết riêng. Trong cặp ngoặc nhọn thì ta có khối khai báo. Đây chỉ bao gồm một tập các khai báo thôi. Lưu ý rằng dòng cuối không cần có dấu chấm phảy, tuy nhiên vẫn nên dùng nhé.

## Comment trong CSS

Comment trong CSS được bắt đầu bằng `/*` và kết thúc bằng `*/`. Comment có thể nằm ở bất kì đâu trong CSS, và nó sẽ không ảnh hưởng gì tới CSS.

```css
/* Đây là một comment */
h1 {
  color: red; /* Đây là một comment khác */
}
```

## Thẻ link

Cuối cùng, ta cần cái gì đó để file HTML của ta nhận ra được file CSS ta viết. Ở đây ta sử dụng thẻ `link` như sau:

```html
<link rel="stylesheet" href="style.css">
```

Nó bao gồm hai thuộc tính chính. Đầu tiên là thuộc tính `rel` (viết tắt của relationship) để nói với trình duyệt rằng file CSS này có quan hệ gì với file HTML. Ở đây ta sử dụng giá trị `stylesheet` để nói rằng file CSS này là một stylesheet. Thuộc tính thứ hai là `href` (viết tắt của hyperlink reference) để nói với trình duyệt rằng file CSS này nằm ở đâu. Ở đây ta sử dụng giá trị `style.css` để nói rằng file CSS này nằm ở cùng thư mục với file HTML. Nếu file CSS nằm ở một thư mục khác, ta cần phải chỉ định đường dẫn tới file CSS đó. Đường dẫn có thể là tuyệt đối hoặc tương đối.

## Ví dụ

Ta có một file HTML như sau (lưu ý là mình phải comment thẻ `link` vì nếu không thì trình duyệt sẽ tìm file CSS theo đường dẫn trong thẻ, các bạn thử code trong máy cá nhân thì bỏ comment đi nhé):

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Hello world!</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```
<apprun-play></apprun-play>

Ta có thể thấy là file HTML này có một `link` với `style.css`. Do code trên trang này không có nhiều file nên ta sẽ giả sử là hai file này riêng biệt nhé. Giờ ta thấy dòng nào cũng canh lề trái cả, ta muốn thẻ `h1` nằm giữa đi, thì code sẽ như sau:

```html
<style type="text/css">
  /* style.css */
  h1 {
    text-align: center;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Hello world!</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```
<apprun-play></apprun-play>

Ta có thể muốn thêm vài tính chất nữa cho `h1`. Giờ ta muốn màu của nó đỏ đi, ta sẽ dùng `color: red;`. Chữ đậm quá hả, thay thành chữ bình thường nhé, dùng `font-weight: normal`. Muốn nó có gạch dưới à, dùng `text-decoration: underline;`. Cứ như thế, ta có thể thêm tính chất cho thẻ thông qua CSS.

```html
<style type="text/css">
  /* style.css */
  h1 {
    text-align: center;
    color: red;
    font-weight: normal;
    text-decoration: underline;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Hello world!</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```
<apprun-play></apprun-play>

Với thẻ `p` thì sao nhỉ? Ta có thể đổi màu nền nó thành đỏ xem sao, dùng `background-color: red;`. Cơ mà lúc này chữ đen trên nền đỏ thì không đọc được, ta sẽ đổi màu chữ thành trắng đi, dùng `color: white;`.

```html
<style type="text/css">
  /* style.css */
  h1 {
    text-align: center;
    color: red;
    font-weight: normal;
    text-decoration: underline;
  }
  p {
    background-color: red;
    color: white;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Hello world!</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```
<apprun-play></apprun-play>

Thử set chiều cao cho `p` xem, ví dụ như `height: 100px` chẳng hạn. Nhưng mà chiều cao cố định thì có vẻ hơi dở, dùng `min-height` xem nào. Nó sẽ giúp ta set chiều cao tối thiểu cho `p`, nếu nội dung của `p` dài hơn thì nó sẽ tự tăng chiều cao lên, ví dụ như khi ta tăng kích thước font lên to quá. Mà nói đến kích thước font, ta có thể set kích thước font cho `p` bằng `font-size: 1.25rem;`. Ủa mà `rem` là cái gì? Nó là đơn vị `root em`, ý nghĩa là 1.25 lần kích thước font của thẻ `html` (thẻ gốc của trang web, thường mặc định là 16 pixels). Dùng `rem` sẽ tốt hơn dùng `px` vì nó sẽ tự động thay đổi theo kích thước font của trang web, nên nếu ta thay đổi kích thước font của trang web thì kích thước font của `p` cũng sẽ tự động thay đổi theo. Chiều rộng cũng thay đổi tí đi, thành `width: 30%;` đi. Đây là đơn vị phần trăm, nghĩa là chiều rộng của `p` sẽ bằng 30% chiều rộng của thẻ cha của nó (ở đây là thẻ `body`). Vậy là ta đã có một trang web như sau:

```html
<style type="text/css">
  /* style.css */
  h1 {
    text-align: center;
    color: red;
    font-weight: normal;
    text-decoration: underline;
  }
  p {
    background-color: red;
    color: white;
    min-height: 100px;
    font-size: 1.25rem;
    width: 30%;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Hello world!</h1>
    <p>This is a paragraph.</p>
  </body>
</html>
```
<apprun-play></apprun-play>
