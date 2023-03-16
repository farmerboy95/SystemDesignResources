# Bên trong các trình duyệt ngày nay (Phần 2)

## Nguồn

[Inside look at modern web browser (part 2) - Chrome Developers Blog](https://developer.chrome.com/blog/inside-browser-part2/)

## Chuyện xảy ra khi điều hướng

Đây là phần 2 trong series 4 phần tìm hiểu về Chrome. Trong phần 1, ta tìm hiểu làm sao các process và thread khác nhau xử lý các phần khác nhau trong trình duyệt. Ở phần này, ta đi sâu hơn về việc làm thế nào mỗi process và thread giao tiếp với nhau để hiển thị một trang web.

Ta bắt đầu với một use case đơn giản như sau: Bạn điền URL vào trình duyệt, trình duyệt lấy data từ internet và hiển thị trang web. Trong phần này, ta sẽ tập trung vào phần mà người dùng request một website và trình duyệt chuẩn bị render một trang - nôm na gọi là điều hướng (navigation).

**Lưu ý: Vui lòng bấm vào hình ảnh để xem chú thích của hình ảnh đó.**

## Nó bắt đầu với browser process

Như ta đã biết từ phần 1, mọi thứ không trong tab sẽ được xử lý bởi browser process. Browser process này có các thread như UI thread, nó sẽ vẽ các nút bấm và ô nhập liệu (như thanh địa chỉ) của trình duyệt. browser process còn có network thread, nó sẽ xử lý các network stack để nhận data từ internet. Storage thread cũng là một thành phần của browser process, nó kiểm soát quyền truy cập file. Và còn một số loại thread nữa. Khi bạn nhập URL vào thanh địa chỉ, nó sẽ được xử lý bởi UI thread của browser process.

![!Hình 1: UI của trình duyệt ở trên, sơ đồ của browser process với UI, network và storage thread bên trong ở dưới](figure1.avif){ style="display: block; margin: 0 auto; height: 300px" }

## Điều hướng đơn giản

### Bước 1: Xử lý input

Khi người dùng bắt đầu nhập vào thanh địa chỉ, đầu tiên UI thread sẽ hỏi là "Đây là truy vấn tìm kiếm hay URL vây?". Trong Chrome, thanh địa chỉ cũng là thanh tìm kiếm, nên UI thread cần phải kiểm tra và quyết định xem nên gửi người dùng đến bộ máy tìm kiếm hay đi đến cái trang mà người ta muốn đến.

![!Hình 2: UI thread hỏi xem input là truy vấn tìm kiếm hay URL](figure2.avif){ style="display: block; margin: 0 auto; height: 300px" }

### Bước 2: Bắt đầu điều hướng

Khi người dùng bấm Enter, UI thread khởi tạo network call để lấy nội dung trang web. Biểu tượng xoay (loading) sẽ xuất hiện ở góc của tab, và network thread đi qua các giao thức thích hợp như tra cứu DNS và thiết lập kết nối TLS cho request.

![!Hình 3: UI thread giao tiếp với network thread để điều hướng đến mysite.com](figure3.avif){ style="display: block; margin: 0 auto; height: 300px" }

Tại thời điểm này, network thread có thể được nhận header chuyển hướng server như HTTP 301. Trong trường hợp đó, network thread giao tiếp với UI thread để nói cho UI thread biết là server đang yêu cầu chuyển hướng. Sau đó, một URL request khác sẽ được khởi tạo.

### Bước 3: Đọc response

Khi mà response body bắt đầu đến, network thread đọc một vài byte đầu của stream nếu cần. Content-Type header của response cho nó biết loại data là gì, nhưng vì thông tìn này có thể bị thiếu hoặc sai, [kiểm tra MIME Type (MIME Type sniffing)](https://developer.mozilla.org/docs/Web/HTTP/Basics_of_HTTP/MIME_types) sẽ được thực hiện tại đây. Đây là một "công việc khó khăn" như được comment trong [code](https://cs.chromium.org/chromium/src/net/base/mime_sniffer.cc?sq=package:chromium&dr=CS&l=5). Bạn có thể đọc comment để xem các trình duyệt khác nhau xử lý cặp Content-Type - response body như thế nào.

![!Hình 4: Response header chứa Content-Type và response body chứa data thực sự](figure4.avif){ style="display: block; margin: 0 auto; height: 200px" }

Nếu response là một file HTML, thì bước tiếp theo sẽ là chuyển data cho renderer process, nhưng nếu nó là file zip hoặc các loại file khác thì nghĩa là nó là một request tại file, nên data được chuyển qua cho bên download manager.

![!Hình 5: Network thread hỏi xem response data có phải HTML từ một trang an toàn không](figure5.avif){ style="display: block; margin: 0 auto; height: 300px" }

Đây cũng là nơi kiểm tra [SafeBrowsing](https://safebrowsing.google.com/) xảy ra. Nếu domain và response data khớp một cái site lừa đảo nào đó, network thread sẽ nhắc trình duyệt để hiển thị một trang cảnh báo. Thêm vào đó, kiểm tra [**C**ross **O**rigin **R**ead **B**locking (**CORB**)](https://www.chromium.org/Home/chromium-security/corb-for-developers) cũng được thực hiện để đảm bảo thông tin nhạy cảm xuyên website không đi vào renderer process.

### Bước 4: Tìm một renderer process

Sau khi các kiểm tra đều đã xong và network thread chắc chắn rằng trình duyệt sẽ được điều hướng đến trang web được yêu cầu, network thread sẽ bảo UI thread rằng data đã sắn sàng. UI thread sau đó tìm một renderer process để thực hiện quá trình render trang web

![!Hình 6: Network thread kêu UI thread tìm Renderer Process](figure6.avif){ style="display: block; margin: 0 auto; height: 300px" }

Vì network request có thể mất vài trăm mili giây để nhận phản hồi, nên một quá trình tối ưu hoá được áp dụng để tăng tốc quá trình trên. Khi UI thread đang gửi URL request cho network thread ở bước 2, nó đã biết trình duyệt sẽ được điều hướng đến trang nào. UI thread cố gắng chủ động tìm hoặc khởi chạy một renderer process song song với network request. Bằng cách này, nếu mọi việc diễn ra bình thường, một renderer process đã chờ sẵn khi network thread nhận đata. Process dự phòng này có thể không được sử dụng nếu việc điều hướng chuyển hướng giữa các trang, trong trường hợp đó thì phải cần một process khác.

### Bước 5: Điều hướng

Giờ data và renderer process đã sẵn sàng, một IPC được gửi từ browser process đến renderer process để thực hiện điều hướng. Nó cũng truyền luồng data để renderer process có thể tiếp tục nhận HTML data. Một khi browser process được xác nhận rằng quá trình điều hướng đã xảy ra trong renderer process, điều hướng hoàn tất và giai đoạn tải tài liệu bắt đầu.

Tại thời điểm này, thanh địa chỉ được cập nhật và giao diện người dùng cài đặt trang và chỉ báo bảo mật phản ánh thông tin trang của trang mới. Lịch sử phiên cho tab sẽ được cập nhật để các nút quay lại / chuyển tiếp sẽ chuyển qua trang web vừa được điều hướng đến. Để tạo điều kiện khôi phục tab / phiên khi bạn đóng tab hoặc cửa sổ trình duyệt, lịch sử phiên sẽ được lưu trên đĩa.

![!Hình 7: IPC giữa trình duyệt và các renderer process, để yêu cầu render trang](figure7.avif){ style="display: block; margin: 0 auto; height: 300px" }

### Bước bổ sung: Hoàn thành việc tải ban đầu

Khi điều hướng được cam kết, renderer process tiếp tục tải tài nguyên và render trang. Ta sẽ xem các chi tiết về những gì xảy ra ở đây trong phần tiếp theo. Sau khi renderer process "hoàn tất" việc render, nó gửi một IPC trở lại browser process (đây là sau khi tất cả các event `onload` đã kích hoạt trên tất cả các frame trên trang và đã thực hiện xong). Tại thời điểm này, UI thread dừng biểu tượng xoay trên tab.

Ta nói "hoàn tất" vì JavaScript phía client vẫn có thể tải các tài nguyên bổ sung và render các view mới sau bước này.

![!Hình 8: IPC từ renderer process sang browser process để báo rằng trang đã được tải](figure8.avif){ style="display: block; margin: 0 auto; height: 300px" }

## Điều hướng đến một trang web khác

Điều hướng đơn giản đã hoàn tất! Nhưng điều gì xảy ra nếu người dùng nhập URL khác vào thanh địa chỉ? Browser process sẽ trải qua các bước tương tự để điều hướng đến trang web khác. Nhưng trước khi có thể làm điều đó, nó cần kiểm tra với trang web hiện được hiển thị xem nó có quan tâm đến event []`beforeunload`](https://developer.mozilla.org/docs/Web/Events/beforeunload) hay không.

`beforeunload` có thể tạo cảnh báo "Bạn muốn rời khỏi trang web này?" khi ta cố gắng điều hướng đi hoặc đóng tab. Mọi thứ bên trong tab bao gồm cả code JavaScript của bạn đều được xử lý bởi renderer process, vì vậy browser process phải kiểm tra với renderer process hiện tại khi có yêu cầu điều hướng mới.

??? tip "Chú ý"
    Không thêm `beforeunload` handler vô điều kiện. Nó tạo ra độ trễ lớn hơn vì handler cần được thực thi trước khi có thể bắt đầu điều hướng. Chỉ nên thêm event handler này khi cần thiết, chẳng hạn như nếu người dùng cần được cảnh báo rằng họ có thể mất dữ liệu đã nhập trên trang.

![!Hình 9: IPC từ browser process sang một renderer process để nói cho nó biết rằng trình duyệt chuẩn bị điều hướng tới một trang web khác](figure9.avif){ style="display: block; margin: 0 auto; height: 300px" }

Nếu điều hướng được khởi tạo từ renderer process (chẳng hạn như khi người dùng đã nhấp vào liên kết hoặc client-side JavaScript đã chạy `window.location = "https://newsite.com"`) thì renderer process trước tiên sẽ kiểm tra `beforeunload` handler. Sau đó, nó trải qua quy trình tương tự như quá trình điều hướng do browser process bắt đầu. Sự khác biệt duy nhất là yêu cầu điều hướng được khởi tạo từ renderer process sang browser process.

Khi điều hướng mới được tạo cho một trang web khác với trang web được render hiện tại, một renderer process riêng biệt sẽ được gọi để xử lý điều hướng mới trong khi renderer process hiện tại được giữ lại để xử lý các event như `unload`. Để biết thêm thông tin, vui lòng xem [tổng quan về trạng thái vòng đời của trang](https://developers.google.com/web/updates/2018/07/page-lifecycle-api#overview_of_page_lifecycle_states_and_events) và cách bạn có thể kết nối với các event bằng [Page Lifecycle API](https://developers.google.com/web/updates/2018/07/page-lifecycle-api).

![!Hình 10: Các IPC từ browser process sang một renderer process mới để render trang mới và sang renderer process cũ để unload trang cũ](figure10.avif){ style="display: block; margin: 0 auto; height: 300px" }

## Trường hợp của Service Worker

Một thay đổi gần đây đối với quy trình điều hướng này là việc giới thiệu [Service Worker](https://developers.google.com/web/fundamentals/primers/service-workers/). Service worker là một cách để viết proxy mạng trong code; cho phép các nhà phát triển web có nhiều quyền kiểm soát hơn đối với những gì cần lưu vào bộ nhớ cache cục bộ và thời điểm nhận dữ liệu mới từ mạng. Nếu Service worker được thiết lập để tải trang từ cache, thì không cần yêu cầu data từ mạng.

Điều quan trọng cần nhớ là Service worker là code JavaScript chạy trong renderer process. Nhưng khi có yêu cầu điều hướng, làm thế nào để trình duyệt biết trang web có Service worker?

![!Hình 11: Network thread trong browser process tra cứu phạm vi của Service worker](figure11.avif){ style="display: block; margin: 0 auto; height: 300px" }

Khi một Service worker được đăng ký, phạm vi của Service worker được giữ làm tham chiếu (bạn có thể đọc thêm về phạm vi trong bài viết [Vòng đời của Service worker](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle) này). Khi một quá trình điều hướng diễn ra, network thread sẽ kiểm tra miền dựa trên phạm vi của Service worker đã đăng ký, nếu một Service worker được đăng ký cho URL đó, thì UI thread sẽ tìm một renderer process để thực thi code của Service worker. Service worker có thể tải data từ cache, loại bỏ nhu cầu yêu cầu data từ mạng hoặc có thể yêu cầu tài nguyên mới từ mạng.

![!Hình 12: UI thread trong browser process khởi tạo một renderer process để xử lý các service worker; một worker thread trong renderer process sau đó yêu cầu data từ mạng](figure12.avif){ style="display: block; margin: 0 auto; height: 300px" }

## Tải trước điều hướng

Bạn có thể thấy round trip này giữa browser process và renderer process có thể dẫn đến delay nếu Service worker quyết định yêu cầu data từ mạng. Tải trước điều hướng là một cơ chế để tăng tốc quá trình này bằng cách tải tài nguyên song song với khởi động Service worker. Nó đánh dấu các request này bằng một header, cho phép các máy chủ quyết định gửi nội dung khác nhau cho các request này; ví dụ: chỉ cập nhật data thay vì full data.

![!Hình 13: UI thread trong browser process khởi tạo một renderer process để xử lý các service worker trong khi bắt đầu network request cùng lúc đó](figure13.avif){ style="display: block; margin: 0 auto; height: 300px" }

## Kết phần 2

Trong phần này, ta đã xem thử chuyện gì sẽ xảy ra trong khi điều hướng và làm sao code của ứng dụng web như response header và JavaScript phía client giao tiếp với trình duyệt. Việc biết được các bước trình duyệt phải thực hiện để lấy data từ mạng giúp ta dễ hiểu hơn vì sao các API như Tải trước điều hướng được phát triển. Trong bài sau, ta sẽ đi vào việc làm sao trình duyệt đọc code HTML/CSS/JavaScript của chúng ta để hiển thị các trang.
