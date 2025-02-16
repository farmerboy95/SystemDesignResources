# CSS Selector

Chúng ta sẽ sử dụng trang HTML sau cho các ví dụ trong bài viết này:

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p>Section paragraph</p>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

Ở đây ta sẽ có 3 phần:

1. Phần header lớn đầu tiên, có thẻ `h1` là `Top Level Heading`.
2. Phần section, có thẻ `h2` là `Section Heading` và thẻ `p` là `Section paragraph`.
3. Phần footer, có thẻ `h2` là `Footer Heading` và thẻ `p` là `Footer paragraph`.

Ta cũng link file html này với một file `style.css`, nhưng như bài trước, do một số giới hạn của nền tảng nên mình sẽ viết chung trong một file và phân biệt bằng comment.

Ta cùng đi vào từng loại selector nhé.

## Selector là gì?

Selector là thứ ta dùng ở đầu ruleset để chọn các phần tử bị ảnh hưởng bởi các khai báo trong ruleset. Ta có các loại selector sau:

### Type selector

Chọn (select) tất cả các phần tử có tên thẻ như đã chỉ định. Ví dụ:

```css
h1 {
  color: red;
}
```

Nó sẽ chọn tất cả các thẻ `h1` và thay đổi màu chữ thành màu đỏ. Thử trên trang HTML nhé.

```html
<style type="text/css">
  /* style.css */
  h1 {
    color: red;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p>Section paragraph</p>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

Ngoài ra ta có thể chọn nhiều thẻ cùng lúc bằng cách viết nhiều type selector cách nhau bởi dấu phẩy `,`. Ví dụ như sau sẽ làm cho tất cả các thẻ `h1` và `h2` có màu chữ đỏ.

```css
h1, h2 {
  color: red;
}
```

Không nhất thiết ta phải dùng chung kiểu thẻ `h` ở đây, ta cũng có thể làm như thế này. Nó sẽ chọn tất cả các thẻ `h1` và `p` và thay đổi màu chữ thành màu đỏ.

```css
h1, p {
  color: red;
}
```

Nếu bạn muốn chọn tất cả mọi thứ, có một selector đặc biệt là `*` để làm điều đó, nó còn được gọi là **Universal Selector**. Ví dụ như sau sẽ làm cho tất cả các phần tử trong trang có màu chữ đỏ.

```css
* {
  color: red;
}
```

Tuy nhiên, selector này không được khuyên dùng vì nó thường chọn nhiều hơn những gì ta muốn. Tốt nhất là nên chọn những gì ta muốn thay đổi một cách cụ thể.

Bạn có thể tự thử các ví dụ này trong trang HTML ở trên sau khi bấm Try the Code nhé.

### Class selector

Chọn tất cả các phần tử có tên class như đã chỉ định. Ví dụ:

```css
.red {
  color: red;
}
```

Ta dùng dấu chấm `.` để chỉ ra rằng đây là class selector, sau đó là tên class. Với trang HTML thì ta thêm class này vào `h1` và `p` trong `section` nhé. Lưu ý là `p` trong `footer` không bị ảnh hưởng bởi nó vì nó không có class `red`.

```html
<style type="text/css">
  /* style.css */
  .red {
    color: red;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1 class="red">Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p class="red">Section paragraph</p>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

Ta cũng có thể kết hợp nhiều loại selector với nhau, cách nhau bởi dấu phẩy. Ví dụ như sau sẽ làm cho các phần tử có class là `red` và thẻ `h2` có màu chữ đỏ.

```css
.red, h2 {
  color: red;
}
```

### ID selector

Chọn phần tử có ID như đã chỉ định. Ví dụ:

```css
#red {
  color: red;
}
```

Ta dùng dấu `#` để chỉ ra rằng đây là ID selector, sau đó là tên ID. Trong trang HTML, ta sẽ chỉ định ID cho `p` trong `section` nhé.

```html
<style type="text/css">
  /* style.css */
  #red {
    color: red;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p id="red">Section paragraph</p>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

### Attribute selector

Ta sẽ thêm hai thẻ `a` vào trang HTML khi nãy. Ngoài ra ta sẽ đổi màu của các thẻ `a` có thuộc tính `href` là `https://google.com` thành màu xanh.

```html
<style type="text/css">
  /* style.css */
  #red {
    color: red;
  }
  a {
    color: green;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p id="red">Section paragraph</p>
      <a href="https://google.com">Google</a>
      <a href="https://facebook.com">Facebook</a>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

Ta có thể dùng attribute selector để chọn các phần tử có thuộc tính HTML như đã chỉ định. Các thuộc tính sẽ được viết bên trong dấu ngoặc vuông `[]` và dùng dấu `=` để chỉ ra rằng thuộc tính đó có giá trị như đã chỉ định. Ví dụ, ta muốn chọn các thẻ `a` có thuộc tính `href` là `https://google.com` thì ta sẽ viết như sau:

```css
a[href="https://google.com"] {
  color: green;
}
```

Sửa vào thẻ `a` trong trang HTML thì ta sẽ thấy chỉ mỗi thẻ `a` có thuộc tính `href` là `https://google.com` mới có màu xanh.

```html
<style type="text/css">
  /* style.css */
  #red {
    color: red;
  }
  a[href="https://google.com"] {
    color: green;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p id="red">Section paragraph</p>
      <a href="https://google.com">Google</a>
      <a href="https://facebook.com">Facebook</a>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

Lưu ý nếu ta không chỉ định giá trị cho thuộc tính thì nó sẽ chọn tất cả các phần tử có thuộc tính đó. Ví dụ, ta muốn chọn tất cả các thẻ `a` có thuộc tính `href` thì ta sẽ viết như sau:

```css
a[href] {
  color: green;
}
```

Hoặc ta cũng có thể bỏ `a` đi, nó sẽ chọn tất cả các phần tử có thuộc tính `href`.

```css
[href] {
  color: green;
}
```

Attribute selector cũng có các cú pháp đặc biệt để giá trị khớp với một pattern nào đó. Một số cú pháp phổ biến là:

- `[attr^=value]`: chọn các phần tử có thuộc tính `attr` bắt đầu bằng `value`.
- `[attr$=value]`: chọn các phần tử có thuộc tính `attr` kết thúc bằng `value`.
- `[attr*=value]`: chọn các phần tử có thuộc tính `attr` chứa `value`.

### Combinator

Combinator là cách ta kết hợp các selector với nhau dựa trên vị trí của nó trên cây DOM. Ta có các loại combinator sau.

#### Descendant combinator

Ta dùng một dấu cách, theo format `selector1 selector2` để chọn phần tử `selector2` bên trong phần tử `selector1` (không nhất thiết là con trực tiếp của `selector1`). Ví dụ, ta muốn chọn thẻ `p` bên trong `footer` thì ta sẽ viết như sau:

```css
section p {
  color: red;
}
```

Thử trên trang HTML xem sao.

```html
<style type="text/css">
  /* style.css */
  footer p {
    color: red;
  }
  a[href="https://google.com"] {
    color: green;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p id="red">Section paragraph</p>
      <a href="https://google.com">Google</a>
      <a href="https://facebook.com">Facebook</a>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
      <div>
        <p>Div paragraph</p>
      </div>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

Ta có thể thấy cả Footer paragraph và Div paragraph đều có màu chữ đỏ vì cả hai đều là phần tử `p` bên trong `footer`.

#### Child combinator

Ta dùng dấu `>` để chọn phần tử `selector2` là con trực tiếp của phần tử `selector1`. Ví dụ, ta muốn chọn thẻ `p` là con trực tiếp của `footer` thì ta sẽ viết như sau:

```css
footer > p {
  color: red;
}
```

Trên trang HTML thì ta sẽ thấy chỉ có Footer paragraph có màu chữ đỏ vì nó là phần tử `p` là con trực tiếp của `footer`.

```html
<style type="text/css">
  /* style.css */
  footer > p {
    color: red;
  }
  a[href="https://google.com"] {
    color: green;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p id="red">Section paragraph</p>
      <a href="https://google.com">Google</a>
      <a href="https://facebook.com">Facebook</a>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
      <div>
        <p>Div paragraph</p>
      </div>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

#### Sibling combinator

Ta dùng dấu `~` để chọn phần tử `selector2` là anh em cùng cấp của phần tử `selector1` và nằm ngay sau `selector1`. Ví dụ như thẻ `section` có 4 phần tử con là `h2`, `p`, `a`, `a` thì tất cả chúng là anh em cùng cấp của nhau. Ta muốn chọn phần tử `a` nằm cùng cấp với `h2` thì ta sẽ viết như sau:

```css
h2 ~ a {
  color: red;
}
```

Ở đây cả hai `a` trong `section` đều là anh em cùng cấp của `h2` nên cả hai đều có màu chữ đỏ.

```html
<style type="text/css">
  /* style.css */
  h2 ~ a {
    color: red;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p id="red">Section paragraph</p>
      <a href="https://google.com">Google</a>
      <a href="https://facebook.com">Facebook</a>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
      <div>
        <p>Div paragraph</p>
      </div>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>

#### Adjacent sibling combinator

Ta dùng dấu `+` để chọn phần tử `selector2` là anh em cùng cấp của phần tử `selector1` và nằm ngay sau `selector1`. Xét trong thẻ `section`, ta không thể chọn `a` dựa trên `h2` được vì chúng không nằm cạnh nhau. Ta có thể chọn `p` dựa trên `h2` như sau:

```css
h2 + p {
  color: red;
}
```

Vì `footer` cũng có cặp `h2` và `p` cùng tính chất nên `p` trong `footer` cũng có màu chữ đỏ (không phải `p` trong `div`).

```html
<style type="text/css">
  /* style.css */
  h2 + p {
    color: red;
  }
</style>

<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Selectors</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
    <h1>Top Level Heading</h1>

    <section>
      <h2>Section Heading</h2>
      <p id="red">Section paragraph</p>
      <a href="https://google.com">Google</a>
      <a href="https://facebook.com">Facebook</a>
    </section>

    <footer>
      <h2>Footer Heading</h2>
      <p>Footer paragraph</p>
      <div>
        <p>Div paragraph</p>
      </div>
    </footer>
  </body>
</html>
```
<apprun-play style="height:300px"></apprun-play>
