# Các thẻ HTML cơ bản

Trong bài viết này, ta sẽ đi qua một số các loại thẻ HTML quan trọng và cơ bản mà bạn sẽ sử dụng thường xuyên khi phát triển website. Một điều quan trọng cần nhớ đó là, việc dùng thẻ theo ngữ nghĩa (semantic) rất quan trọng, nghĩa là việc ta dùng thẻ với nội dung phản ánh đúng chức năng của thẻ. Thứ hai là, các thẻ này cung cấp cấu trúc cho nội dung, chứ không chỉ là layout và style. Thực ra, các thẻ chẳng cung cấp layout và style nào cả, đó chỉ là các layout và style mặc định của trình duyệt thôi, HTML không định nghĩa các thẻ phải trông như thế nào. Vì thế, trong bài viết này, ta sẽ không quan tâm đến output mà chỉ quan tâm đến cấu trúc nội dung.

## Thẻ `<p>`

Thẻ `<p>` dùng để đánh dấu một đoạn văn bản. Ta đưa nội dung vào bên trong thẻ `<p>`. Điểm cần lưu ý ở đây là nội dung không nhất thiết phải là văn bản mà có thể là những thứ như hình ảnh, nếu nó là một phần của một đoạn văn bản.

```html
<p>Đây là một đoạn văn bản.</p>
```

## Các thẻ tiêu đề

Tiêu đề chính của website thường được đánh dấu bằng thẻ `<h1>`, đây là thẻ tiêu đề cấp cao nhất. Tiếp đó ta có thẻ `<h2>` cho tiêu đề con, tiếp tục cho đến `<h6>`.

```html
<h1>Đây là tiêu đề cấp 1</h1>
<h2>Đây là tiêu đề cấp 2</h2>
<h3>Đây là tiêu đề cấp 3</h3>
<h4>Đây là tiêu đề cấp 4</h4>
<h5>Đây là tiêu đề cấp 5</h5>
<h6>Đây là tiêu đề cấp 6</h6>
```

Tuy nhiên, cần lưu ý rằng kích cỡ font và cấp tiêu đề không liên quan đến nhau. Ta không nên dùng các thẻ tiêu đề để thay đổi kích cỡ font. Kích cỡ font được trình duyệt chọn chỉ là kích cỡ mặc định và ta có thể thay đổi được. Ta nên chọn thẻ tiêu đề dựa trên cấp độ quan trọng của nội dung.

## Thẻ `<img>`

Thẻ `<img>` là một thẻ tự đóng, được dùng để chèn hình ảnh vào trang web. Nó có hai thuộc tính, `src` để chỉ đường dẫn đến hình ảnh và `alt` để cung cấp một mô tả cho hình ảnh, cái này sẽ được dùng bởi trình đọc màn hình khi ảnh không hiển thị được.

```html
<img src="path/to/image.jpg" alt="dog">
```

## Các loại thẻ danh sách

Có hai loại danh sách chính trong HTML, danh sách không sắp xếp (unordered list) và danh sách sắp xếp (ordered list). Danh sách không sắp xếp được đánh dấu bằng thẻ `<ul>`, còn danh sách sắp xếp được đánh dấu bằng thẻ `<ol>`. Mỗi mục trong danh sách được đánh dấu bằng thẻ `<li>`. Theo mặc định của trình duyệt, với danh sách không sắp xếp, mỗi mục sẽ được đánh dấu bằng dấu chấm đen, còn với danh sách sắp xếp, mỗi mục sẽ được đánh số.

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>

<ol>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ol>
```

## Thẻ `<pre>`

Thẻ `<pre>` dùng để hiển thị nội dung một cách nguyên thủy, không xử lý khoảng trắng và xuống dòng (whitespace). Thẻ này thường được dùng để hiển thị mã nguồn.

```html
<pre>
  if (true) {
    console.log('Hello, world!');
  }
</pre>
```

## Thẻ `<br>`

Thẻ `<br>` là một thẻ tự đóng, dùng để tạo một dòng mới trong văn bản. Tuy nhiên, thẻ này không nên được dùng để giãn cách các phần tử, mà nên dùng CSS để làm điều đó.

```html
<p>Đây là dòng 1<br>Đây là dòng 2</p>
```

## Thẻ `<hr>`

Thẻ `<hr>` là một thẻ tự đóng, dùng để phân chia các phần của trang. Theo mặc định của trình duyệt, thẻ này sẽ tạo ra một đường ngang.

```html
<p>Đây là phần 1</p>
<hr>
<p>Đây là phần 2</p>
```

## Thẻ `<a>`

Thẻ `<a>` dùng để tạo một liên kết đến một trang web khác hoặc một tài nguyên khác. Thuộc tính `href` của thẻ này chứa đường dẫn đến trang web hoặc tài nguyên đó. Ngoài ra ta có thể sử dụng thuộc tính `target="_blank"` để mở liên kết trong một tab mới.

```html
<a href="https://www.google.com">Google</a>
```

## Thẻ `<em>`

Thẻ `<em>` dùng để nhấn mạnh một phần của văn bản. Theo mặc định, trình duyệt sẽ hiển thị nội dung bên trong thẻ này bằng cách in nghiêng.

```html
<p>Đây là một <em>đoạn văn bản nhấn mạnh</em>.</p>
```

## Thẻ `<strong>`

Thẻ `<strong>` cũng được dùng để nhấn mạnh một phần của văn bản. Theo mặc định, trình duyệt sẽ hiển thị nội dung bên trong thẻ này bằng cách in đậm.

```html
<p>Đây là một <strong>đoạn văn bản nhấn mạnh</strong>.</p>
```
