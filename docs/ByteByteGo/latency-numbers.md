# Các mốc độ trễ mà lập trình viên nên biết

## Nguồn

<img src="../../assets/images/bytebytego.png" width="16" height="16"/> [Latency Numbers Programmer Should Know](https://www.youtube.com/watch?v=FqR5vESuKe0)

## Tại sao nên nói về độ trễ?

Độ trễ (latency) là một yếu tố quan trọng trong các hệ thống ngày nay. Thực sự chúng ta không cần biết độ trễ chính xác mà chỉ cần ghi nhớ một số khoảng độ trễ nhất định để hình dung ra thứ ta đang làm sẽ tốn khoảng trễ như thế nào.

Ta sẽ đi vào từng khoảng thời gian trễ để xem có những ứng dụng nào liên quan đến chúng hay không.

![](../assets/ByteByteGo/latency-numbers/figure1.png){:class="centered-img h-500"}

![](../assets/ByteByteGo/latency-numbers/figure2.png){:class="centered-img h-200"}

## 1 nano giây

Truy cập vào các thanh ghi CPU tốn khoảng dưới 1 nano giây. Ta chỉ có vài thanh ghi trong CPU nhưng truy cập chúng lại rất nhanh.

![](../assets/ByteByteGo/latency-numbers/figure3.png){:class="centered-img h-500"}

Chu kỳ xung nhịp (clock cycle) của máy tính cũng tốn khoảng dưới 1 nano giây.

![](../assets/ByteByteGo/latency-numbers/figure4.png){:class="centered-img h-200"}

## 1-10 nano giây

Truy cập vào bộ nhớ đệm (cache) L1 và L2 của CPU tốn từ 1 đến 10 nano giây. Một số thao tác tốn kém của CPU cũng ở trong khoảng này, ví dụ như branch mispredict penalty, tốn khoảng 20 chu kỳ xung nhịp.

![](../assets/ByteByteGo/latency-numbers/figure5.png){:class="centered-img h-500"}

![](../assets/ByteByteGo/latency-numbers/figure6.png){:class="centered-img h-500"}

## 10-100 nano giây

Truy cập vào bộ nhớ đệm L3 của CPU thường tốn khoảng 10 nano giây. 

![](../assets/ByteByteGo/latency-numbers/figure7.png){:class="centered-img h-500"}

Với các bộ xử lý hiện đại như Apple M1, tham chiếu vào bộ nhớ chính tốn khoảng gần 100 nano giây. Nói cách khác, truy cập vào bộ nhớ chính của máy tính hiện đại sẽ chậm hơn vài trăm lần truy cập vào thanh ghi CPU.

## 100-1000 nano giây

Khoảng này sẽ gồm lệnh gọi hệ thống (system call). Trong Linux, gọi một lệnh hệ thống đơn giản sẽ tốn khoảng vài trăm nano giây. Lưu ý phần lớn thời gian trong này là chi phí để di chuyển vào kernel và đi ra, thời gian thực gọi lệnh hệ thống là rất ít.

![](../assets/ByteByteGo/latency-numbers/figure8.png){:class="centered-img h-500"}

Ngoài ra, ta còn tốn khoảng 200 nano giây để MD5 hash một số 64 bit.

![](../assets/ByteByteGo/latency-numbers/figure9.png){:class="centered-img h-500"}

## 1-10 micro giây

Context switch giữa các thread trong Linux tốn tầm khoảng vài micro giây. Dù vậy thì đây là trường hợp tốt nhất để có độ trễ này. Tuỳ vào workload, nếu context switch bao gồm việc mang các trang dữ liệu từ bộ nhớ cho luồng mới, nó có thể tốn nhiều thời gian hơn.

![](../assets/ByteByteGo/latency-numbers/figure10.png){:class="centered-img h-300"}

Copy 64KB dữ liệu từ một chỗ trong bộ nhớ chính sang chỗ khác (cũng trong bộ nhớ chính) cũng tốn vài micro giây.

![](../assets/ByteByteGo/latency-numbers/figure11.png){:class="centered-img h-300"}

## 10-100 micro giây

Network proxy như Nginx cần khoảng 50 micro giây để xử lý một HTTP request.

![](../assets/ByteByteGo/latency-numbers/figure12.png){:class="centered-img h-300"}

Đọc 1MB liên tiếp từ bộ nhớ chính sẽ tốn khoảng 50 micro giây.

![](../assets/ByteByteGo/latency-numbers/figure13.png){:class="centered-img h-300"}

Độ trễ đọc của ổ cứng SSD cũng trong khoảng này, tốn tầm 100 micro giây để đọc một trang 8K.

![](../assets/ByteByteGo/latency-numbers/figure14.png){:class="centered-img h-300"}

## 100-1000 micro giây

Độ trễ ghi của ở cứng SSD lớn hơn tầm 10 lần so với độ trễ đọc, gần 1000 micro giây để ghi một trang.

![](../assets/ByteByteGo/latency-numbers/figure15.png){:class="centered-img h-300"}

Thời gian trễ trọn vòng của mạng nội vùng (với các nền tảng đám mây hiện nay) là tầm khoảng vài trăm micro giây. Ngày nay độ trễ này có xu hướng giảm, đôi khi về dưới 100 micro giây.

![](../assets/ByteByteGo/latency-numbers/figure16.png){:class="centered-img h-300"}

Một thao tác thông thường trên Memcache hay Redis tốn khoảng 1000 micro giây, bao gồm cả thời gian trễ trọn vòng ở trên.

![](../assets/ByteByteGo/latency-numbers/figure17.png){:class="centered-img h-300"}

## 1-10 mili giây

Thời gian trễ trọn vòng của mạng xuyên vùng là vào khoảng 5 mili giây.

![](../assets/ByteByteGo/latency-numbers/figure18.png){:class="centered-img h-300"}

Thời gian tìm (seek time) của ổ cứng cũng vào khoảng 5 mili giây.

![](../assets/ByteByteGo/latency-numbers/figure19.png){:class="centered-img h-300"}

## 10-100 mili giây

Thời gian trễ trọn vòng của bờ đông và bờ tây nước Mỹ, hay bờ đông nước Mỹ và châu Âu nằm trong khoảng này.

![](../assets/ByteByteGo/latency-numbers/figure20.png){:class="centered-img h-300"}

Đọc 1GB liên tiếp từ bộ nhớ chính cũng nằm trong khoảng này.

![](../assets/ByteByteGo/latency-numbers/figure21.png){:class="centered-img h-300"}

## 100-1000 mili giây

Mất tầm 300 mili giây để bcrypt một mật khẩu.

![](../assets/ByteByteGo/latency-numbers/figure22.png){:class="centered-img h-300"}

Bắt tay TLS nằm trong khoảng 250 đến 500 mili giây. Nó bao gồm nhiều network round trip nên con số sẽ tuỳ vào khoảng cách giữa 2 máy.

![](../assets/ByteByteGo/latency-numbers/figure23.png){:class="centered-img h-500"}

Thời gian trễ trọn vòng với mạng từ bờ tây nước Mỹ đến Singapore cũng trong khoảng 100 đến 1000 mili giây.

![](../assets/ByteByteGo/latency-numbers/figure24.png){:class="centered-img h-300"}

Đọc 1GB liên tiếp từ SSD cũng trong khoảng này.

## 1 giây

Chuyển 1GB trên mạng trong cùng một vùng đám mây tốn khoảng 10 giây.

![](../assets/ByteByteGo/latency-numbers/figure25.png){:class="centered-img h-300"}
