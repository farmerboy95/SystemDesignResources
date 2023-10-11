# Bên trong các trình duyệt ngày nay (Phần 4)

## Nguồn

<img src="../../../assets/images/chrome.png" width="16" height="16"/> [Inside look at modern web browser (part 4) - Chrome Developers Blog](https://developer.chrome.com/blog/inside-browser-part4/)

## Input đi đến Compositor

Đây là phần cuối trong series 4 phần về bên trong trình duyệt Chrome, nơi ta sẽ xem làm sao nó có thể xử lý code để hiện ra một trang web. Trong bài trước, ta đã tìm hiểu rendering process và compositor. Trong bài này, ta sẽ xem compositor tạo ra những tương tác mượt mà như thế nào khi có input từ người dùng.

## Các input event từ góc nhìn trình duyệt

Khi nghe "input event", bạn có thể nghĩ ngay đến việc nhập vào một textbox hay click chuột, nhưng từ góc nhìn của trình duyệt, input có nghĩa là bất kỳ hành động nào từ người dùng. Cuộn cũng là một input event, chạm hoặc di chuột qua cũng là một input event.

Khi hành động của người dùng như chạm vào màn hình xảy ra, browser process sẽ nhận hành động đó trước. Tuy nhiên, browser process chỉ biết nơi hành động đó xảy ra do nội dung bên trong tab được xử lý bởi renderer process. Vì vậy, browser process sẽ gửi loại event (như `touchstart`) và toạ độ của nó đến renderer process. Renderer process xử lý event một cách thích hợp bằng cách tìm event target và chạy event listener được đính kèm.

<figure markdown>
![Hình 1: Input event được dẫn qua browser process sang renderer process](../../assets/misc/chrome-blog/inside-browser-4/figure1.avif){:class="centered-img h-300"}
<figcaption>Hình 1: Input event được dẫn qua browser process sang renderer process</figcaption>
</figure>

## Compositor nhận các input event

Trong phần trước, ta đã xem cách compositor có thể xử lý cuộn một cách mượt mà bằng cách tổng hợp các rasterized layer. Nếu không có input event listener nào được đính kèm vào trang, compositor thread có thể tạo một composite frame mới hoàn toàn độc lập với main thread. Nhưng nếu có listener thì sao? Làm sao compositor thread tìm ra event cần được xử lý.

<figure markdown>
<video controls>
    <source id="mp4" src="../../../assets/misc/chrome-blog/inside-browser-4/figure2.mp4" type="video/mp4">
</video>
<figcaption>Hình 2: Khung nhìn (viewport) trên các layer</figcaption>
</figure>

## Hiểu về non-fast scrollable region

Vì chạy JavaScript là việc của main thread, nên khi có một trang được tổng hợp (composited), compositor thread sẽ đánh dấu một vùng của trang có đính kém các event handler là "vùng cuộn không nhanh" (Non-Fast Scrollable Region). Với thông tin này, compositor thread có thể đảm bảo gửi input event đến main thread nến event xảy ra trong vùng đó. Nếu input event đến từ ngoài vùng, compositor thread sẽ tiếp tục tổng hợp frame mới mà không cần đợi main thread.

<figure markdown>
![Hình 3: Sơ đồ mô tả input vào non-fast scrollable region](../../assets/misc/chrome-blog/inside-browser-4/figure3.avif){:class="centered-img h-300"}
<figcaption>Hình 3: Sơ đồ mô tả input vào non-fast scrollable region</figcaption>
</figure>

### Để ý khi viết event handler

Một pattern phổ biến khi xử lý event trong phát triển web là uỷ quyền event. Vì event chuyển thông tin từ node có event đi lên trên cây DOM (bubble), ta có thể đính kèm một event handler ở element trên cùng và uỷ quyền các tác vụ dựa trên event target. Bạn có thể đã thấy hoặc viết code như sau:

```javascript
document.body.addEventListener('touchstart', event => {
    if (event.target === area) {
        event.preventDefault();
    }
});
```

Vì ta chỉ cần viết một event handler cho tất cả element, pattern uỷ quyền event này rất hấp dẫn. Tuy nhiên, nếu bạn ở góc nhìn của trình duyệt, toàn bộ trang sẽ được xem như non-fast scrollable region. Nghĩa là ngay cả khi ứng dụng của bạn không quan tâm đến input từ một số phần nhất định của trang, compositor thread sẽ phải giao tiếp với main thread và đợi nó mỗi khi có input event đến. Do đó, khả năng cuộn mượt mà của compositor sẽ không còn.

<figure markdown>
![Hình 4: Sơ đồ cho thấy input vào non-fast scrollable region mà bao trọn toàn bộ trang](../../assets/misc/chrome-blog/inside-browser-4/figure4.avif){:class="centered-img h-300"}
<figcaption>Hình 4: Sơ đồ cho thấy input vào non-fast scrollable region mà bao trọn toàn bộ trang</figcaption>
</figure>

Để giảm thiểu điều này, bạn có thể truyền `passive : true` vào event listener. Nó sẽ gợi ý cho trình duyệt rằng bạn vẫn muốn tiếp tục lắng nghe event ở main thread, nhưng compositor vẫn có thể chạy tiếp và tạo frame mới.

```javascript
document.body.addEventListener('touchstart', event => {
    if (event.target === area) {
        event.preventDefault()
    }
 }, {passive: true});
```

## Kiểm tra xem có huỷ event được không

Giả sử bạn có một cái khung trong trang, bạn muốn cái khung này chỉ cuộn ngang.

<figure markdown>
![Hình 5: Trang web với khung chỉ cho cuộn ngang](../../assets/misc/chrome-blog/inside-browser-4/figure5.avif){:class="centered-img h-300"}
<figcaption>Hình 5: Trang web với khung chỉ cho cuộn ngang</figcaption>
</figure>

Dùng `passive: true` trong pointer event nghĩa là trang có thể cuộn một cách mượt mà, nhưng cuộn dọc có thể đã bắt đầu khi bạn muốn `preventDefault` để chặn cuộn dọc. Bạn có thể kiểm tra bằng cách dùng `event.cancelable`.

```javascript
document.body.addEventListener('pointermove', event => {
    if (event.cancelable) {
        event.preventDefault(); // block the native scroll
        /*
        *  do what you want the application to do here
        */
    }
}, {passive: true});
```

Thay vào đó, bạn có thể dùng CSS rule như `touch-action` để loại bỏ hoàn toàn event handler.

```javascript
#area {
  touch-action: pan-x;
}
```

## Tìm event target

Khi compositor thread gửi một input event đến main thread, nó sẽ chạy một hit test để tìm event target. Hit test dùng dữ liệu paint record, được tạo ra trong rendering process để tìm hiểu những thứ nằm dưới toạ độ điểm mà event xảy ra.

<figure markdown>
![Hình 6: Main thread kiểm tra paint record để xem điểm x.y có gì](../../assets/misc/chrome-blog/inside-browser-4/figure6.avif){:class="centered-img h-300"}
<figcaption>Hình 6: Main thread kiểm tra paint record để xem điểm x.y có gì</figcaption>
</figure>

## Giảm thiểu việc gửi event đến main thread

Trong bài trước, ta đã xem cách màn hình refresh 60 lần trên giây và cách mà ta cần theo kịp điều đó để có hoạt ảnh mượt mà. Với input, một thiết bị màn hình cảm ứng thông thường cung cấp event chạm 60 - 120 lần mỗi giây. Input event rõ ràng là nhiều hơn mức refresh của màn hình.

Nếu một event liên tục như `touchmove` được gửi đến main thread 120 lần mỗi giây, nó sẽ kích hoạt quá nhiều hit test và thực thi JavaScript so với mức mà màn hình có thể refresh.

<figure markdown>
![Hình 7: Quá nhiều event trong khung thời gian làm trang bị giật](../../assets/misc/chrome-blog/inside-browser-4/figure7.avif){:class="centered-img h-200"}
<figcaption>Hình 7: Quá nhiều event trong khung thời gian làm trang bị giật</figcaption>
</figure>

Để giảm thiểu quá nhiều lệnh gọi đến main thread, Chrome kết hợp các event liên tục (như `wheel`, `mousewheel`, `mousemove`, `pointermove`, `touchmove`) và trì hoãn việc gửi đến main thread cho đến ngay trước `requestAnimationFrame` tiếp theo.

<figure markdown>
![Hình 8: Vẫn cái dòng thời gian đó, nhưng các event được kết hợp và trì hoãn](../../assets/misc/chrome-blog/inside-browser-4/figure8.avif){:class="centered-img h-200"}
<figcaption>Hình 8: Vẫn cái dòng thời gian đó, nhưng các event được kết hợp và trì hoãn</figcaption>
</figure>

Các event rời rạc như `keydown`, `keyup`, `mouseup`, `mousedown`, `touchstart`, và `touchend` được gửi ngay lập tức.

## Sử dụng `getCoalescedEvents` để nhận các event trong frame { data-toc-label='Sử dụng getCoalescedEvents để nhận các event trong frame' }

Với hầu hết cac ứng dụng web, các event được kết hợp phải đủ để cung cấp trải nghiệm người dùng tốt. Tuy nhiên, nếu bạn đang làm những thứ như ứng dụng vẽ và tạo một đường thẳng dựa trên các toạ độ `touchmove`, bạn có thể mất các toạ độ ở giữa để vẽ ra một đường thẳng đẹp. Trong trường hợp đó, bạn có thể dùng phương thức `getCoalescedEvents` trong pointer event để lấy thông tin về các event kết hợp đó.

<figure markdown>
![Hình 9: Hành động touch trong thực tế ở bên trái, và sau khi bị kết hợp ở bên phải](../../assets/misc/chrome-blog/inside-browser-4/figure9.avif){:class="centered-img h-300"}
<figcaption>Hình 9: Hành động touch trong thực tế ở bên trái, và sau khi bị kết hợp ở bên phải</figcaption>
</figure>

```javascript
window.addEventListener('pointermove', event => {
    const events = event.getCoalescedEvents();
    for (let event of events) {
        const x = event.pageX;
        const y = event.pageY;
        // draw a line using x and y coordinates.
    }
});
```

## Các bước tiếp theo

Trong series này, ta đã đề cập đến các hoạt động bên trong một trình duyệt web. Nếu bạn chưa bao giờ nghĩ vì sao DevTools khuyên bạn thêm `{passive: true}` vào event handler hoặc vì sao bạn có thể viết thuộc tính `async` vào thẻ script, mình hi vọng series có thể làm rõ vì sao trình duyệt cần các thông tin đó để cung cấp trải nghiệm duyệt web mượt mà và nhanh hơn.

### Dùng Lighthouse

Nếu bạn muốn làm cho code của mình phù hợp với trình duyệt nhưng không biết bắt đầu từ đâu, thì [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) là một công cụ chạy kiểm tra bất kỳ trang web nào và cung cấp cho bạn báo cáo về những gì đang được thực hiện đúng và những gì cần phải cải thiện. Đọc qua danh sách kiểm tra cũng cho bạn ý tưởng về những thứ mà trình duyệt cần.

### Học cách đo lường hiệu suất

Các điều chỉnh hiệu suất có thể khác nhau đối với các trang web khác nhau, do đó, điều quan trọng là bạn phải đo hiệu suất trang web của mình và quyết định điều gì phù hợp nhất cho trang web của mình. Team Chrome DevTools có một số hướng dẫn về [cách đo hiệu suất trang web của bạn](https://developers.google.com/web/tools/chrome-devtools/speed/get-started).

### Thêm Feature Policy vào trang của bạn

Nếu bạn muốn đi xa hơn, [Feature Policy](https://developers.google.com/web/updates/2018/06/feature-policy) là một tính năng web mới có thể là rào cản cho bạn khi bạn đang tạo ra website cho mình. Việc bật feature policy sẽ đảm bảo hành vi nhất định của ứng dụng và ngăn bạn mắc lỗi. Ví dụ, nếu muốn đảm bảo ứng dụng của mình sẽ không bao giờ chặn parsing, thì bạn có thể chạy ứng dụng của mình theo synchronous scripts policy. Khi có `sync-script: 'none'`, đoạn code JavaScript chặn parser sẽ bị chặn thực thi. Điều này ngăn bất kỳ code nào của bạn chặn parser và trình duyệt không cần phải lo về việc tạm dừng parse.

## Kết phần 4

Khi mới bắt đầu xây dựng website, mình gần như chỉ quan tâm đến việc làm sao để viết code và làm gì để trở nên năng suất hơn. Những thứ đó quan trọng thật, nhưng ta cũng cần biết làm sao trình duyệt đọc được code của chúng ta. Trình duyệt hiện đại đã và đang tiếp tục nâng cao trải nghiệm web cho người dùng. Đối xử tốt với trình duyệt bằng cách tổ chức tốt code của chúng ta sẽ dẫn đến việc cải thiện trải nghiệm người dùng. Mình hi vọng các bạn sẽ tham gia cùng mình trong việc đối xử tốt với trình duyệt!
