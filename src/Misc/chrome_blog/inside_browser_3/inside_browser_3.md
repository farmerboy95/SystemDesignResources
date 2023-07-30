# Bên trong các trình duyệt ngày nay (Phần 3)

## Nguồn

[Inside look at modern web browser (part 3) - Chrome Developers Blog](https://developer.chrome.com/blog/inside-browser-part3/)

## Bên trong Renderer Process

Đây là phần 3 trong series 4 phần về cách trình duyệt hoạt động. Trước đó, ta đã đi qua kiến trúc đa tiến trình và quá trình điều hướng. Trong phần này, ta sẽ xem điều gì xảy ra trong renderer process.

Renderer process liên quan đến nhiều khía cạnh của hiệu suất web. Vì có rất nhiều thứ xảy ra trong renderer process, phần này sẽ chỉ là khái quát chung. Nếu bạn muốn đi sâu hơn, [phần Hiệu suất của Web cơ bản](https://developers.google.com/web/fundamentals/performance/why-performance-matters/) có rất nhiều tài nguyên.

## Các renderer process xử lý nội dung web

Renderer process chịu trách nhiệm cho tất cả mọi thứ xảy ra trong một tab trên trình duyệt. Trong renderer process, main thread sẽ xử lý gần như mọi code bạn gửi cho user. Đôi khi các phần của code JavaScript được xử lý bởi các worker thread nếu bạn dùng một web worker hoặc một service worker. Các raster thread và compositor thread cũng được chạy trong renderer process để render trang một cách hiệu quả và mượt mà.

Công việc chủ yếu của renderer process là chuyển HTML, CSS và JavaScript thành một trang web mà người dùng có thể tương tác.

<figure markdown>
![Hình 1: Renderer process với một main thread, các worker thread, một compositor thread và một raster thread bên trong](figure1.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 1: Renderer process với một main thread, các worker thread, một compositor thread và một raster thread bên trong</figcaption>
</figure>

## Parsing (Phân tích cú pháp)

### Xây dựng DOM

Khi renderer process nhận một thông báo commit cho điều hướng và bắt đầu nhận dữ liệu HTML, main thread bắt đầu parse (phân tích cú pháp) chuỗi văn bản (HTML) và biến nó thành Mô hình Đối tượng Tài liệu - **D**ocument **O**bject **M**odel (**DOM**).

DOM là biểu diễn bên trong của trình duyệt về trang cũng như cấu trúc dữ liệu và API mà nhà phát triển web có thể tương tác qua JavaScript.

Parse một tài liệu HTML thành DOM được định nghĩa bởi [Tiêu chuẩn HTML](https://html.spec.whatwg.org/). Bạn có thể nhận thấy rằng việc cung cấp HTML cho trình duyệt không bao giờ gây ra lỗi. Ví dụ, thiếu thẻ đóng `</p>` là một HTML hợp lệ. Markup sai như `Hi! <b>I'm <i>Chrome</b>!</i>` (tag b đóng trước tag i) sẽ được xem như bạn viết `Hi! <b>I'm <i>Chrome</i></b><i>!</i>`. Điều này là do đặc tả HTML được thiết kế để linh hoạt xử lý các lỗi đó. Nếu bạn muốn biết chúng được thực hiện như thế nào, bạn có thể đọc phần "[Giới thiệu về xử lý lỗi và các trường hợp lạ trong parser](https://html.spec.whatwg.org/multipage/parsing.html#an-introduction-to-error-handling-and-strange-cases-in-the-parser)" của đặc tả HTML.

### Tải nguồn phụ

Một website thường dùng các tài nguyên bên ngoài như hình ảnh, CSS và JavaScript. Các file này cần được tải từ mạng hoặc cache. Main thread *có thể* yêu cầu từng cái một khi nó tìm ra các tài nguyên đó trong lúc parse để xây dựng DOM, nhưng để tăng tốc, "scanner tải trước" được chạy đồng thời. Nếu có những thứ như `<img>` hoặc `<link>` trong tài liệu HTML, scanner tải trước sẽ xem xét mã thông báo do HTML parser tạo và gửi request đến network thread trong browser process.

<figure markdown>
![Hình 2: Main thread parse HTML và xây dựng cây DOM](figure2.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 2: Main thread parse HTML và xây dựng cây DOM</figcaption>
</figure>

### JavaScript có thể chặn parsing

Khi HTML parser tìm thấy thẻ `<script>`, nó sẽ tạm dừng việc parse tài liệu HTML và phải tải, parse và thực thi code JavaScript. Vì sao? Bởi vì JavaScript có thể thay đổi tài liệu HTML với những thứ như `document.write()`, nó sẽ thay đổi toàn bộ cấu trúc DROM ([tổng quan về mô hình parsing](https://html.spec.whatwg.org/multipage/parsing.html#overview-of-the-parsing-model) trong đặc tả HTML có một sơ đồ rất đẹp). Đây là lý do vì sao HTML parser phải đợi JavaScript chạy trước khi tiếp tục parse tài liệu HTML. Nếu bạn tò mò về những thứ xảy ra trong quá trình thực thi JavaScript, [team V8 đã có những buổi toạ đàm và bài viết về vấn đề đó](https://mathiasbynens.be/notes/shapes-ics).

## Gợi ý cho trình duyệt rằng bạn muốn tải tài nguyên như thế nào

Có nhiều cách để các nhà phát triển ứng dụng web có thể gửi gợi ý đến trình duyệt để tải tài nguyên theo thứ tự nào đó. Nếu code JavaScript của bạn không dùng `document.write()`, bạn có thể thêm các thuộc tính [`async`](https://developer.mozilla.org/docs/Web/HTML/Element/script#attr-async) hay [`defer`](https://developer.mozilla.org/docs/Web/HTML/Element/script#attr-defer) cho thẻ `<script>`. Trình duyệt sẽ tải và chạy code JavaScript một cách bất đồng bộ và không làm gián đoạn việc parse. Bạn có thể dùng [JavaScript module](https://developers.google.com/web/fundamentals/primers/modules) nếu thấy hợp lý. `<link rel="preload">` là một cách để báo cho trình duyệt rằng tài nguyên là rất cần thiết cho việc điều hướng bây giờ và bạn muốn tải càng sớm càng tốt. Bạn có thể đọc thêm tại [Ưu tiên tài nguyên - Nhờ trình duyệt giúp bạn](https://developers.google.com/web/fundamentals/performance/resource-prioritization).

## Tính toán cho các style

DOM là chưa đủ để biết trang web sẽ trông như thế nào vì ta cần các style trong CSS. Main thread sẽ parse CSS và quyết định computed style cho từng node DOM. Đây là thông tin về loại style được áp dụng cho từng element dựa trên bộ chọn CSS. Bạn có thể xem thông tin này trong phần `computed` của DevTools.

<figure markdown>
![Hình 3: Main thread parse CSS để thêm các computed style vào](figure3.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 3: Main thread parse CSS để thêm các computed style vào</figcaption>
</figure>

Ngay cả khi nếu bạn không cung cấp CSS, mỗi DOM node vẫn có một style được tính toán. Tag `<h1>` được hiển thị to hơn tag `<h2>` và margin được định nghĩa cho mỗi element. Lý do là vì browser có CSS mặc định. Nếu bạn muốn biết CSS mặc định của Chrome trông như nào, [bạn có thể xem source code ở đây](https://cs.chromium.org/chromium/src/third_party/blink/renderer/core/html/resources/html.css).

## Layout

Giờ renderer process đã biết cấu trúc của tài liệu và style cho từng node, nhưng vẫn không đủ để render trang. Tưởng tượng bạn đang cố gắng miêu tả một bức vẽ cho bạn của mình qua điện thoại. "Có một hình tròn lớn màu đủ và một hình vuông nhỏ màu xanh" là không đủ thông tin để bạn của bạn biết chính xác bức vẽ trông thế nào.

<figure markdown>
![Hình 4: Một người đứng trước một bức vẽ, với kết nối điện thoại đến một người khác](figure4.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 4: Một người đứng trước một bức vẽ, với kết nối điện thoại đến một người khác</figcaption>
</figure>

Layout (bố cục) là một process để tìm hình dạng của các element. Main thread sẽ đi qua DOM và các computed style và tạo cây layout, nó sẽ có các thông tin như toạ độ x y và kích thước hộp giới hạn. Cây layout có cấu trúc tương tự cây DOM, nhưng nó chỉ chứa thông tin liên quan đến những gì hiển thị trên trang. Nếu `display: none` được áp dụng, element đó không nằm trong layout tree (tuy nhiên, element với `visibility: hidden` vẫn nằm trên cây layout). Tương tự, nếu một lớp giả với nội dung như `p::before{content:"Hi!"}` được áp dụng, nó sẽ nằm trong cây layout mặc dù nó không nằm trong DOM.

<figure markdown>
![Hình 5: Main thread chạy dọc cây DOM với computed style và tạo ra cây layout](figure5.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 5: Main thread chạy dọc cây DOM với computed style và tạo ra cây layout</figcaption>
</figure>

Xác định layout của trang là một việc khó. Ngay cả layout của trang đơn giản nhất như một đống block từ trên xuống dưới cũng phải xem xét độ lớn của font chữ và vị trí ngắt dòng vì chúng ảnh hưởng đến kích thước và hình dạng của đoạn văn; mà sau đó ảnh hưởng đến vị trí của đoạn sau, như video dưới đây.

<figure markdown>
<video controls>
    <source id="mp4" src="../figure6.mp4" type="video/mp4">
</video>
<figcaption>Hình 6: Box Layout của đoạn văn phải di chuyển vì có dấu xuống dòng mới</figcaption>
</figure>

CSS có thể làm cho phần tử nổi sang một bên, che phần mục tràn và thay đổi hướng viết. Bạn có thể tưởng tượng, giai đoạn layout này có một nhiệm vụ to lớn. Trong Chrome, toàn bộ team kỹ sư làm việc trên layout. Nếu bạn muốn xem chi tiết về công việc của họ, bạn có thể xem [một vài buổi toạ đàm từ Hội nghị BlinkOn](https://www.youtube.com/watch?v=Y5Xa4H2wtVA).

## Vẽ

Có DOM, style, và layout vẫn chưa đủ để render trang. Giả sử bạn muốn vẽ lại bức tranh đó. Bạn biết kích thước, hình dáng và vị trí các element, nhưng theo thứ tự nào?

<figure markdown>
![Hình 7: Một người đứng trước một bức vẽ với cọ sơn, tự hỏi xem mình nên vẽ hình tròn hay hình vuông trước](figure7.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 7: Một người đứng trước một bức vẽ với cọ sơn, tự hỏi xem mình nên vẽ hình tròn hay hình vuông trước</figcaption>
</figure>

Ví dụ, `z-index` có thể được set cho một số element nhất định, trong trường hợp đó, vẽ theo thứ tự trên HTML sẽ cho ra kết quả sai khi render.

<figure markdown>
![Hình 8: Element của trang xuất hiện theo thứ tự HTML markup, cho ra kết quả sai vì z-index không được sử dụng](figure8.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 8: Element của trang xuất hiện theo thứ tự HTML markup, cho ra kết quả sai vì z-index không được sử dụng</figcaption>
</figure>

Ở bước vẽ này, main thread sẽ đi qua cây layout để tạo ra các bản ghi vẽ. Bản ghi vẽ (paint record) là một ghi chú của quá trình vẽ như "nền trước, rồi chữ, rồi đến hình chữ nhật". Nếu bạn từng vẽ trên element `<canvas>` với JavaScript, bạn sẽ quen với quá trình này.

<figure markdown>
![Hình 9: Main thread đi qua cây layout và cho ra các bản ghi vẽ](figure9.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 9: Main thread đi qua cây layout và cho ra các bản ghi vẽ</figcaption>
</figure>

### Cập nhật rendering pipeline rất tốn kém.

Điều quan trọng nhất cần nắm bắt trong renderer pipeline là ở mỗi bước, kết quả của thao tác trước đó được sử dụng để tạo dữ liệu mới. Ví dụ, nếu có gì đó thay đổi trong cây layout, thì thứ tự vẽ cần được tạo lại cho các phần bị ảnh hưởng của tài liệu. Video bên dưới cho thấy các cây DOM + Style, Layout và Paint theo thứ tự chúng được sinh ra.

<figure markdown>
<video controls>
    <source id="mp4" src="../figure10.mp4" type="video/mp4">
</video>
<figcaption>Hình 10: Các cây DOM + Style, Layout và Paint theo thứ tự chúng được sinh ra</figcaption>
</figure>

Nếu bạn đang tạo hiệu ứng cho các element, trình duyệt phải chạy các thao tác này ở giữa mọi frame. Hầu hết các display sẽ refresh 60 lần một giây (60 frame/giây); hình ảnh động sẽ xuất hiện mượt mà đối với mắt người khi bạn di chuyển mọi thứ trên màn hình ở mọi frame. Tuy nhiên, nếu hoạt ảnh mất đi các frame ở giữa, thì trang sẽ xuất hiện một cách "lộn xộn".

<figure markdown>
![Hình 11: Các frame hoạt ảnh trên dòng thời gian](figure11.avif){ style="display: block; margin: 0 auto; height: 150px" }
<figcaption>Hình 11: Các frame hoạt ảnh trên dòng thời gian</figcaption>
</figure>

Ngay cả khi các hoạt động render của bạn theo kịp quá trình refresh màn hình, thì các tính toán này vẫn đang chạy trên main thread, nghĩa là thread này có thể bị chặn khi ứng dụng của bạn đang chạy JavaScript.

<figure markdown>
![Hình 12: Các frame hoạt ảnh trên dòng thời gian, nhưng một frame bị chặn bởi JavaScript](figure12.avif){ style="display: block; margin: 0 auto; height: 150px" }
<figcaption>Hình 12: Các frame hoạt ảnh trên dòng thời gian, nhưng một frame bị chặn bởi JavaScript</figcaption>
</figure>

Bạn có thể chia thao tác JavaScript thành các phần nhỏ và lên lịch chạy ở mọi frame bằng cách sử dụng `requestAnimationFrame()`. Để biết thêm về chủ đề này, vui lòng xem [Tối ưu hóa thực thi JavaScript](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution). Bạn cũng có thể chạy [JavaScript trong Web Worker](https://www.youtube.com/watch?v=X57mh8tKkgE) để tránh chặn main thread.

<figure markdown>
![Hình 13: Các đoạn JavaScript nhỏ hơn chạy trên dòng thời gian có frame hoạt ảnh](figure13.avif){ style="display: block; margin: 0 auto; height: 150px" }
<figcaption>Hình 13: Các đoạn JavaScript nhỏ hơn chạy trên dòng thời gian có frame hoạt ảnh</figcaption>
</figure>

## Compositor

### Bạn vẽ một trang như thế nào?

Bây giờ trình duyệt đã biết cấu trúc của tài liệu, style của từng element, hình dạng của trang và thứ tự tô màu, vậy nó sẽ vẽ một trang như thế nào? Biến thông tin này thành pixel trên màn hình được gọi là **rasterizing**.

Có lẽ một cách ngây thơ để xử lý việc này là raster các phần bên trong viewport (tầm nhìn của màn hình). Nếu người dùng cuộn trang, sau đó di chuyển frame đã raster và điền vào các phần còn thiếu bằng cách raster nhiều hơn, giống video bên dưới. Đây là cách Chrome xử lý rasterizing khi nó được phát hành lần đầu tiên. Tuy nhiên, trình duyệt hiện đại chạy một quy trình phức tạp hơn được gọi là compositing (tổng hợp).

<figure markdown>
<video controls>
    <source id="mp4" src="../figure14.mp4" type="video/mp4">
</video>
<figcaption>Hình 14: Hoạt ảnh của rasterizing process</figcaption>
</figure>

### Compositing là gì?

Compositing là một kỹ thuật để tách các phần của trang thành các layer, rasterize từng layer và tổng hợp dưới dạng một trang trong một thread riêng biệt được gọi là compositor thread. Nếu cuộn trang xảy ra, vì các lớp đã được rasterized, tất cả những gì nó phải làm là composite một frame mới. Hoạt ảnh có thể đạt được theo cách tương tự bằng cách di chuyển các layer và composite một frame mới, như video bên dưới.

<figure markdown>
<video controls>
    <source id="mp4" src="../figure15.mp4" type="video/mp4">
</video>
<figcaption>Hình 15: Hoạt ảnh của compositing process</figcaption>
</figure>

Bạn có thể xem cách trang web của mình được chia thành các layer trong DevTools bằng cách sử dụng [bảng Layers](https://blog.logrocket.com/eliminate-content-repaints-with-the-new-layers-panel-in-chrome-e2c306d4d752?gi=cd6271834cea).

### Chia ra thành các layer

Để tìm ra mỗi element nằm trong layer nào, main thread đi qua cây layout để tạo cây layer (phần này được gọi là "Cập nhật cây layer" trong bảng hiệu suất DevTools). Nếu một số phần của trang lẽ ra là layer riêng biệt (như menu bên trượt vào) mà không có layer riêng, thì bạn có thể gợi ý cho trình duyệt bằng cách sử dụng thuộc tính `will-change` trong CSS.

<figure markdown>
![Hình 16: Main thread đi qua cây layout để tạo ra cây layer](figure16.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 16: Main thread đi qua cây layout để tạo ra cây layer</figcaption>
</figure>

Bạn có thể muốn cung cấp các layer cho mọi element, nhưng việc composite trên một số lượng lớn các layer có thể dẫn đến hoạt động chậm hơn so với việc rasterizing các phần nhỏ của trang trên mỗi frame, vì vậy, điều quan trọng là bạn phải đo hiệu suất render của ứng dụng của mình. Để biết thêm về chủ đề này, hãy xem [Bám sát các thuộc tính chỉ dành cho Compositor và Quản lý số lượng Layer](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count).

### Raster và composite không liên quan đến main thread

Khi cây layer được tạo và thứ tự vẽ được xác định, main thread sẽ commit thông tin đó cho compositor thread. Compositor thread sau đó rasterize từng layer. Một layer có thể lớn bằng toàn bộ chiều dài của một trang, do đó, compositor thread chia chúng thành các ô và gửi từng ô tới các raster thread. Các raster thread sẽ rasterize từng ô và lưu chúng trong bộ nhớ GPU.

<figure markdown>
![Hình 17: Các raster thread tạo bitmap của các ô và gửi chúng đến GPU](figure17.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 17: Các raster thread tạo bitmap của các ô và gửi chúng đến GPU</figcaption>
</figure>

Compositor thread có thể ưu tiên các raster thread khác nhau để những thứ trong viewport(hoặc gần đó) có thể được raster trước. Một layer cũng có nhiều ô cho các độ phân giải khác nhau để xử lý những thứ như phóng to chẳng hạn.

Sau khi các ô được raster, compositor thread sẽ thu thập thông tin ô được gọi là **draw quads** để tạo **compositor frame**.

| | |
|---|---|
| Draw quad | Chứa thông tin như vị trí của ô trong bộ nhớ và vị trí trong trang để vẽ ô, có tính đến việc composite trang. |
| Compositor frame | Tập các draw quad đại diện cho một frame của trang. |

Một compositor frame sau đó được gửi tới browser process thông qua IPC. Ở đây, một compositor frame khác có thể được thêm vào từ UI thread để thay đổi UI của trình duyệt hoặc từ các renderer process khác dành cho tiện ích mở rộng. Các compositor frame này được gửi đến GPU để hiển thị trên màn hình. Nếu một sự kiện cuộn xảy ra, compositor thread sẽ tạo một compositor frame khác để gửi tới GPU.

<figure markdown>
![Hình 18: Compositor thread tạo compositing frame. Frame được gửi đến browser process rồi đến GPU](figure18.avif){ style="display: block; margin: 0 auto; height: 300px" }
<figcaption>Hình 18: Compositor thread tạo compositing frame. Frame được gửi đến browser process rồi đến GPU</figcaption>
</figure>

Lợi ích của việc composite là nó được thực hiện mà không liên quan đến main thread. Compositor thread không cần đợi tính toán style hoặc thực thi JavaScript. Đây là lý do tại sao các [hoạt-ảnh-chỉ-cần-composite](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/) được coi là tốt nhất để có hiệu suất mượt mà. Nếu layout hoặc paint cần được tính toán lại thì main thread phải tham gia.

## Kết phần 3

Trong phần này, ta đã xem qua rendering pipeline từ parsing đến compositing. Hi vọng rằng bạn đã đủ khả năng để đọc thêm về tối ưu hoá hiệu suất của một trang web.

Trong phần tiếp theo và cũng là phần cuối, ta sẽ nghiên cứu compositor thread chi tiết hơn và xem điều gì xảy ra khi người dùng nhập liệu cũng như di chuyển và nhấp chuột.
