# Bên trong các trình duyệt ngày nay (Phần 1)

## Nguồn

<img src="../../../assets/images/chrome.png" width="16" height="16"/> [Inside look at modern web browser (part 1) - Chrome Developers Blog](https://developer.chrome.com/blog/inside-browser-part1/)

## CPU, GPU, bộ nhớ, và kiến trúc đa tiến trình

Trong series 4 phần này, ta sẽ đi sâu vào trình duyệt Chrome từ kiến trúc bậc cao đến các chi tiết của quá trình render (kết xuất). Nếu bạn từng thắc mắc làm sao trình duyệt biến code của bạn thành một trang web, hoặc tại sao một kỹ thuật cụ thể nào đó có thể được dùng để cải thiện hiệu suất, thì series này là dành cho bạn.

Trong phần 1 của series này, ta sẽ xem xét đến thuật ngữ core computing (điện toán lõi) và kiến trúc đa tiến trình của Chrome.

## Nhân máy tính chứa CPU và GPU

Để hiểu môi trường mà trình duyệt sẽ chạy, ta cần hiểu một số thành phần trong máy tính trước đã.

### CPU

Đầu tiên là **CPU** (**C**entral **P**rocessing **U**nit - đơn vị xử lý trung tâm). CPU có thể được xem như não bộ của máy tính. Một nhân CPU, như trong hình là một kiểu nhân viên văn phòng, có thể xử lý nhiều tác vụ lần lượt. Nó có thể xử lý mọi thứ từ toán cho đến mỹ thuật. Trong quá khứ, hầu hết các CPU là một chip đơn. Một core giống như một CPU khác ở trong cùng một con chịp. Trong phần cứng hiện đại, ta thường có nhiều hơn một core, mang lại nhiều sức mạnh tính toán hơn cho điện thoại và laptop của bạn.

<figure markdown>
![Hình 1: 4 core làm việc như nhân viên văn phòng xử lý các lần lượt các tác vụ nhận được](../../assets/Misc/chrome_blog/inside_browser_1/figure1.avif){:class="centered-img h-200"}
<figcaption>Hình 1: 4 core làm việc như nhân viên văn phòng xử lý các lần lượt các tác vụ nhận được</figcaption>
</figure>

### GPU

**GPU** (**G**raphics **P**rocessing **U**nit - đơn vị xử lý đồ hoạ) là một phần khác của máy tính. Khác với CPU, GPU xử lý các tác vụ đơn giản nhưng trên nhiều core một lần. Giống như tên gọi thì ban đầu nó được phát triển để xử lý đồ hoạ. Đây là lý do vì sao mà trong các trường hợp "dùng GPU" hay "được hỗ trợ bởi GPU" thường đi liền với render nhanh và tương tác mượt mà. Trong những năm gần đây, với điện toán được GPU tăng tốc, ngày càng có thể thực hiện được nhiều tính toán hơn chỉ trên GPU.

<figure markdown>
![Hình 2: Nhiều core cầm cờ lê cho thấy chúng đang xử lý một tác vụ đơn giản](../../assets/Misc/chrome_blog/inside_browser_1/figure2.avif){:class="centered-img h-200"}
<figcaption>Hình 2: Nhiều core cầm cờ lê cho thấy chúng đang xử lý một tác vụ đơn giản</figcaption>
</figure>

Khi bạn khởi động một ứng dụng trên máy tính hoặc điện thoại, CPU và GPU là những thứ chính yếu của ứng dụng. Thông thường, các ứng dụng chạy trên CPU và GPU sử dụng các cơ chế do hệ điều hành cung cấp.

<figure markdown>
![Hình 3: Ba lớp kiến trúc máy tính. Phần cứng máy tính nằm dưới cùng, Hệ điều hành nằm giữa, còn Ứng dụng nằm trên cùng](../../assets/Misc/chrome_blog/inside_browser_1/figure3.avif){:class="centered-img h-200"}
<figcaption>Hình 3: Ba lớp kiến trúc máy tính. Phần cứng máy tính nằm dưới cùng, Hệ điều hành nằm giữa, còn Ứng dụng nằm trên cùng</figcaption>
</figure>

## Khởi chạy chương trình trên Process và Thread

Một khái niệm nữa cần nắm trước khi đi sâu vào kiến trúc trình duyệt là Process và Thread. Một process có thể được xem như một chương trình thực thi của ứng dụng. Một thread là thứ nằm bên trong process và thực thi bất kỳ phần nào trong chương trình của process của nó.

<figure markdown>
![Hình 4: Process là hình chữ nhật nằm ngoài, còn Thread thì là mấy con cá bên trong Process](../../assets/Misc/chrome_blog/inside_browser_1/figure4.avif){:class="centered-img h-200"}
<figcaption>Hình 4: Process là hình chữ nhật nằm ngoài, còn Thread thì là mấy con cá bên trong Process</figcaption>
</figure>

Khi bạn khởi động một ứng dụng, một process sẽ được tạo ra. Chương trình có thể tạo các thread để giúp chương trình hoạt động, nhưng không bắt buộc. Hệ điều hành cung cấp cho process một khối bộ nhớ để hoạt động và tất cả các trạng thái của ứng dụng được giữ trong không gian bộ nhớ riêng đó. Khi đóng ứng dụng, process này cũng biến mất và hệ điều hành sẽ giải phóng bộ nhớ.

<figure markdown>
![Hình 5: Sơ đồ process sử dụng không gian bộ nhớ và lưu trữ dữ liệu của ứng dụng](../../assets/Misc/chrome_blog/inside_browser_1/figure5.svg){:class="centered-img h-200"}
<figcaption>Hình 5: Sơ đồ process sử dụng không gian bộ nhớ và lưu trữ dữ liệu của ứng dụng</figcaption>
</figure>

Một process có thể yêu cầu hệ điều hành khởi động một process khác để chạy các tác vụ khác nhau. Khi điều này xảy ra, các phần khác nhau của bộ nhớ được phân bố cho process mới. Nếu hai process cần giao tiếp với nhau, chúng sẽ dùng **IPC** (**I**nter **P**rocess **C**ommunication - Giao tiếp giữa các process). Nhiều ứng dụng được thiết kế để hoạt động theo cách này sao cho nếu một worker process bị treo, nó có thể được khởi động lại mà không phải dừng các process khác đang chạy các phần khác nhau của ứng dụng.

<figure markdown>
![Hình 6: Sơ đồ các process riêng biệt giao tiếp với nhau thông qua IPC](../../assets/Misc/chrome_blog/inside_browser_1/figure6.svg){:class="centered-img h-200"}
<figcaption>Hình 6: Sơ đồ các process riêng biệt giao tiếp với nhau thông qua IPC</figcaption>
</figure>

## Kiến trúc trình duyệt

Giờ thì trình duyệt được xây dựng dựa trên process và thread như thế nào? Có thể là một process với nhiều thread khác nhau hoặc nhiều process khác nhau với một ít thread giao tiếp với nhau qua IPC.

<figure markdown>
![Hình 7: Các kiến trúc trình duyệt khác nhau trong sơ đồ process/thread](../../assets/Misc/chrome_blog/inside_browser_1/figure7.avif){:class="centered-img h-200"}
<figcaption>Hình 7: Các kiến trúc trình duyệt khác nhau trong sơ đồ process/thread</figcaption>
</figure>

Một lưu ý quan trọng là các kiến trúc khác nhau này là các chi tiết cài đặt. Không có một chuẩn nào cho việc tạo ra một trình duyệt. Cách tiếp cận để xây dựng trình duyệt này có thể khác hoàn toàn với cách tiếp cận để xây dựng một trình duyệt khác.

Ta sẽ chỉ tập trung vào kiến trúc bây giờ của Chrome được miêu tả trong sơ đồ bên dưới.

Ở trên cùng là process trình duyệt phối hợp với các process khác để đảm nhiệm các phần khác nhau của ứng dụng. Với renderer process, nhiều process được tạo ra và gán cho mỗi tab. Cho đến gần đây, Chrome cho mỗi tab một process khi có thể; giờ nó cố gắng cho mỗi trang một process riêng, kể cả iframe (Ở phần Site Isolation).

<figure markdown>
![Hình 8: Sơ đồ kiến trúc đa tiến trình của Chrome. Renderer Process có nhiều lớp để biểu diễn việc Chrome chạy nhiều process ở đây cho mỗi tab](../../assets/Misc/chrome_blog/inside_browser_1/figure8.avif){:class="centered-img h-300"}
<figcaption>Hình 8: Sơ đồ kiến trúc đa tiến trình của Chrome. Renderer Process có nhiều lớp để biểu diễn việc Chrome chạy nhiều process ở đây cho mỗi tab</figcaption>
</figure>

## Các Process điều khiển cái gì?

Bảng sau cho thấy các process của Chrome và những thứ chúng điều khiển:

| Process | Điều khiển |
| :---: | :----------- |
| Trình duyệt | Điều khiển phần "chrome" của ứng dụng bao gồm thanh địa chỉ, bookmark, nút quay lại trang trước và đi tiếp trang sau. <br/> Xử lý các phần ẩn, đặc quyền của trình duyệt như network request hay truy cập file. |
| Renderer | Điều khiển bất cứ cái gì trong tab, nơi mà website được hiển thị. |
| Plugin | Điều khiển bất kỳ plugin nào dùng bởi website, ví dụ như flash. |
| GPU | Xử lý các tác vụ GPU riêng biệt, tách ra khỏi các process khác. Nó được phân ra riêng vì các GPU xử lý các yêu cầu từ nhiều ứng dụng và vẽ chúng trên cùng một chỗ |

<figure markdown>
![Hình 9: Các process khác nhau trỏ vào các phần khác nhau trên UI của trình duyệt](../../assets/Misc/chrome_blog/inside_browser_1/figure9.avif){:class="centered-img h-400"}
<figcaption>Hình 9: Các process khác nhau trỏ vào các phần khác nhau trên UI của trình duyệt</figcaption>
</figure>

Còn có nhiều process hơn nữa như là process cho các tiện ích mở rộng hay process cho các tiện ích khác. Nếu bạn muốn xem có bao nhiêu process đang chạy trong Chrome, bấm vào dấu ba chấm ở góc phải trên của Chrome, chọn More Tools, rồi chọn Task Manager. Một cửa sổ sẽ hiện lên với danh sách các process đang chạy và chúng đang dùng bao nhiêu CPU / Bộ nhớ.

## Lợi ích của kiến trúc đa tiến trình trong Chrome

Trước đó, ta đã đề cập đến việc Chrome sử dụng nhiều renderer process. Trong trường hợp đơn giản nhất, ta có thể tưởng tượng mỗi tab có một renderer process riêng. Giả sử bạn có 3 tab đang mở và mỗi tab được chạy bởi một renderer process độc lập. Nếu một tab bị treo, thì bạn có thể đóng tab đó trong khi vẫn giữ cho các tab khác hoạt động. Nếu tất cả các tab đang chạy trên một process, khi một tab treo, các tab khác cũng treo luôn. Khá buồn.

<figure markdown>
![Hình 10: Sơ đồ cho thấy nhiều process chạy trên mỗi tab](../../assets/Misc/chrome_blog/inside_browser_1/figure10.avif){:class="centered-img h-400"}
<figcaption>Hình 10: Sơ đồ cho thấy nhiều process chạy trên mỗi tab</figcaption>
</figure>

Một lợi ích khác của việc tách công việc của trình duyệt thành nhiều process là do tính bảo mật và độc lập. Vì các hệ điều hành cung cấp một cách để hạn chế các quyền của các process, trình duyệt có thể sandbox một số process nhất định từ các tính năng nhất định. Ví dụ, trình duyệt Chrome hạn chế quyền truy cập file tuỳ ý đối với các process xử lý đầu vào tuỳ ý của người dùng như renderer process.

Vì các process có không gian bộ nhớ riêng nên chúng thường chứa bản sao của cơ sở hạ tầng chung (như V8 là công cụ JavaScript của Chrome). Điều này nghĩa là sử dụng nhiều bộ nhớ hơn vì chúng không thể được chia sẻ theo cách truyền thống nếu chúng là các thread trong cùng một process. Để tiết kiệm bộ nhớ, Chrome đặt giới hạn về số process có thể thực hiện. Giới hạn khác nhau tuỳ vào dung lượng bộ nhớ và sức mạnh CPU mà thiết bị có, nhưng khi Chrome đạt giới hạn, nó sẽ bắt đầu chạy nhiều tab của cùng một trang trên cùng một process.

## Tiết kiệm nhiều bộ nhớ hơn - Servicification trong Chrome

Cách tiếp cận tương tự cũng được áp dụng cho process trình duyệt. Chrome đang tiến hành các thay đổi về kiến trúc để chạy từng phần của trình duyệt dưới dạng một service cho phép phân chia dễ dàng các process khác nhau hoặc tổng hợp thành một.

Ý tưởng chung là khi Chrome đang chạy trên phần cứng mạnh mẽ, nó có thể chia từng service thành các process khác nhau để giữ tính ổn định. Nhưng nếu chạy trên một thiết bị hạn chế tài nguyên, Chrome sẽ hợp nhất các service thành một process để tiết kiệm bộ nhớ. Cách tiếp cận tương tự về hợp nhất các process để sử dụng ít bộ nhớ hơn đã được dùng trên các nền tảng khác như Android trước khi có sự thay đổi này.

<figure markdown>
![Hình 11: Sơ đồ Servicification trong Chrome dịch chuyển các service khác nhau vào nhiều process và một process trình duyệt đơn](../../assets/Misc/chrome_blog/inside_browser_1/figure11.svg){:class="centered-img h-400"}
<figcaption>Hình 11: Sơ đồ Servicification trong Chrome dịch chuyển các service khác nhau vào nhiều process và một process trình duyệt đơn</figcaption>
</figure>

## Renderer process trên mỗi frame - Site Isolation

[Site Isolation](https://developers.google.com//web/updates/2018/07/site-isolation) (cách ly trang web) là một tính năng được giới thiệu gần đây trong Chrome, nó chạy các renderer process khác nhau cho từng iframe của các trang web khác (cross-site iframe) trên cùng một tab. Ta đã nói về một renderer process trên mỗi tab cho phép các iframe của các trang web khác chạy một renderer process với không gian bộ nhớ chia sẻ trên các site khác nhau. Chạy `a.com` và `b.com` trên cùng một renderer process nghe có vẻ ổn. [Same Origin Policy](https://developer.mozilla.org/docs/Web/Security/Same-origin_policy) (chính sách xuất xứ giống nhau) là mô hình bảo mật cốt lõi của web; nó bảo đảm một trang web không thể truy cập dữ liệu từ các web khác mà không có sự đồng ý. Bỏ qua chính sách này là một lỗ hổng lớn cho các cuộc tấn công mạng. Việc cách ly các process là cách hiệu quả nhất để tách các trang web. Với [Meltdown và Spectre](https://developers.google.com/web/updates/2018/02/meltdown-spectre), rõ ràng hơn là chúng ta cần tách các trang web bằng các process. Với Site Isolation được áp dụng mặc định trên Chrome desktop từ bản 67, mỗi iframe của các trang web khác trong một tab có một renderer process riêng.

<figure markdown>
![Hình 12: Sơ đồ Site Isolation; nhiều renderer process trỏ vào các iframe trong cùng một trang web](../../assets/Misc/chrome_blog/inside_browser_1/figure12.avif){:class="centered-img h-400"}
<figcaption>Hình 12: Sơ đồ Site Isolation; nhiều renderer process trỏ vào các iframe trong cùng một trang web</figcaption>
</figure>

Cho phép Site Isolation là một nỗ lực trong nhiều năm của đội ngũ kỹ sư. Site Isolation không đơn giản chỉ là chỉ định các renderer process khác nhau; về cơ bản nó thay đổi cách các iframe giao tiếp với nhau. Mở devtools trên một trang có iframe chạy trên các process khác nhau nghĩa là devtools phải triển khai công việc hậu trường để làm nó xuất hiện liền mạch. Ngay cả việc chạy `Ctrl+F` đơn giản để tìm một từ trong trang cũng có nghĩa là tìm kiếm trên nhiều renderer process khác nhau. Bạn có thể thấy vì sao các kỹ sư trình duyệt nói về việc phát hành Site Isolation như là một cột mốc đỉnh cao!

## Kết phần 1

Trong phần này, ta đã đi qua góc nhìn cơ bản của kiến trúc trình duyệt và lợi ích của kiến trúc đa tiến trình. Ta cũng đi qua Servicification và Site Isolation trong Chrome, những thứ cực kỳ liên quan đến kiến trúc đa tiến trình. Trong phần sau, ta sẽ khám phá chuyện gì xảy ra giữa các process và thread này để hiển thị website.
