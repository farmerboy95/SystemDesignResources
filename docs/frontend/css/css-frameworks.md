# Các framework và preprocessor CSS phổ biến

## Định nghĩa

CSS framework là các code được viết sẵn, từ đó giúp ta tăng tốc dộ phát triển website, thường là các stylesheet được viết sẵn hoặc các component được build trước.

CSS preprocessor là cách để thêm chức năng vào ngôn ngữ. Trình duyệt chỉ hiểu được CSS nguyên thủy, nhưng các preprocessor là các chương trình sẽ chuyển đổi một số code khác thành CSS để có thêm chức năng cho CSS, mục đích cuối cùng cũng là để tăng tốc độ phát triển và cho phép ta viết code chất lượng tốt và dễ bảo trì hơn.

## Các framework CSS phổ biến

Đầu tiên là **Tailwind**. Đây là framework cung cấp những class để dùng trong HTML, cho phép ta dùng các CSS có sẵn của framework. Ví dụ, nó có thể tạo các class để dùng các màu nền khác nhau trên trang web. Ta có thể dùng các class đó trên các element của HTML và không phải viết CSS. Với Tailwind, ta sẽ không phải viết nhiều CSS, rất phù hợp để tạo các trang web nhanh chóng và responsive. Ngược lại, dùng quá nhiều class sẽ làm người ta ngại dùng, vì họ sẽ cảm thấy styling đều phải bỏ hết vào HTML, làm cho nó khó đọc. Tựu chung lại, đây là một lựa chọn rất tốt và được dùng rất nhiều hiện nay.

Tiếp theo là **Bootstrap**. Đây là framework CSS phổ biến nhất hiện nay, được phát triển bởi Twitter với ý định tạo ra các thiết kế nhất quán và responsive. Bootstrap bao gồm nhiều component build trước mà bạn có thể dùng để tạo website chuyên nghiệp mà lại ít tốn công sức. Tuy nhiên, do nó quá phổ biến nên bạn sẽ cần cẩn thận khi dùng các component này mà không chỉnh sửa đôi chút, vì nó sẽ làm website của bạn trông không khác gì các website khác cũng dùng Bootstrap. Bootstrap cũng có một hệ thống grid layout mạnh mẽ, tương tự như cái có sẵn trong CSS, giúp cho việc tạo ra các layout chất lượng cao nằm trong tầm tay bạn. Bootstrap cũng có một điểm yếu nữa là nó thêm một file CSS rất lớn vào website của bạn, làm website load chậm hơn và cũng khó customize các component hơn vì bạn cần phải tìm kiếm để xem phải sửa các thuộc tính nào.

Tiếp đến là **Materialize**. Nó kiểu như Bootstrap nhưng dựa trên Hệ thống Thiết kế Material UI của Google. Nó có hệ thống grid layout riêng với nhiều component build trước chất lượng cao mà bạn có thể dùng trực tiếp trên website của mình. Material UI của Google được lấy cảm hứng từ concept của giấy và mực, và Google tập trung tạo ra các thiết kế mang cảm giác quen thuộc ngay cả khi ở trên màn hình và rất thành công với chúng. Điểm yếu nhất ở đây cũng là điểm mạnh nhất là bạn sẽ phải theo Hệ thống Thiết kế Material UI, và phải nhất quán với các component này. Nói chung, đây là một hệ thống thiết kế rất mạnh mẽ, tuy nhiên khi bạn muốn tùy chính, bạn sẽ phải theo các nguyên tắc của họ.

Vị khách tiếp theo là **Foundation**, một framework với nhiều component build trước và hệ thống grid layout để thiết kế responsive. Foundation cho phép tùy chỉnh dễ dàng hơn các framework khác, nhưng không phải để nói rằng các framework khác không cho tùy chỉnh hoắc Foundation là framework duy nhất làm được chuyện này. Thường thì yếu tố lớn nhất để chọn framework là hệ thống thiết kế có phù hợp với website của bạn hay không.

Cuối cùng, **Bulma** là framework ít phổ biến nhất, nhưng nó là framework chỉ có CSS. Các framework khác thường có cả JavaScript, nhưng Bulma thì không, cho nên Bulma ít bị phàn nàn về việc cài đặt component, vì nó chỉ tập trung vào styling. Tuy nhiên, điều đó cũng có nghĩa rằng bạn cần phải làm nhiều hơn để giúp các component chạy tốt, và tất nhiên là phải viết nhiều code JavaScript hơn.

## Các preprocessor CSS phổ biến

Đầu tiên là **Sass**, preprocessor CSS phổ biến nhất, đây là bản mở rộng của CSS với một số cú pháp và chức năng mới. Ví dụ, nó hỗ trợ biến và mixin, cho phép bạn dùng lại code, kế thừa các class, và nhiều hơn nữa.

**Less** ít phổ biến hơn, nhưng cũng tương tự cái ở trên. Điểm khác biệt lớn nhất giữa cả hai là Sass dùng Ruby, còn Less được viết bằng JavaScript, cho nên điểm khác sẽ là về cú pháp vì chúng có những chức năng tương tự nhau nhưng với cú pháp khác nhau.

Tiếp theo là **Stylus**, chức năng giống Sass, nhưng khác về cú pháp, nhưng cú pháp ở đây đơn giản hơn, bỏ đi nhiều dấu ngoặc và dấu chấm phẩy, giúp cho code ngắn gọn hơn. Nhiều dev từ Python thấy Stylus rất hợp với mình vì cú pháp CSS trở nên giống Python khi dùng Stylus.

Cuối cùng là **PostCSS**, nó được viết bằng JavaScript nhưng tập trung vào việc tùy chỉnh mạnh hơn. Nó có nhiều plugin và bạn có thể chọn plugin để chuyển đổi CSS, giống việc tạo preprocessor CSS cho một project cụ thể nào đó.

## Những thứ này có thực sự cần thiết không?

Cũng tùy.

Với framework, nó phụ thuộc vào project bạn đang có. Với các project với team nhỏ hay deadline ngắn, ta nên dùng framework, nó sẽ giúp tăng tốc độ phát triển và tập trung vào các tính năng thay vì tạo CSS. Tuy nhiên, khi project to ra, ta sẽ cần tạo các component riêng để làm cho website độc đáo và có tính tùy chỉnh cao. Tuy nhiên, với các project lớn này, ta vẫn cần framework và nhiều công ty vẫn dùng, nhưng họ bỏ nhiều công sức tùy chỉnh các component.

Với preprocessor, thường là nên dùng. Nó sẽ giúp bạn viết CSS dễ hơn, bảo trì tốt hơn và ít lỗi hơn. Không phải nói vậy mà chê CSS thuần túy, thêm vào đó, bạn nên học và nâng cao kiến thức CSS. Hiểu CSS thuần túy sẽ giúp bạn hiểu rõ và sử dụng các công cụ này tốt hơn.
