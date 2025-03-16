# CSS Methodology và các Best Practice với CSS

## Những điều cần lưu ý khi viết CSS

Đầu tiên, ta sẽ cần phải nhất quán. Tìm kiếm code trong dự án phải dễ dàng, và bạn cũng cần phải nhớ component nào có các class nào.

Thứ hai, ta cần tránh CSS can thiệp vào HTML, nghĩa là tránh viết code CSS trực tiếp vào file HTML. Output lý tưởng của HTML là một cái file không có styling gì cả, nhưng cái này khá khó đạt được.

Thứ ba, ta sẽ phải tập trung vào thiết kế responsive, tránh các giá trị tuyệt đối. Việc này sẽ giúp cho website scale tốt hơn trên các thiết bị khác nhau.

Thứ tư, tôn trọng thứ tự sử dụng stylesheet (cascade), tránh dùng `!important` nếu không cần thiết.

Cuối cùng, tránh lặp lại code, nên sử dụng biến và gộp các component giống nhau lại với nhau.

## Một số cách áp dụng

### BEM (Block Element Modifier)

BEM chia CSS thành 3 phần: block, element, và modifier. 

- Block là một thực thể độc lập với một ý nghĩa độc lập. Ví dụ nếu ta có một menu, thì menu đó sẽ là một block, khi gọi nó bằng class name, ta sẽ gọi nó là `menu`.
- Element là một phần của block, không thể tồn tại một cách độc lập. Ví dụ, trong menu, ta có các item, thì item đó sẽ là một element, khi gọi nó bằng class name, ta sẽ gọi nó là `menu__item`.
- Modifier là những flag cho element hay block dùng để thay đổi trạng thái hoặc kiểu dáng của element và block. Ví dụ, ta có thể thêm một modifier `menu__item--active` để chỉ ra rằng item đó đang được chọn.

Điểm yếu của BEM là làm cho HTML dài dòng với những class name dài, nhưng cũng giúp CSS dễ viết và bảo trì hơn.

### OOCSS (Object Oriented CSS)

OOCSS dựa trên concept lập trình hướng đối tượng. Ta xem các component như các object, sau đó chia các style thành hai loại.

- Structure: các thuộc tính vô hình, như chiều rộng, chiều cao, margin, padding, ...
- Skin: các thuộc tính liên quan đến màu sắc, hình ảnh, font, ...

Trong OOCSS, ta phân biệt rõ ràng giữa nội dung và container, nghĩa là ta tránh các kết hợp giữa chúng. Điểm yếu của OOCSS là ta thường có các class rất nhỏ, làm code trở nên phức tạp hơn.

### Atomic CSS

Trong Atomic CSS, ta cần tránh các khai báo lặp lại, thay vào đó ta sẽ tạo ra các class nhỏ, mỗi class sẽ chỉ chứa một thuộc tính. Ví dụ, ta có class `m-0` để set margin thành 0, `p-0` để set padding thành 0, ...

### SMACSS (Scalable and Modular Architecture for CSS)

Trong SMACSS, ta cần chia CSS thành các file dựa trên use case, cụ thể là 5 file (với các project đơn giản thì ta cũng không cần hết 5 file)

- Base: chứa các style mặc định, type selector, ...
- Layout: chứa các style cho layout, ID và class selector. Các class thường sẽ có tiền tố `l-`.
- Module: chứa các component nhỏ, class selector và không có tiền tố.
- State: chứa các trạng thái cho các module, layout, ví dụ như disabled. Các class thường sẽ có tiền tố `is-`.
- Theme: chứa các style cho module và layout, dựa theo sở thích của người dùng.

### ITCSS (Inverted Triangle CSS)

ITCSS quy định một cấu trúc CSS theo hình tam giác đảo, với các layer từ dễ nhìn nhất đến khó nhìn nhất. Từ phần rộng nhất của tam giác, đó sẽ là các style generic, style mà ảnh hưởng nhiều nhất đến website, càng ít chi tiết càng tốt. Càng vào sâu, ta sẽ thấy các style cụ thể hơn.

- Settings: chứa các biến và config cho project.
- Tools: chứa các mixins và function.
- Generic: chứa các style mà ảnh hưởng nhiều nhất đến website, thường để reset style mặc định của trình duyệt.
- Elements: chứa các style cho các thẻ HTML, như `h1`, `a`, ...
- Objects: chứa các style cho các object, những generic class cho website và thường được dùng bởi các container lớn.
- Components: chứa các style cho các component nhỏ, như `button`, `menu`, ...
- Trumps: chứa các style override, như `!important`.

## Các vấn đề hiệu suất

Thứ nhất, trình duyệt đọc CSS từ phải qua trái, ví dụ `#specific > p` thì trình duyệt sẽ tìm tất cả `p` rồi mới tìm `#specific`.

Thứ hai, tránh các thuộc tính có chi phí cao, phức tạp, như `box-shadow`, `transform`, `position: fixed`, ... Nhiều thuộc tính kiểu vậy sẽ làm website chậm đi.

Thứ ba, tránh ghi các code không dùng tới vì nó làm tăng kích thước CSS và gây tốn thời gian cho trình duyệt. Có nhiều lý do khiến các code rác xuất hiện, như việc xóa code HTML mà không xóa code CSS liên quan, hay khi trang web có quy mô quá lớn và tất cả CSS nằm trong một file. Ta nên đưa trình duyệt các file CSS mà nó cần cho trang web hiện tại hoặc có khả năng trỏ tới.

Thứ tư, dùng các công cụ để minify CSS để giảm kích thước các file CSS.

Cuối cùng, hoãn tải một số file CSS không quan trọng để render nhanh hơn. Một cách là đổi `rel` của link CSS từ `stylesheet` sang `preload`, để báo cho trình duyệt biết rằng file CSS đó cần được tải trước khi render trang, nhớ thêm `as="style"` vào để trình duyệt biết đó là file CSS. Trong phần `onload`, ta nên dùng `this.onload=null; this.rel='stylesheet'` để tránh việc tải file CSS hai lần và trả lại thuộc tính `rel` về `stylesheet`. Ngoài ra, ta cần bọc `<link>` trong một thẻ `<noscript>` để tránh việc trình duyệt không hỗ trợ JavaScript, việc này sẽ khiến code trước đó không thực hiện được và ta phải chấp nhận tải chậm file CSS.

```html
<link rel="preload" as="style" href="style.css" onload="this.onload=null; this.rel='stylesheet'">
<noscript>
    <link rel="stylesheet" href="style.css">
</noscript>
```
