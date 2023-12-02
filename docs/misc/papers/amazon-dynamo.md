# Amazon DynamoDB

## Nguồn

[Dynamo: Amazon’s Highly Available Key-value Store](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)

Các tác giả:

- Giuseppe DeCandia
- Deniz Hastorun
- Madan Jampani
- Gunavardhan Kakulapati
- Avinash Lakshman
- Alex Pilchin
- Swaminathan Sivasubramanian
- Peter Vosshall
- Werner Vogels

## Tóm tắt

Độ tin cậy (Reliability) ở quy mô lớn là một trong những thách thức mà chúng tôi gặp phải tại Amazon.com, một trong những tập đoàn thương mại điện tử lớn nhất thế giới; ngay cả một vài giây gián đoạn cũng gây ra hậu quả tài chính to lớn và ảnh hưởng đến lòng tin của người dùng. Nền tảng Amazon.com, nơi cung cấp dịch vụ cho rất nhiều trang web toàn cầu, được xây dựng trên một hạ tầng bao gồm hàng chục nghìn server và thành phần mạng được đặt ở rất nhiều trung tâm dữ liệu trên thế giới. Ở quy mô này, các thành phần nhỏ và lớn có thể liên tục gặp sự cố và cách để quản lý trạng thái liên tục khi đối mặt với những lỗi này giúp thúc đẩy độ tin cậy và tính mở rộng (scalability) của hệ thống phần mềm.

Bài báo này giới thiệu thiết kế và cài đặt của Dynamo, một hệ thống lưu trữ khóa-giá trị (key-value) có tính khả dụng cao (highly available) được sử dụng bởi một số hệ thống quan trọng của Amazon để cung cấp một trải nghiệm không gián đoạn cho người dùng. Để đạt được mục tiêu này, Dynamo hi sinh tính nhất quán (consistency) trong một số trường hợp cụ thể. Dynamo còn sử dụng nhiều kỹ thuật như phiên bản hóa (versioning) và giải quyết xung đột (conflict resolution) với sự giúp sức của tầng ứng dụng theo cách cung cấp một giao diện mới mẻ cho nhà phát triển.

## 1. Giới thiệu

Amazon vận hành một nền tảng thương mại điện tử toàn cầu phục vụ hàng chục triệu khách hàng trong giờ cao điểm với hàng chục nghìn server ở nhiều trung tâm dữ liệu trên thế giới. Có nhiều yêu cầu hoạt động nghiêm ngặt trên nền tảng của Amazon về hiệu suất, độ tin cậy và độ hiệu quả. Độ tin cậy là một trong các yêu cầu quan trọng nhất vì một vài giây gián đoạn có thể gây ra hậu quả tài chính to lớn và ảnh hưởng đến lòng tin của người dùng. Thêm nữa, để đáp ứng nhu cầu tăng trưởng liên tục, nền tảng cần có tính mở rộng cao để có thể dễ dàng mở rộng hệ thống khi cần thiết.

Một trong những bài học mà tổ chức của chúng tôi đã học được khi vận hành nền tảng của Amazon là độ tin cậy và tính mở rộng của một hệ thống phụ thuộc vào cách trạng thái ứng dụng của nó được quản lý. Amazon sử dụng một kiến trúc hướng dịch vụ có tính phi tập trung cao (highly decentralized), liên kết lỏng lẻo (loosely coupled), bao gồm hàng trăm dịch vụ. Trong môi trường này, sẽ có nhu cầu đặc biệt về các công nghệ lưu trữ có tính khả dụng. Ví dụ, khách hàng có thể xem và thêm các mặt hàng vào giỏ hàng của mình ngay cả khi ổ đĩa bị lỗi, các tuyến mạng bị lỗi hoặc các trung tâm dữ liệu bị tàn phá bởi thiên tai. Do đó, dịch vụ chịu trách nhiệm quản lý giỏ hàng yêu cầu rằng dịch vụ này luôn có thể ghi và đọc từ kho lưu trữ dữ liệu của mình và dữ liệu của nó cần phải khả dụng (available) trên nhiều trung tâm dữ liệu.

Xử lý vấn đề trong một hạ tầng gồm hàng triệu thành phần là việc thường xuyên của chúng tôi; luôn có một lượng nhỏ nhưng đáng kể các thành phần máy chủ và mạng có thể bị lỗi ở bất kỳ lúc nào. Như vậy các hệ thống phần mềm của Amazon cần phải được xây dựng theo cách mà chúng xem việc xử lý lỗi như chuyện thường ngày ở huyện mà không ảnh hưởng đến tính khả dụng hoặc hiệu suất.

Để đáp ứng các như cầu về độ tin cậy và tính mở rộng, Amazon đã phát triển nhiều công nghệ lưu trữ, trong đó Amazon Simple Storage Service (cũng đã ra mắt công chúng và được biết đến với cái tên Amazon S3) là nổi tiếng nhất. Bài báo này này trình bày thiết kế và cài đặt Dynamo, một kho dữ liệu phân tán có tính khả dụng và tính mở rộng cao khác, được xây dựng cho nền tảng của Amazon. Dynamo được sử  dụng để quản lý trạng thái của các dịch vụ có nhiều yêu cầu về độ tin cậy vào cùng với việc kiểm soát chặt chẽ những sự cân bằng về tính khả dụng, tính nhất quán, hiệu quả chi phí và hiệu suất. Nền tảng của Amazon có một tập rất đa dạng các ứng dụng với nhiều yêu cầu lưu trữ khác nhau. Một tập các ứng dụng như thế yêu cầu một công nghệ lưu trữ đủ linh hoạt để cho phép các nhà thiết kế ứng dụng cấu hình kho dữ liệu của họ một cách thích hợp, dựa trên những sự cân bằng ở trên để đạt được tính khả dụng cao và đảm bảo hiệu quả với chi phí hợp lý nhất.

Có rất nhiều dịch vụ trên nền tảng của Amazon mà chỉ cần truy cập bằng khóa chính để lấy dữ liệu. Với nhiều dịch vụ, như danh sách các nhà bán hàng tốt nhất, giỏ hàng, sở thích khách hàng, quản lý phiên, thứ hạng bán hàng và danh mục sản phẩm, mô hình chung của việc dụng cơ sở dữ liệu quan hệ (relational database) sẽ dẫn đến sự kém hiệu quả và hạn chế quy mô và tính khả dụng. Dynamo cung cấp một giao diện chỉ có khóa chính đơn giản để đáp ứng yêu cầu của các ứng dụng này.

Dynamo sử dụng kết hợp các ký thuật phổ biến để đạt được tính khả dụng và tính mở rộng. Dữ liệu được phân vùng và sao chép bằng cách sử dụng hàm băm nhất quán (consistent hashing) [10], và tính nhất quán được hỗ trợ bằng cách phiên bản hóa đối tượng (object versioning) [12]. Tính nhất quán giữa các bản sao dữ liệu trong quá trình cập nhật được duy trì bằng một kỹ thuật kiểu quorum và một giao thức đồng bộ hóa bản sao phi tập trung (decentralized replica synchronization protocol).

Dynamo sử dụng một giao thức thành viên và phát hiện lỗi phân tán dựa trên gossip. Dynamo là một hệ thống hoàn toàn phi tập trung với việc quản trị thủ công giảm đến mức tối thiểu. Các node lưu trữ có thể được thêm vào và bớt khỏi Dynamo mà không yêu cầu bất kỳ việc phân vùng và phân phối lại thủ công nào.

Trong năm qua, Dynamo là công nghệ lưu trữ đằng sau một cơ số các dịch vụ cốt lõi trong nền tảng thương mại điện tử của Amazon. Nó có thể tăng quy mô lên mức cực đại hiệu quả mà không có thời gian chết trong mùa mua sắm. Ví dụ, dịch vụ quản lý giỏ hàng phục vụ hàng chục triệu yêu cầu đã mang lại hơn 3 triệu lượt thanh toán trong một ngày và dịch vụ quản lý phiên đã sử lý hàng trăm nghìn phiên hoạt động đồng thời.

Đóng góp chính của công việc này cho cộng đồng nghiên cứu là việc đánh giá làm thế nào các kỹ thuật khác nhau có thể được kết hợp để tạo ra một hệ thống có tính khả dụng cao. Nó chứng tỏ rằng một hệ thống lưu trữ eventually-consistent có thể được dùng trong các ứng dụng cần nó. Nó cũng cung cấp cái nhìn sâu sắc về việc điều chỉnh các kỹ thuật này để đáp ứng các nhu cầu của hệ thống thực với rất nhiều yêu cầu hiệu suất khắt khe.

Bài báo này được trình bày như sau. Phần 2 giới thiệu bối cảnh và Phần 3 trình bày các công việc liên quan. Phần 4 trình bày thiết kế hệ thống và Phần 5 mô tả cài đặt. Phần 6 trình bày chi tiết những kinh nghiệm và hiểu biết của chúng tôi khi vận hành Dynamo trong thực tế và Phần 7 kết luận bài báo. Có một số  chỗ trong bài viết này cần thêm một số thông tin hay ho nữa nhưng việc bảo vệ lợi ích kinh doanh của Amazon yêu cầu chúng tôi phải giảm các chi tiết này đi. Vì lý do này, phần về độ trễ trong và giữa các trung tâm dữ liệu trong phần 6, tỉ lệ request tuyệt đối trong phần 6.2, thời gian ngừng hoạt động và khối lượng công việc trong phần 6.3 được cung cấp thông qua các chỉ số tổng hợp thay vì các chi tiết cụ thể.

## 2. Bối cảnh

Nền tảng thương mại điện tử của Amazon được kết hợp bởi hàng trăm dịch vụ hoạt động phối hợp với nhau để cung cấp các chức năng khác nhau từ đề xuất, thực hiện đơn hàng đến phát hiện gian lận. Mỗi dịch vụ được thể hiện thông qua một giao diện được xác định rõ ràng và có thể truy cập qua mạng. Các dịch vụ này được host trên một hạ tầng bao gồm hàng chục nghìn server được đặt ở nhiều trung tâm dữ liệu trên thế giới. Một số dịch vụ trong này thì stateless (ví dụ như các dịch vụ tổng hợp phản hồi từ các dịch vụ khác), và một số thì stateful (ví dụ như dịch vụ tạo ra phản hồi bằng cách thực thi các logic nghiệp vụ dựa trên trạng thái được lưu trữ trong kho lưu trữ).

Các hệ thống truyền thống lưu trạng thái của chúng trong các cơ sở dữ liệu quan hệ. Tuy nhiên, đối với nhiều mô hình sử đụng trạng thái bền vững, một cơ sở dữ liệu quan hệ không phải là một lựa chọn tốt. Hầu hết các dịch vụ này chỉ lưu trữ và truy xuất dữ liệu bằng khóa chính và không yêu cầu chức năng quản lý và truy vấn phức tập do cơ sở dữ liệu quan hệ cung cấp. Các chức năng dư thừa đòi hỏi phần cứng đắt tiền và nhân viên có tay nghề cao để vận hành, khiến nó trở thành một giải pháp rất kém hiệu quả. Ngoài ra, các công nghệ nhân rộng dữ liệu hiện thời còn hạn chế và thường chọn tính nhất quán hơn là tính khả dụng. Mặc dù đã có nhiều tiến bộ đạt được trong những năm qua, ta vẫn không dễ để mở rộng các cơ sở dữ liệu hoặc dùng phân vùng thông minh để cân bằng tải.

Bài báo này mô tả Dynamo, một công nghệ lưu trữ dữ liệu có tính khả dụng cao, giúp giải quyết nhu cầu về lớp dịch vụ này. Dynamo có giao diện key-value đơn giản, và có tính khả dụng cao với vùng nhất quán rõ ràng, hiệu quả trong việc sử dụng tài nguyên và có thể mở rộng một cách đơn giản để giải quyết sự tăng trưởng về kích thước tập dữ liệu hoặc tốc độ yêu cầu. Mỗi dịch vụ sử dụng Dynamo đều chạy các phiên bản Dynamo của riêng nó.

### 2.1. Các yêu cầu và giả định cho hệ thống

Hệ thống lưu trữ cho các kiểu dịch vụ này có các yêu cầu sau:

*Mô hình truy vấn:* Các thao tác đọc và ghi đơn giản vào một bản ghi được xác định duy nhất bởi một khóa. Trạng thái được lưu trữ dưới dạng đối tượng nhị phân (như blob) được xác định bởi các khóa duy nhất. Không có thao tác nào liên quan đến nhiều bản ghi và không cần có sơ đồ quan hệ. Yêu cầu này dựa trên một nhận xét rằng phần lớn các dịch vụ của Amazon có thể hoạt động chỉ với mô hình truy vấn này và không cần bất kỳ sơ đồ quan hệ nào. Dynamo nhắm vào các ứng dụng cần lưu các đối tượng có kích thước nhỏ (thường nhỏ hơn 1 MB).

*Tính chất ACID:* ACID (Atomicity - Tính nguyên tử, Consistency - Tính nhất quán, Isolation - Tính độc lập, Durability - Tính bền vững) là một tập các tính chất được đưa ra để đảm bảo các transaction trong cơ sở dữ liệu được thực thi một cách đáng tin cậy. Trong hoàn cảnh của các cơ sở dữ liệu, một thao tác logic đơn trên dữ liệu được gọi là một transaction. Kinh nghiệm ở Amazon cho thấy rằng các kho dữ liệu với tính chất ACID thường có tính khả dụng khá tệ. Điều này đã được kiểm nghiệm bởi cả những công trình nghiên cứu và thực tế  [5]. Dynamo tập trung vào các ứng dụng vận hành với tính nhất quán ít hơn (chữ "C" trong ACID ấy) nếu điều này hỗ trợ tính khả dụng cao hơn. Dynamo không cung cấp sự đảm bảo tính độc lập (isolation guarantee) và chỉ cho phép cập nhật một khóa duy nhất.

*Độ hiệu quả*: Hệ thống cần được hoạt động trên một hạ tầng phần cứng thương mại. Trên nền tảng của Amazon, các dịch vụ có những yêu cầu nghiêm ngặt về độ trễ, thường được đo ở phân vị 99.9 (99.9th percentile) trong hệ thống phân tán. Do quyền truy cập trạng thái đóng một vai trò quan trọng trong hoạt động dịch vụ nên hệ thống lưu trữ phải có khả năng đáp ứng các SLA nghiêm ngặt như vậy (xem phần 2.2 ở dưới). Các dịch vụ phải có khả năng định cấu hình Dynamo sao cho chúng luôn đạt được các yêu cầu về độ trễ (latency) và thông lượng (throughput). Đánh đổi nằm ở hiệu suất, hiệu quả chi phí, tính khả dụng và đảm bảo tính bền vững.

*Các giả định khác*: Dynamo chỉ được sử dụng bởi các dịch vụ nội bộ của Amazon. Môi trường hoạt động của nó được coi là không thù địch và không có các yêu cầu liên quan đến bảo mật như xác thực và ủy quyền. Hơn nữa, vì mỗi dịch vụ sử dụng phiên bản Dynamo riêng biệt nên thiết kế ban đầu của nó hướng tới quy mô lên đến hàng trăm máy chủ lưu trữ. Chúng ta sẽ thảo luận về các hạn chế về khả năng mở rộng của Dynamo và các tiện ích mở rộng có thể có liên quan đến khả năng mở rộng trong các phần sau.

### 2.2. Thỏa thuận mức dịch vụ (Service Level Agreements - SLA)

Để đảm bảo rằng ứng dụng có thể cung cấp các tính năng trong thời gian cho phép, mỗi và mọi thành phần của nền tảng cần phải cung cấp các tính năng của chúng trong thời gian thậm chí còn ngắn hơn. Khách hàng và dịch vụ sẽ có một thỏa thuận mức dịch vụ (Service Level Agreement - SLA). Đó là một hợp đồng thương lượng chính thức giữa khách hàng và dịch vụ về một số đăc điểm liên quan đến hệ thống, trong đó nổi bật nhất là phân bổ tỉ lệ request dự kiến của khách hàng cho một API cụ thể và độ trễ dịch vụ dự kiến dưới những điều kiện đó. Một ví dụ về SLA đơn giản là một dịch vụ đảm bảo rằng nó sẽ trả về response trong vòng 300ms với 99.9% request khi tải lớn nhất là 500 request trên giây.

<figure markdown>
![Hình 1: Kiến trúc hướng dịch vụ của nền tảng Amazon](../../assets/misc/papers/amazon-dynamo/figure1.png){:class="centered-img"}
<figcaption>Hình 1: Kiến trúc hướng dịch vụ của nền tảng Amazon</figcaption>
</figure>

Trên hạ tầng hướng dịch vụ phi tập trung của Amazon, các SLA đóng vai trò rất quan trong. Ví dụ, một request đến một trong các site thương mại điện tử thường cần công cụ kết xuất trang phải tạo ra response bằng cách gửi các request nhỏ đến hơn 150 dịch vụ. Các dịch vụ này thường phụ thuộc vào một số thành phần khác, thường là các dịch vụ khác nữa, và cũng không có gì lạ khi phải gọi request qua nhiều dịch vụ khác nhau. Để đảm bảo rằng công cụ kết xuất trang có thể duy trì một giới hạn phân phối trang rõ ràng, mỗi dịch vụ trong chuỗi các dịch vụ này phải tuân theo hợp đồng hiệu suất của nó.

Hình 1 ở trên cho ta một cái nhìn toàn cảnh về kiến trúc của nền tảng Amazon, nơi nội dung web động được tạo ra bằng các thành phần kết xuất, các thành phần này lại gọi nhiều dịch vụ khác nhau nữa. Một dịch vụ có thể dùng nhiều kho lưu trữ khác nhau để quản lý trạng thái của chính nó, và các kho dữ liệu này chỉ có thể được truy cập trong ranh giới dịch vụ của dịch vụ đó. Một số dịch vụ hoạt động như một bộ tổng hợp bằng cách sử dụng một số dịch vụ khác để tạo ra phản hồi tổng hợp. Thông thường, các dịch vụ tổng hợp (aggregator service) đi theo hướng stateless, mặc dù chúng dùng caching rất nhiều.

Một phương pháp phổ biến để tạo một SLA hướng hiệu suất là mô tả nó với các biến thể trung bình (average), trung vị (median) và kì vọng (expected). Ở Amazon, chúng tôi nhận thấy rằng các chỉ số này không phản ánh đầy đủ trong trường hợp mục tiêu là tạo ra một hệ thống nơi **tất cả** các khách hàng đều có trải nghiệm tốt, hơn là chỉ đa số. Ví dụ, nếu sử dụng các kỹ thuật cá nhân hoá rộng rãi thì các khách hàng lâu dài sẽ yêu cầu xử lý nhiều hơn, điều này sẽ ảnh hưởng đến hiệu suất ở phân khúc cao cấp. Một SLA dưới dạng thời gian response trung bình hoặc trung vị sẽ không giải quyết được vấn đề hiệu suất của phân khúc khách hàng quan trọng này. Để giải quyết vấn đề này, các SLA ở Amazon được thể hiện và đo lường ở phân vị thứ 99.9 của mức phân phối. Lựa chọn 99.9% so với tỉ lệ phần trăm thậm chí còn cao hơn đã được thực hiện dựa trên phân tích chi phí - lợi ích cho thấy rằng chi phí đã tăng đáng kể để cải thiện hiệu suất đến mức đó. Kinh nghiệm với các hệ thống live của Amazon đã chỉ ra rằng cách tiếp cận này mang lại trải nghiệm tổng thể tốt hơn so với những hệ thống đáp ứng SLA được xác định dựa trên các chỉ số trung bình.

Trong bài báo này có nhiều tài liệu tham khảo về phân vị phân phối thứ 99.9 này, điều này phản ánh sự tập trung không ngừng nghỉ của các kỹ sư Amazon vào hiệu suất từ góc độ trải nghiệm của khách hàng. Nhiều bài báo khác báo cáo mức trung bình, nên chúng được đưa vào khi có ý nghĩa cho việc so sánh. Tuy nhiên, nỗ lực tối ưu hoá và kỹ thuật của Amazon không tập trung vào các giá trị trung bình. Một số kỹ thuật, chẳng hạn như lựa chọn cân bằng tải của các write coordinator, hoàn toàn nhằm mục đích kiểm soát hiệu suất ở phân vị thứ 99.9.

Các hệ thống lưu trữ thường đóng vai trò quan trọng trong việc thiết lập SLA của dịch vụ, đặc biệt nếu logic nghiệp vụ tương đối nhẹ, như trường hợp của nhiều dịch vụ của Amazon. Quản lý trạng thái sau đó trở thành thành phần chính của SLA của dịch vụ. Một trong những thứ cần cân nhắc trong thiết kế chính của Dynamo là việc cung cấp cho các dịch vụ quyền kiểm soát các thuộc tính hệ thống của chúng, như tính bền vững hoặc tính nhất quán, đồng thời cho phép các dịch vụ tự cân bằng giữa chức năng, hiệu suất và hiệu quả chi phí.

### 2.3. Các cân nhắc về thiết kế

Các thuật toán sao chép dữ liệu được sử dụng trong các hệ thống thương mại thường thực hiện sao chép đồng bộ (synchronous replica coordination) để cung cấp giao diện truy cập dữ liệu nhất quán mạnh (strong consistency). Để đạt được mức độ nhất quán này, các thuật toán như vậy buộc phải đánh đổi tính khả dụng của dữ liệu trong các tính huống lỗi nhất định. Ví dụ, thay vì giải quyết sự không chắc chắn về tính chính xác của response, dữ liệu sẽ từ chối trả về cho đến khi nó chắc chắn đúng. Từ buổi bình minh của việc sao chép dữ liệu, người ta biết rằng khi xử lý khả năng xảy ra lỗi mạng, không thể đạt được đồng thời tính nhất quán mạnh và tính khả dụng dữ liệu cao [2, 11], vì các hệ thống và ứng dụng như vậy cần phải biết rằng các đặc tính nào có thể đạt được và đạt được trong các điều kiện nào.

Với các hệ thống dễ xảy ra lỗi server hoặc mạng, có thể tăng tính khả dụng bằng cách sử dụng các kỹ thuật sao chép lạc quan (optimistic replication technique), trong đó các thay đổi được phép truyền tới các bản sao ở background và những việc đồng thời, không liên quan đến nhau được chấp nhận. Thách thức ở đây là nó có thể dẫ tới những thay đổi mâu thuẫn với nhau, chúng cần phải được phát hiện và giải quyết. Quá trình giải quyết xung đột dữ liệu có hai vấn đề: khi nào giải quyết và ai giải quyết. Dynamo được thiết kế để trở thành một kho dữ liệu nhất quán đến cùng (eventual consistency); nghĩa là tất cả các cập nhật đến cuối cùng cũng sẽ đến những bản sao.

Một điều quan trọng cần cân nhắc trong thiết kế là quyết định thời điểm thực hiện quy trình giải quyết xung đột trong cập nhât dữ liệu, tức là liệu xung đột có nên được giải quyết trong quá trình đọc hoặc ghi dữ liệu hay không. Nhiều kho dữ liệu truyền thống giải quyết xung đột trong quá trình ghi và giữ cho việc đọc đơn giản hơn [7]. Trong các hệ thống như vậy, việc ghi có thể bị từ chối nếu kho dữ liệu không thể kết nối đến tất cả (hoặc phần lớn) các bản sao tại một thời điểm nhất định. Mặt khác, Dynamo nhắm đến không gian thiết kế của một kho dữ liệu "luôn ghi được" (tức là một cái kho dữ liệu mà việc ghi dữ liệu không bao giờ bị từ chối). Đối với một số dịch vụ của Amazon, việc từ chối việc ghi dữ liệu của khách có thể dẫn đến trải nghiệm khách hàng kém. Ví dụ, dịch vụ giỏ hàng phải cho phép khách hàng thêm và xóa các mặt hàng khỏi giỏ hàng của họ ngay cả khi mạng và máy chủ bị lỗi, yêu cầu này buộc chúng tôi phải đẩy sự phức tạp của việc giải quyết xung đột sang các lần đọc để đảm bảo rằng việc ghi không bao giờ bị từ chối.

Lựa chọn thiết kế tiếp theo là ai sẽ thực hiện giải quyết xung đột dữ liệu. Điều này có thể được thực hiện bởi chính ứng dụng hoặc kho dữ liệu. Nếu việc giải quyết xung đột được thực hiện bởi kho dữ liệu thì các lựa chọn của nó sẽ khá hạn chế. Trong các trường hợp như vậy, kho dữ liệu chỉ có thể dùng một số cách đơn giản, như "lần ghi cuối thắng" (last write wins) [22] để giải quyết xung đột cập nhật. Mặt khác, vì ứng dụng nhận thức được lược đồ dữ liệu nên nó có thể quyết định phương pháp giải quyết xung đột phù hợp nhất với trải nghiệm của khách hàng. Ví dụ, ứng dụng quản lý giỏ hàng của khách hàng có thể chọn "hợp nhất" các phiên bản xung đột và trả về một giỏ hàng thống nhất. Bất chấp tính linh hoạt này, một số nhà phát triển ứng dụng có thể không muốn viết cơ chế giải quyết xung đột của riêng họ và chọn đẩy nó xuống kho dữ liệu, do đó, kho dữ liệu sẽ chọn một cách đơn giản như "lần ghi cuối thắng".

Một số nguyên tắc khác được áp dụng trong thiết kế là:

- *Khả năng mở rộng tăng dần (Incremental scalability)*: Dynamo nên có khả năng mở rộng quy mô một máy chủ lưu trữ (node) tại một thời điểm, với tác động tối thiểu đến người vận hành hệ thống và chính hệ thống.
- *Tính đối xứng (Symmetry)*: Mọi node trong Dynamo phải có cùng vai trò và chịu trách nhiệm tương tự nhau. Không có node nào có vai trò đặc biệt trong việc xử lý các request của khách hàng. Theo kinh nghiệm của chúng tôi, tính đối xứng giúp đơn giản hóa quá trình vận hành và bảo trì hệ thống.
- *Tính phi tập trung (Decentralization)*: Mở rộng của tính đối xứng, thiết kế nên ưu tiên các kỹ thuật ngang hàng phi tập trung hơn là kiểm soát tập trung. Trước đây, việc kiểm soát tập trung đã dẫn đến tình trạng ngừng hoạt động dịch vụ và mục tiêu là tránh nó xảy ra. Điều này dẫn đến một hệ thống đơn giản hơn, có khả năng mở rộng hơn và khả dụng hơn.
- *Tính không đồng nhất (Heterogeneity)*: Hệ thống cần có khả năng khai thác tính không đồng nhất trong cơ sở hạ tầng mà nó chạy trên đó. Ví dụ, việc phân bổ công việc phải tỷ lệ thuận với khả năng của từng máy chủ. Điều này rất cần thiết trong việc thêm các node mới có sức mạnh tốt hơn mà không cần phải nâng cấp tất cả các máy chủ cùng một lúc.

## 3. Các công việc liên quan

### 3.1. Các hệ thống ngang hàng (Peer-to-peer systems)

Có một số hệ thống ngang hàng (P2P) đã xem xét vấn đề lưu trữ và phân phối dữ liệu. Thế hệ hệ thống P2P đầu tiên, như [Freenet](http://freenetproject.org/) và [Gnutella](http://www.gnutella.org/), chủ yếu được sử dụng làm hệ thống chia sẻ file. Đây là những ví dụ về mạng P2P không có cấu trúc trong đó các liên kết lớp phủ giữa các mạng ngang hàng được thiết lập tùy ý. Trong các mạng này, một truy vấn tìm kiếm thường đi khắp mạng để tìm càng nhiều mạng ngang hàng chia sẻ dữ liệu càng tốt. Các hệ thống P2P đã phát triển sang thế hệ tiếp theo, trở thành cái được gọi là mạng P2P có cấu trúc. Các mạng này sử dụng giao thức nhất quán toàn cầu để đảm bảo rằng bất kỳ node nào cũng có thể định tuyến truy vấn tìm kiếm đến một số thiết bị ngang hàng có dữ liệu mong muốn một cách hiệu quả. Các hệ thống như Pastry [16] và Chord [20] sử dụng cơ chế định tuyến để đảm bảo rằng các truy vấn có thể được trả lời trong một số bước nhảy giới hạn. Để giảm độ trễ bổ sung do định tuyến nhiều bước tạo ra, một số hệ thống P2P (ví dụ như [14]) sử dụng định tuyến O(1) trong đó mỗi thiết bị ngang hàng duy trì đủ thông tin định tuyến cục bộ để nó có thể định tuyến các yêu cầu (để truy cập một dữ liệu nào đó) tới thiết bị ngang hàng thích hợp trong một số ít bước nhảy.

Nhiều hệ thống lưu trữ như Oceanstore [9] và PAST [17] được xây dựng dựa trên các lớp phủ định tuyến này. Oceanstore cung cấp dịch vụ lưu trữ liên tục, mang tính giao dịch, toàn cầu, hỗ trợ cập nhật tuần tự trên dữ liệu được sao chép rộng rãi. Để cho phép cập nhật đồng thời trong khi tránh được nhiều vấn đề cố hữu với khóa diện rộng, nó sử dụng mô hình cập nhật dựa trên giải quyết xung đột. Việc giải quyết xung đột đã được nói đến trong [21] để giảm số lượng giao dịch bị hủy bỏ. Oceanstore giải quyết xung đột bằng cách xử lý một loạt các bản cập nhật, chọn tổng thứ tự (total order) trong số chúng và sau đó áp dụng chúng một cách nguyên tử (atomically) theo thứ tự đó. Nó được xây dựng cho môi trường nơi dữ liệu được sao chép trên cơ sở hạ tầng không đáng tin cậy. Để so sánh, PAST cung cấp một lớp trừu tượng đơn giản trên Pastry cho các đối tượng bền vững và không thể thay đổi. Nó giả định rằng ứng dụng có thể xây dựng ngữ nghĩa lưu trữ cần thiết (chẳng hạn như các tệp có thể thay đổi) trên đó.

### 3.2. Các cơ sở dữ liệu và hệ thống file phân tán

Phân phối dữ liệu về hiệu suất, tính khả dụng và độ bền đã được nghiên cứu rộng rãi trong cộng đồng hệ thống file và hệ thống cơ sở dữ liệu. So với các hệ thống lưu trữ P2P chỉ hỗ trợ các namespace phẳng, các hệ thống file phân tán thường hỗ trợ các namespace phân cấp. Các hệ thống như Ficus [15] và Coda [19] sao chép các file để có tính khả dụng cao nhưng phải đánh đổi bằng tính nhất quán. Xung đột cập nhật thường được quản lý bằng các hàm giải quyết xung đột chuyên biệt. Hệ thống Farsite [1] là một hệ thống file phân tán không sử dụng bất kỳ máy chủ tập trung nào như NFS (Network File System). Farsite đạt được tính khả dụng cao và khả năng mở rộng bằng cách sao chép. Google File System (GFS) [6] là một hệ thống file phân tán khác được xây dựng để lưu trữ trạng thái các ứng dụng nội bộ của Google. GFS sử dụng thiết kế đơn giản với một server chính duy nhất để lưu trữ toàn bộ metadata và nơi dữ liệu được chia thành các chunk (phần) và được lưu trữ trong các chunkserver. Bayou là một hệ thống cơ sở dữ liệu quan hệ phân tán có các thao tác khi bị ngắt kết nối và cung cấp tính nhất quán đến cùng của dữ liệu [21].

Trong số các hệ thống này, Bayou, Coda và Ficus có các thao tác khi bị ngắt kết nối và có khả năng phục hồi trước các sự cố như phân vùng mạng và mất điện. Các hệ thống này khác nhau về thủ tục giải quyết xung đột. Chẳng hạn, Coda và Ficus thực hiện giải quyết xung đột ở cấp hệ thống và Bayou cho phép giải quyết ở cấp ứng dụng. Tuy nhiên, tất cả chúng đều đảm bảo tính nhất quán đến cùng. Tương tự như các hệ thống này, Dynamo cho phép tiếp tục đọc và ghi ngay cả khi phân vùng mạng và giải quyết các xung đột bằng các cơ chế giải quyết xung đột khác nhau. Các hệ thống lưu trữ khối phân tán như FAB [18] chia các đối tượng có kích thước lớn thành các khối nhỏ hơn và lưu trữ từng khối theo cách có tính khả dụng cao. So với các hệ thống này, kho lưu trữ key-value phù hợp hơn trong trường hợp này vì: (a) nó nhằm mục đích lưu trữ các đối tượng tương đối nhỏ (kích thước < 1MB) và (b) kho lưu trữ key-value dễ dàng định cấu hình hơn trên mỗi hệ thống. Antiquity là một hệ thống lưu trữ phân tán diện rộng được thiết kế để xử lý nhiều lỗi server [23]. Nó sử dụng secure log để bảo toàn tính toàn vẹn của dữ liệu, sao chép từng log trên nhiều server để đảm bảo độ bền và sử dụng các giao thức chịu lỗi Byzantine để đảm bảo tính nhất quán của dữ liệu. Ngược lại với Antiquity, Dynamo không tập trung vào vấn đề toàn vẹn và bảo mật dữ liệu mà được xây dựng cho một môi trường đáng tin cậy. Bigtable là một hệ thống lưu trữ phân tán để quản lý dữ liệu có cấu trúc. Nó duy trì một bản đồ thưa, được sắp xếp, đa chiều và cho phép các ứng dụng truy cập dữ liệu của chúng bằng nhiều thuộc tính [2]. So với Bigtable, Dynamo nhắm đến các ứng dụng chỉ yêu cầu quyền truy cập key-value với trọng tâm chính là tính khả dụng cao, nơi các bản cập nhật không bị từ chối ngay cả khi có phân vùng mạng hoặc lỗi server.

<figure markdown>
![Hình 2: Phân vùng và sao chép khoá trên vòng Dynamo](../../assets/misc/papers/amazon-dynamo/figure2.png){:class="centered-img"}
<figcaption>Hình 2: Phân vùng và sao chép khoá trên vòng Dynamo</figcaption>
</figure>

Các hệ thống cơ sở dữ liệu quan hệ truyền thống sử dụng sao chép thì tập trung vào vấn đề đảm bảo tính nhất quán mạnh cho dữ liệu được sao chép. Mặc dù tính nhất quán mạnh cung cấp cho người viết ứng dụng một mô hình lập trình thuận tiện, nhưng các hệ thống này bị hạn chế về khả năng mở rộng và tính khả dụng [7]. Các hệ thống này không có khả năng xử lý các phân vùng mạng vì chúng thường cung cấp sự đảm bảo tính nhất quán mạnh.

### 3.3. Thảo luận

Dynamo khác với các hệ thống lưu trữ phi tập trung nói trên về các yêu cầu của nó. Đầu tiên, Dynamo chủ yếu nhắm mục tiêu vào các ứng dụng cần kho lưu trữ dữ liệu “luôn ghi được”, nơi không có bản cập nhật nào bị từ chối do lỗi hoặc ghi đồng thời. Đây là một yêu cầu quan trọng đối với nhiều ứng dụng của Amazon. Thứ hai, như đã nhắc trước đó, Dynamo được xây dựng cho cơ sở hạ tầng trong một miền quản trị duy nhất, nơi tất cả các node được coi là đáng tin cậy. Thứ ba, các ứng dụng sử dụng Dynamo không yêu cầu hỗ trợ namespace phân cấp (chuẩn mực trong nhiều hệ thống file) hoặc lược đồ quan hệ phức tạp (được cơ sở dữ liệu truyền thống hỗ trợ). Thứ tư, Dynamo được xây dựng cho các ứng dụng nhạy cảm với độ trễ, yêu cầu thực hiện ít nhất 99,9% thao tác đọc và ghi trong vòng vài trăm mili giây. Để đáp ứng các yêu cầu nghiêm ngặt về độ trễ này, chúng tôi bắt buộc phải tránh định tuyến các request qua nhiều node (đây là thiết kế điển hình được một số hệ thống hash table phân tán như Chord và Pastry áp dụng). Điều này là do định tuyến nhiều bước làm tăng sự thay đổi về thời gian response, do đó làm tăng độ trễ ở phân vị cao hơn. Dynamo có thể được mô tả như một DHT (Distributed hash table - bảng băm phân tán) zero-hop, trong đó mỗi node duy trì đủ thông tin định tuyến cục bộ để định tuyến trực tiếp request đến node thích hợp.

## 4. Kiến trúc hệ thống

### 4.1. Giao diện hệ thống

### 4.2. Thuật toán phân vùng

### 4.3. Sao chép (Replication)

### 4.4. Phiên bản hóa dữ liệu (Data Versioning)

### 4.5. Thực thi các thao tác get() và put()

## 7. Kết luận

Bài báo này mô tả Dynamo, một kho lưu trữ có tính khả dụng và tính mở rộng cao, được dùng để lưu trạng thái của nhiều dịch vụ cốt lõi của nền tảng thương mại điện tử Amazon.com. Dynamo cung cấp một mức độ khả dụng và hiệu suất mong muốn và đã thành công trong việc xử lý lỗi của các server, các trung tâm dữ liệu và phân vùng mạng. Dynamo có thể mở rộng quy mô và cho phép người quản trị dịch vụ mở rộng hay thu hẹp quy mô dựa trên nhu cầu. Dynamo cho phép người quản trị tùy chỉnh hệ thống lưu trữ đáp ứng các yêu cầu về hiệu suất, tính bền vững và nhất quán thông qua SLA bằng cách cho phép chúng điều chỉnh các tham số N, R và W.

Việc sử dụng Dynamo trong thực tế những năm qua chứng tỏ rằng các kỹ thuật phi tập trung có thể được kết hợp để tạo ra một hệ thống duy nhất có tính khả dụng cao. Thành công của nó trong một trong những môi trường ứng dụng khắc nghiệt nhất cho thấy rằng một hệ thống lưu trữ eventual-consistent có thể  là một phần quan trọng cho các ứng dụng có tính khả dụng cao.

## Cảm ơn và tham khảo

Xin mời bạn tham khảo link nguồn ở phần đầu bài viết.
