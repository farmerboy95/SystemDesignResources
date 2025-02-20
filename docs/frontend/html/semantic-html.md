# Semantic HTML

## Semantic HTML là gì?

Khi nói về Semantic HTML, ta muốn trả lời câu hỏi rằng, file HTML của ta có dễ hiểu cả khi không có trình duyệt không? Có rất nhiều trường hợp khi ai đó muốn xem code HTML hay tương tác với code HTML của bạn, nhưng họ không thể xem output của code thông qua trình duyệt. Ta muốn chắc chắn rằng code được viết ra có thể hiểu được mà không cần trình duyệt để phục vụ những nhu cầu này. Ví dụ, khi sử dụng trình đọc màn hình (screen reader), người sử dụng (khả năng cao) sẽ không thể thấy trực tiếp trang web trên trình duyệt, và ta cần phải đảm bảo rằng HTML phải được hiểu đúng cách, qua đó giúp trình đọc màn hình có thể đọc được nội dung một cách chính xác.

Trong phần trước ta đã biết cách dùng các thẻ (tag) để đánh dấu nội dung. Ta cần một cách để cho thấy nội dung nào được liên kết với nội dung nào. Ta cần một cách nhóm chúng lại, và đây là lúc để sử dụng các loại thẻ nhóm ngữ nghĩa (semantic grouping tag).

Tóm lại, Semantic HTML là cách viết HTML mà ta sử dụng các thẻ HTML ứng với nội dung chứa trong nó. Ví dụ, nếu bạn muốn viết một tiêu đề, bạn sẽ sử dụng thẻ `<h1>`, nếu bạn muốn viết một đoạn văn, bạn sẽ sử dụng thẻ `<p>`, và cứ như vậy. HTML mất đi tính ngữ nghĩa khi thẻ HTML bị dùng sai mục đích, hay các thẻ chung như `<div>` hay `<span>` bị sử dụng quá nhiều thay vì các thẻ ngữ nghĩa.

## Một số loại thẻ thông dụng

### Thẻ `<article>`

Thẻ `<article>` được sử dụng để đánh dấu một phần nội dung độc lập, tức là nội dung đó có thể tồn tại mà không cần phụ thuộc vào nội dung xung quanh. Ví dụ, một bài viết trên blog, một bài báo, một bài viết trên forum, hay một bài viết trên trang web.

### Thẻ `<section>`

Thẻ `<section>` được sử dụng để đánh dấu một phần nội dung trong một trang web. Điểm khác biệt ở đây so với thẻ `<article>` là thẻ `<section>` không phải là một phần nội dung độc lập, mà nó là một phần của nội dung chính. Đôi khi ta thấy thẻ `<section>` là một phần của thẻ `<article>`, lúc khác lại thấy nó độc lập so với thẻ `<article>`, nhưng lại phụ thuộc vào các `<section>` khác.

### Thẻ `<header>`

Thẻ `<header>` được sử dụng để đánh dấu phần header của một trang web hoặc một phần header của một phần nội dung. Header thường chứa các thông tin như logo, tiêu đề, menu, hay các thông tin khác.

### Thẻ `<main>`

Thẻ `<main>` được sử dụng để đánh dấu phần nội dung chính của một trang web. Thẻ `<main>` chỉ được sử dụng một lần trong một trang web.

### Thẻ `<nav>`

Thẻ `<nav>` được sử dụng để đánh dấu phần navigation của một trang web. Navigation thường chứa các liên kết đến các trang khác trong trang web. Một trang có thể có nhiều thẻ `<nav>`.

### Thẻ `<aside>`

Thẻ `<aside>` được sử dụng để đánh dấu phần nội dung không quan trọng hoặc không cần thiết cho nội dung chính. Thẻ `<aside>` thường chứa các thông tin bổ sung, như quảng cáo, biểu đồ, hay các thông tin khác.

### Thẻ `<footer>`

Thẻ `<footer>` được sử dụng để đánh dấu phần footer của một trang web hoặc một phần footer của một phần nội dung. Footer thường chứa các thông tin như thông tin liên hệ, thông tin bản quyền, hay các thông tin khác.

## Ví dụ

```html
<!DOCTYPE html>
<html>
    <head>
        <title>VR Article</title>
    </head>
  
    <body>
        <div>
            <header>
                <h1>Experience VR</h1>
                <p>A simple blog about virtual reality experience</p>
       
                <nav>
                    <ul>
                    <li><a href="/">Home</a></li>
                    <li><a href="/#about">About</a></li>
                    <li><a href="/#articles">Articles</a></li>
                    <li><a href="/#contact">Contact</a></li>
                    </ul>
                </nav>
            </header>
      
            <article id="vr-articles">
                <header>
                    <h2>VR Article </h2>
                    <p>By: Harley</p>
                    <p>Publish: June 19, 2018</p>
                </header>
        
                <img src="../../img/vr-user.jpg" alt="User trying a VR headset">
        
                <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce finibus urna lacus, ut lacinia elit pretium a. Praesent rutrum ac ipsum vitae rhoncus. Nam non molestie purus.</p>
                <aside>
                    <q>This is a pull quote from the VR Article...</q>
                </aside>
                <p>Nunc imperdiet, dui in varius eleifend, magna enim imperdiet felis, at ultricies magna metus vitae ante. Nulla in porttitor nibh. Mauris non libero in massa porta varius non sed magna. Donec ac mauris mattis, viverra turpis ac, dictum arcu.</p>
                <p>Vivamus molestie laoreet viverra. Ut ac fringilla ex. Donec at nisl semper, commodo mi maximus, fermentum nisi. Duis bibendum gravida ante sit amet consectetur. Curabitur ac est id justo euismod porta quis ac arcu.</p>
            </article>
     
            <aside>
                <h3>More Article About VR</h3>
                <ol>
                    <li><a href="#">Make a VR Game</a></li>
                    <li><a href="#">Learn VR in Unity</a></li>
                    <li><a href="#">Build Users Interfaces in VR</a></li>
                </ol>
                <blockquote>
                    "Virtual reality was once the dream of science fiction. But the internet was also once a dream, and so were computers and smartphones. The future is coming."  
                    <footer>
                        - <cite><a href="https://www.facebook.com/zuck/posts/10101319050523971">Mark Zuckerberg</a></cite>
                    </footer>
                </blockquote>
            </aside>
     
            <footer>
                <p>©2018 Experience VR, The Blog</p> 
            </footer>
        </div>
    </body>
</html>
```
<apprun-play style="height:1000px" hide_button="true"></apprun-play>

Lưu ý rằng trong ví dụ ở trên, ta sử dụng các thẻ ngữ nghĩa như `<header>`, `<nav>`, `<article>`, `<aside>`, và `<footer>` để đánh dấu các phần nội dung chứ không có bất kỳ layout hay style nào. Vì thế, đừng ngạc nhiên khi bạn không thấy các thẻ hiển thị ở vị trí mà bạn mong muốn trên trình duyệt.
