# Cơ bản về HTML

## HTML là gì?

**HTML** là viết tắt của **HyperText Markup Language** - Ngôn ngữ Đánh dấu Siêu văn bản. Ta có thể chia nó thành 2 phần:

- **HyperText**: Siêu văn bản ở đây đơn giản là văn bản có thể chứa liên kết (link) tới các văn bản khác.
- **Markup Language**: Ngôn ngữ đánh dấu ở đây là ngôn ngữ được dùng để chú thích cho một dạng văn bản hay nội dung gì đó. Trong trường hợp của HTML, ta sẽ chú thích nội dung của trang web để trình duyệt hiểu được cách hiển thị nó.

Ví dụ, ta muốn có một trang blog như sau:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <h1>My Blog Post</h1>
    <h2>Day One</h2>
    <p>This is a paragraph for day 1.</p>
    <h2>Day Two</h2>
    <p>This is a paragraph for day 2.</p>
  </body>
</html>
```
<apprun-play style="height:300px" hide_src="true" hide_button="true"></apprun-play>

Người bình thường có thể nhìn vào đó và hiểu được nội dung. Ta có thể thấy rằng có một cái tiêu đề nằm trên, sau đó đến hai phần nhỏ hơn nằm dưới. Tuy nhiên, trình duyệt không hiểu được nó. Vậy nên ta cần một cách để chú thích cho trình duyệt hiểu được nó. Đó chính là HTML.

Như vậy ta sẽ có một cái tiêu đề  (heading) ở trên cùng. Sau đó ta có hai phần, mỗi phần bao gồm một cái tiêu đề nhỏ hơn, và một đoạn văn bản. Nhưng mà chú thích mấy cái này cho trình duyệt kiểu gì, cú pháp như thế nào? Ta sẽ dùng các thẻ (tag) trong HTML.

## Thẻ HTML là gì?

Thẻ (tag) trong HTML là một cách chú thích nội dung trong HTML.

Ví dụ ta có một đoạn văn bản "Hello World" và muốn nó nằm trong một tag văn bản, ký hiệu bằng chữ `p`. Ta mở thẻ bằng dấu `<`, sau đó là tên thẻ, và kết thúc thẻ bằng dấu `>`. Tiếp đến là nội dung của thẻ, và cuối cùng là thẻ đóng, giống như thẻ mở nhưng thêm dấu `/` trước tên thẻ. Ta gọi sự kết hợp giữa thẻ và nội dung là một phần tử (element).

Ví dụ đầy đủ sẽ như sau:

```html
<p>Hello World</p>
```

Nhìn chung, phần lớn các thẻ sẽ theo cấu trúc này, nhưng cũng có một số thẻ không cần thẻ đóng, chúng được gọi là thẻ tự đóng (self-closing tag). Đây là các thẻ không có nội dung, bởi vậy nên nó không cần bọc cái gì cả và cũng chả có nội dung gì hết. Ví dụ như trong trang blog của chúng ta, ta muốn chia hai phần "Day One" và "Day Two" bằng một đường ngang. Ta sẽ dùng thẻ tự đóng `<hr>`. Chẳng có lý do gì để có nội dung trong cái thẻ này nên ta không cần thẻ đòng mà chỉ cần thẻ mở thôi.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <h1>My Blog Post</h1>
    <h2>Day One</h2>
    <p>This is a paragraph for day 1.</p>
    <hr>
    <h2>Day Two</h2>
    <p>This is a paragraph for day 2.</p>
  </body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

Tuy nhiên, đôi khi bạn sẽ cảm thấy khá bối rối khi đọc code HTML vì bạn có thể thấy một thẻ mà không biết có thẻ đóng hay không. Hãy yên tâm là khi bạn bắt đầu quen dần với các loại thẻ trong HTML, bạn sẽ tự nhiên biết thẻ nào là thẻ tự đồng và thẻ nào cần thẻ đóng. Dù vậy thì ta vẫn có cú pháp riêng cho thẻ tự đóng, đó là thêm dấu `/` trước dấu `>`. Ví dụ với thẻ `hr` thì ta có thể viết là `<hr/>`. Dùng cách nào là tùy bạn thôi, nhưng hãy nhất quán nhé.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <h1>My Blog Post</h1>
    <h2>Day One</h2>
    <p>This is a paragraph for day 1.</p>
    <hr/>
    <h2>Day Two</h2>
    <p>This is a paragraph for day 2.</p>
  </body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

## Thuộc tính trong thẻ

Thuộc tính (attribute) trong thẻ là một cách để thêm thông tin cho thẻ, thứ sẽ thay đổi cách trình duyệt hiển thị thẻ. Nó giống kiểu tham số trong các hàm trong các ngôn ngữ lập trình ấy.

Giờ ví dụ ta muốn có một cái checkbox đi, ta sẽ sử dụng thẻ `input` để nói cho trình duyệt biết rằng đây là một ô để người dùng chọn hoặc bỏ chọn. Thẻ `input` là một thẻ tự đóng, nhưng nó khá đặc biệt và được dùng trong tất cả các loại input, ví dụ như để nhập văn bản hay chọn ngày tháng gì đấy. Tuy nhiên ở đây ta cần nó phải là một cái checkbox, và thuộc tính sẽ giúp ta thực hiện điều đó.

Thuộc tính sẽ nằm trong thẻ mở hoặc với thẻ tự đóng như `input`, nó nằm trong thẻ duy nhất ta có. Các thuộc tính sẽ theo dạng cặp khóa - giá trị (key - value), vậy nên ta sẽ bắt đầu với tên thuộc tính, ở đây là `type`, đến dầu `=`, và cuối cùng là giá trị của thuộc tính, ở đây là `checkbox` nằm trong cặp dấu nháy kép `"`.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <input type="checkbox"/>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Việc này sẽ cho trình duyệt biết rằng ta không muốn một ô input nào đó mà chỉ có thể là checkbox thôi. Có rất nhiều loại thuộc tính cho các thẻ, nhưng bạn không cần phải nhớ hết chúng đâu, khi quen dần với HTML, bạn sẽ tự nắm được chúng. Nếu quá khó nhớ, hãy cứ tra cứu trên mạng thôi.

## Comment trong HTML

Ta mở comment trong HTML bằng cách sử dụng cặp dấu `<!--` để bắt đầu comment và `-->` để kết thúc comment. Bất cứ thứ nào nằm giữa cặp dấu này sẽ không được trình duyệt hiển thị.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <!-- This is a comment -->
    <h1>My Blog Post</h1>
    <h2>Day One</h2>
    <p>This is a paragraph for day 1.</p>
    <hr/>
    <h2>Day Two</h2>
    <p>This is a paragraph for day 2.</p>
  </body>
</html>
```
<apprun-play style="height:300px" hide_button="true"></apprun-play>

## Whitespace trong HTML

Whitespace hướng đến các ký tự trống, như khoảng trắng (space), tab, hay dòng mới (new line). Whitespace khá thú vị vì nó thường bị bỏ qua trong HTML, nghĩa là nhiều whitespace sẽ được coi là một khoảng trắng duy nhất. Ví dụ, ta có hai đoạn văn bản như sau:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <p>This is a paragraph for day 1.</p>
    <p>This
    is
    a
    paragraph
    for
    day
    2.</p>
  </body>
</html>
```
<apprun-play hide_button="true"></apprun-play>

Bạn có thể nghĩ cái số 2 sẽ hiển thị trên nhiều dòng, nhưng chúng lại y hệt nhau trên trình duyệt vì các whitespace đã được coi như một khoảng trắng. Trông khá là kỳ nhưng rất có ích vì nó cho phép ta canh dòng code HTML để xử lý các thẻ lồng nhau và ta có thể chia một thành nhiều dòng code để dễ đọc hơn. Tất nhiên cũng có cách hiện tất cả whitespace nhưng ta sẽ thực hiện trong các phần sau.

## Một file HTML hoàn chỉnh bao gồm những gì?

Dòng đầu tiên của một file HTML sẽ là DOCTYPE, chính xác là `<!DOCTYPE html>`. DOCTYPE là cách để cho trình duyệt biết rằng ta muốn sử dụng phiên bản HTML mới nhất. Bạn cũng có thể dùng phiên bản HTML cũ hơn nhưng thực tế hiếm ai làm vậy.

Tiếp theo ta sẽ mở thẻ `html`. Đây sẽ là gốc của văn bản, nó sẽ chứa tất cả các phần tử HTML khác. Cụ thể hơn, thẻ `html` sẽ luôn chứa hai thẻ con là `head` và `body`.

Thẻ `head` sẽ chứa các metadata của trang web, như tiêu đề, các file CSS, JavaScript, hay các thẻ khác như `meta`, `link`, `script`,... Các thông tin này không được hiển thị trên trang web nhưng giúp trình duyệt hiểu được trang web của bạn hơn. Thẻ `body` sẽ chứa tất cả các phần tử hiển thị trên trang.

Trong thẻ `head`, có một thẻ bắt buộc phải có, đó là thẻ `title`. Thẻ này sẽ chứa tiêu đề của trang web, nó sẽ hiển thị trên tab của trình duyệt, nó cũng có thể xuất hiện dưới dạng tiêu đề trang web trong các kết quả tìm kiếm ở trên mạng.

Trong thẻ `body`, bạn có thể thêm bất cứ phần tử nào bạn muốn, như tiêu đề, đoạn văn bản, hình ảnh, video, hay bất cứ thứ gì bạn muốn hiển thị trên trang web.

Giờ ta có thể đưa file HTML cho trình duyệt để hiển thị rồi.
