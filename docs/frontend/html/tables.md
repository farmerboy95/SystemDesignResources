# Bảng trong HTML

Bảng là một phần quan trọng của HTML. Bạn có thể sử dụng bảng để hiển thị dữ liệu dưới dạng hàng và cột. Bảng HTML được tạo bằng cách sử dụng thẻ `<table>`. Mỗi bảng HTML bao gồm một hoặc nhiều hàng, mỗi hàng chứa một hoặc nhiều ô. Mỗi ô trong bảng được tạo bằng cách sử dụng thẻ `<td>`. Một hàng trong bảng được tạo bằng cách sử dụng thẻ `<tr>`.

Dưới đây là một ví dụ về cách tạo một bảng đơn giản trong HTML:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <table>
      <tr>
        <td>100</td>
        <td>200</td>
      </tr>
      <tr>
        <td>300</td>
        <td>400</td>
      </tr>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Trong ví dụ trên, chúng ta đã tạo một bảng đơn giản với hai hàng và hai cột. Mỗi ô trong bảng chứa một số nguyên. Bạn có thể thấy rằng mỗi ô được bao bởi thẻ `<td>`, mỗi hàng được bao bởi thẻ `<tr>`, và bảng được bao bởi thẻ `<table>`.

Ta có thể thêm tiêu đề cho bảng bằng cách sử dụng thẻ `<th>`. Thuộc tính `scope` của thẻ `<th>` có thể  là `col` (cột) hoặc `row` (hàng) để chỉ ra xem tiêu đề đó áp dụng cho cột hay hàng.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <table>
      <tr>
        <th scope="col">Column 0</th>
        <th scope="col">Column 1</th>
        <th scope="col">Column 2</th>
      </tr>
      <tr>
        <th scope="row">Row 0</th>
        <td>100</td>
        <td>200</td>
      </tr>
      <tr>
        <th scope="row">Row 1</th>
        <td>300</td>
        <td>400</td>
      </tr>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Trong ví dụ trên, `scope` không ảnh hưởng đến cách hiển thị của bảng, nó chỉ giúp trình duyệt hiểu được ý nghĩa của tiêu đề đó, trong đó `col` là tiêu đề cột và `row` là tiêu đề hàng.

Để biểu thị code tốt hơn về mặt ngữ nghĩa, bạn có thể sử dụng thẻ `<thead>`, `<tbody>`, và `<tfoot>` để phân chia bảng thành các phần khác nhau. Thẻ `<thead>` được sử dụng để chứa các tiêu đề cột, thẻ `<tbody>` chứa dữ liệu chính của bảng, và thẻ `<tfoot>` chứa dữ liệu tổng kết hoặc thông tin khác. Ta cũng có thể thêm tiêu đề cho bảng bằng cách sử dụng thẻ `<caption>`.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <table>
      <caption>Table Caption</caption>
      <thead>
        <tr>
          <th scope="col">Column 0</th>
          <th scope="col">Column 1</th>
          <th scope="col">Column 2</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">Row 0</th>
          <td>100</td>
          <td>200</td>
        </tr>
        <tr>
          <th scope="row">Row 1</th>
          <td>300</td>
          <td>400</td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <th>Total</th>
          <td>400</td>
          <td>600</td>
        </tr>
      </tfoot>
    </table>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Thuộc tính `colspan` và `rowspan` cho phép bạn kết hợp nhiều ô thành một ô lớn hơn. Thuộc tính `colspan` cho biết số cột mà ô đó sẽ trải dài qua, và thuộc tính `rowspan` cho biết số hàng mà ô đó sẽ trải dài qua.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <table>
      <tr>
        <td>100</td>
        <td>200</td>
        <td rowspan="2">300</td>
      </tr>
      <tr>
        <td>400</td>
        <td>500</td>
      </tr>
      <tr>
        <td colspan="3">600</td>
        <td>700</td>
      </tr>
    </table>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Ta có thể thấy rằng ô chứa số 300 trải dài qua 2 hàng, ô chứa số 600 trải dài qua 3 cột.

Một thẻ khá hay nữa là thẻ `<colgroup>`, nó cho phép bạn nhóm các cột lại với nhau và áp dụng các thuộc tính cho nhóm cột đó. Thẻ `<col>` được sử dụng bên trong thẻ `<colgroup>` để định nghĩa một cột.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <table>
      <colgroup>
        <col style="background-color: lightblue;">
        <col style="background-color: lightgreen;">
        <col style="background-color: lightyellow;" span="2">
      </colgroup>
      <tr>
        <td>100</td>
        <td>200</td>
        <td>300</td>
        <td>400</td>
      </tr>
      <tr>
        <td>400</td>
        <td>500</td>
        <td>600</td>
        <td>700</td>
      </tr>
    </table>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Trong ví dụ trên, ta đã nhóm các cột lại với nhau bằng thẻ `<colgroup>`, và áp dụng màu nền khác nhau cho từng cột bằng cách sử dụng thẻ `<col>`. Hai cột cuối cùng được nhóm lại với nhau và trải dài qua 2 cột bằng thuộc tính `span`.
