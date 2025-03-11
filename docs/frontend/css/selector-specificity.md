# Selector Specificity

## Xung đột trong CSS

Giả sử ta có code CSS như sau:

```css
a[href="www.google.com"] {
    color: green;
}

section.links a {
    color: red;
}
```

Ta có một rule set chọn thẻ `<a>` có `href` là `www.google.com` và một rule set chọn thẻ `<a>` trong một `section` có class `links`. Màu trong mỗi rule set là khác nhau. Giờ giả sử ta có một thẻ `<a>` có `href` là `www.google.com` và nằm trong một `section` có class `links`. Vậy màu của thẻ `<a>` đó sẽ là màu gì?

CSS xử lý vấn đề này thông qua cơ chế **selector specificity**. Nghĩa là, 

- Selector nào càng cụ thể (specific) thì sẽ có trọng số cao hơn và được ưu tiên áp dụng. 
- Nếu có hai selector có cùng trọng số, thì selector nào xuất hiện sau cùng trong file CSS sẽ được áp dụng.

Nhưng làm sao để xác định trọng số của selector? Mà "cụ thể" nghĩa là sao? Ta cần làm một chút toán ở đây. Ta chia selector ra thành các loại khác nhau, cho mỗi loại một giá trị, sau đó với rule set, đếm mỗi loại có bao nhiêu, rồi nhân chúng với giá trị mà ta cho. Cuối cùng ta sẽ có một số gọi là trọng số, sau đó ta chọn rule nào có trọng số cao nhất.

## Giá trị của selector

- Inline style (các style được viết trực tiếp trong thẻ HTML): 1000
- ID selector: 100
- Class selector: 10
- Pseudo class: 10
- Attribute selector: 10
- Element selector: 1
- Pseudo element: 1

Quay lại với ví dụ trên, trong rule set đầu tiên, ta có thẻ `<a>`, nghĩa là element selector (+1), tiếp đến là `href`, một attribute selector (+10). Như vậy rule set đầu tiên có trọng số là 1 + 10 = 11.

Trong rule set thứ hai, ta có `section`, một element selector (+1), và `links`, một class selector (+10), và cuối cùng là thẻ `<a>`, một element selector (+1). Như vậy rule set thứ hai có trọng số là 1 + 10 + 1 = 12.

Vậy rule set thứ hai sẽ "cụ thể" hơn và được ưu tiên áp dụng khi cả hai rule set đều đúng.

## !important

Ở trên chính là cách trình duyệt sử dụng để chọn ra rule set nào được áp dụng. Tuy nhiên, đôi khi ta cần một cách để bắt buộc trình duyệt áp dụng một rule set nào đó, dù nó không cụ thể hơn. Để làm điều này, ta sử dụng `!important`.

```css
a {
    color: red !important;
}
```

Khi một rule set có `!important`, nó sẽ được ưu tiên hơn tất cả các rule set khác, kể cả rule set có trọng số cao nhất. Tuy nhiên ta cần tránh sử dụng `!important` quá nhiều, vì nó đi ngược lại với cơ chế selector specificity, làm cho code CSS khó hiểu và khó bảo trì.

