# Form trong HTML

Form, hay biểu mẫu, là một phần quan trọng của mọi trang web. Chúng cho phép người dùng nhập dữ liệu và gửi nó đến máy chủ để xử lý. Trong HTML, form được tạo bằng thẻ `<form>`. Trong bài viết này, chúng ta sẽ tìm hiểu cách tạo form trong HTML và các phần tử form phổ biến.

Trong form, ta có thể sử dụng thẻ `<input>` để tạo các trường nhập dữ liệu. Trông nó sẽ như sau:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form>
      <input type="text">
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Trong ví dụ trên, ta chỉ thấy một trường nhập dữ liệu, ta có thể tương tác với trường này thông qua bàn phím. Ở đây ta sẽ muốn cho người khác biết họ sẽ phải nhập cái gì, ta dùng thẻ `<label>`. Hãy tạo thử một trường nhập username và một trường nhập password:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form>
      <label>Username:</label>
      <input type="text">
      <label>Password:</label>
      <input type="password">
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Thuộc tính `type` của thẻ `<input>` cho phép bạn chỉ định loại dữ liệu mà trường nhập sẽ chứa, ví dụ với `password` thì dữ liệu sẽ được ẩn đi.

Để giúp trình duyệt hiểu hơn về form, cụ thể là nó sẽ không biết label cho mật khẩu sẽ dành cho input trước hay sau nó, ta dùng thuộc tính `for` cho thẻ `<label>` và `id` cho thẻ `<input>`. Ví dụ:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form>
      <label for="username">Username:</label>
      <input type="text" id="username">
      <label for="password">Password:</label>
      <input type="password" id="password">
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Kết quả trên màn hình sẽ không thay đổi, nhưng trình duyệt sẽ hiểu được rằng label `Username` sẽ dành cho input có `id` là `username`, và tương tự với `Password`.

Tiếp theo là thuộc tính `placeholder`, nó cho phép bạn hiển thị một dòng chữ mô tả trước khi người dùng nhập dữ liệu. Ví dụ:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form>
      <label for="username">Username:</label>
      <input type="text" id="username" placeholder="Enter your username">
      <label for="password">Password:</label>
      <input type="password" id="password" placeholder="Enter your password">
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Ta sẽ muốn có một cái nút để gửi form, ta sử dụng thẻ `<button>` với thuộc tính `type="submit"`. Ngoài ra ta cũng có thể sử dụng thẻ `<input>` với `type="submit"`, cách nào cũng được.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form>
      <label for="username">Username:</label>
      <input type="text" id="username" placeholder="Enter your username">
      <label for="password">Password:</label>
      <input type="password" id="password" placeholder="Enter your password">
      <button type="submit">Submit</button>
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Tuy nhiên khi gửi lên server thì server sẽ không biết username có tên là gì, password có tên là gì, ta cần thêm thuộc tính `name` cho thẻ `<input>`. Ví dụ:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form>
      <label for="username">Username:</label>
      <input type="text" id="username" name="username" placeholder="Enter your username">
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" placeholder="Enter your password">
      <button type="submit">Submit</button>
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Với thẻ `<form>` ta cũng có thể thêm thuộc tính `action` để chỉ định URL mà form sẽ gửi dữ liệu đến, và thuộc tính `method` để chỉ định phương thức gửi dữ liệu, mặc định là `GET`. Ví dụ:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form action="/submit" method="POST">
      <label for="username">Username:</label>
      <input type="text" id="username" name="username" placeholder="Enter your username">
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" placeholder="Enter your password">
      <button type="submit">Submit</button>
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Ta cũng có thể xác thực dữ liệu nhập vào form bằng cách sử dụng thuộc tính `required` cho thẻ `<input>`, nó sẽ yêu cầu người dùng nhập dữ liệu vào trường đó trước khi gửi form, hay thuộc tính `minlength` để yêu cầu dữ liệu nhập vào có độ dài tối thiểu nào đó. Ví dụ:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form action="/submit" method="POST">
      <label for="username">Username:</label>
      <input type="text" id="username" name="username" placeholder="Enter your username" required>
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" placeholder="Enter your password" required minlength="6">
      <button type="submit">Submit</button>
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Ngoài ra, ta còn có các loại type khác cho thẻ `<input>` như `date`, `checkbox`, và `radio`. Với `radio` thì hơi đặc biệt hơn một chút, vì nó lấy một trong một nhóm các lựa chọn, nên ta cần phải gán cùng một `name` cho các `radio` cùng một nhóm. Ví dụ:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form action="/submit" method="POST">
      <label for="username">Username:</label>
      <input type="text" id="username" name="username" placeholder="Enter your username" required>
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" placeholder="Enter your password" required minlength="6">
      <input type="date">
      <input type="checkbox">
      <label for="dog">Dog</label>
      <input type="radio" id="dog" name="pet" value="dog">
      <label for="cat">Cat</label>
      <input type="radio" id="cat" name="pet" value="cat">
      <label for="bird">Bird</label>
      <input type="radio" id="bird" name="pet" value="bird">
      <button type="submit">Submit</button>
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Các radio button có vẻ như không dễ nhận ra là chung một nhóm cho lắm, ta có thể sử dụng thẻ `<fieldset>` và `<legend>` để nhóm chúng lại. Ví dụ:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <form action="/submit" method="POST">
      <label for="username">Username:</label>
      <input type="text" id="username" name="username" placeholder="Enter your username" required>
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" placeholder="Enter your password" required minlength="6">
      <input type="date">
      <input type="checkbox">
      <fieldset>
        <legend>Pet</legend>
        <label for="dog">Dog</label>
        <input type="radio" id="dog" name="pet" value="dog">
        <label for="cat">Cat</label>
        <input type="radio" id="cat" name="pet" value="cat">
        <label for="bird">Bird</label>
        <input type="radio" id="bird" name="pet" value="bird">
      </fieldset>
      <button type="submit">Submit</button>
    </form>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>
