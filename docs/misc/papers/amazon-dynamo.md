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
![Hình 1: Kiến trúc hướng dịch vụ của nền tảng Amazon](../../assets/misc/papers/amazon-dynamo/figure1.png){:class="centered-img h-200"}
<figcaption>Hình 1: Kiến trúc hướng dịch vụ của nền tảng Amazon</figcaption>
</figure>

### 2.3. Các cân nhắc về thiết kế

## 3. Các công việc liên quan

### 3.1. Các hệ thống ngang hàng (Peer-to-peer systems)

### 3.2. Các cơ sở dữ liệu và hệ thống file phân tán

### 3.3. Thảo luận

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
