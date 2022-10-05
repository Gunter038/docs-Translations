- - -
Lớp dữ liệu sẵn sàng của Celestia
- - -

# Lớp dữ liệu sẵn sàng của Celestia

Lớp dữ liệu sẵn sàng của Celestia cung cấp giải pháp mở rộng cho [ vấn đề dữ liệu có sẵn ](https://coinmarketcap.com/alexandria/article/what-is-data-availability). Do tính chất của các blockchain không cần được cấp phép, lớp dữ liệu sẵn sàng phải cung cấp một cơ chế cho lớp thực hiện và lớp giải quyết để kiểm tra độ tin cậy xem dữ liệu giao dịch có thực sự có sẵn hay không.

Hai tính năng chính của lớp dữ liệu sẵn sàng của Celestia [ mẫu thử sẵn sàng ](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) và [ không gian tên cây Merkle  ](https://github.com/celestiaorg/nmt) (NMTs). Cả hai tính năng đều là các giải pháp mở rộng quy mô blockchain mới: DAS cho phép các light node xác minh tính khả dụng của dữ liệu mà không cần tải xuống toàn bộ khối;NMT cho phép các lớp thực thi và thanh toán trên Celestia để tải xuống các giao dịch chỉ có liên quan đến chúng.

## Lấy mẫu của dữ liệu sẵn sàng(DAS)

Nói chung, các light node chỉ tải xuống các tiêu đề của khối có chứa các cam kết (tức là gốc của Merkle) của khối dữ liệu (tức là danh sách các giao dịch).

Để làm cho DAS khả thi, Celestia sử dụng Reed-Solomon 2 chiều lược đồ mã hóa để mã hóa dữ liệu khối: mọi dữ liệu khối đều được chia nhỏ thành k × k khối, được sắp xếp trong ma trận k × k và mở rộng với tính chẵn lẻ dữ liệu vào ma trận mở rộng 2k × 2k bằng cách áp dụng nhiều lần mã hóa Reed-Solomon.

Sau đó, 4k gốc Merkle riêng biệt được tính cho các hàng và cột của ma trận mở rộng; gốc Merkle của những gốc Merkle này được sử dụng như cam kết dữ liệu khối trong tiêu đề của khối.

![Mã hóa 2D Reed-Soloman (RS)](/img/concepts/reed-solomon-encoding.png)

Để xác minh rằng dữ liệu có sẵn, các light node của Celestia đang lấy mẫu các khối dữ liệu 2k × 2k.

Mỗi light node chọn ngẫu nhiên một tập hợp các tọa độ duy nhất trong ma trận mở rộng và truy vấn các full node cho các khối dữ liệu và các bằng chứng Merkle tương ứng tại các tọa độ đó. Nếu các light node nhận được phản hồi hợp lệ cho mỗi truy vấn lấy mẫu, sau đó sẽ có [xác suất cao đảm bảo ](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075) rằng dữ liệu của toàn khối có sẵn.

Ngoài ra, mọi đoạn dữ liệu nhận được đều có bằng chứng Merkle chính xác được bàn tán trên mạng. Kết quả là, miễn là các light node của Celestia lấy mẫu cùng nhau đủ các khối dữ liệu (tức là ít nhất k × k khối duy nhất), toàn bộ khối có thể được phục hồi bởi tính trung thực của full node.

Để biết thêm chi tiết về DAS, hãy xem [ tài liệu gốc ](https://arxiv.org/abs/1809.09044).

### Khả năng mở rộng

DAS cho phép Celestia mở rộng quy mô lớp DA. DAS có thể được thực hiện bởi giới hạn tài nguyên của light node vì mỗi nút sáng chỉ lấy mẫu một phần dữ liệu khối. Càng có nhiều light node trong mạng, càng nhiều dữ liệu họ có thể tải xuống và lưu trữ chung.

Điều này có nghĩa là việc tăng số lượng light node thực hiện DAS cho phép cho các khối lớn hơn (tức là có nhiều giao dịch hơn), trong khi vẫn giữ DAS khả thi cho các light node bị giới hạn tài nguyên. Tuy nhiên, để xác thực tiêu đề khối, các light node của  Celestia cần tải xuống trung gian 4k gốc Merkle.

Đối với kích thước dữ liệu khối là n byte, điều này có nghĩa là mọi light node phải tải xuống O (n) byte. Do đó, bất kỳ sự cải thiện nào về dung lượng băng thông của  Celestia light node có ảnh hưởng bậc hai đến thông lượng của Celestia Lớp DA.

### Bằng chứng gian lận về dữ liệu mở rộng không chính xác

Yêu cầu tải xuống các gốc Merkle trung gian 4k là một hệ quả của việc sử dụng lược đồ mã hóa Reed-Solomon 2 chiều. Ngoài ra, DAS có thể được thiết kế với mã hóa Reed-Solomon chuẩn (tức là 1 chiều), trong đó dữ liệu ban đầu được chia thành k khối và mở rộng với k phần bổ sung khối dữ liệu chẵn lẻ. Vì cam kết dữ liệu khối là gốc Merkle của 2k khối dữ liệu, các nút sáng không còn cần tải O (n) byte xuống xác thực tiêu đề khối.

Nhược điểm của mã hóa Reed-Solomon tiêu chuẩn là đối phó với mã độc chặn các nhà sản xuất tạo dữ liệu mở rộng không chính xác.

Điều này có thể xảy ra vì __ Celestia không yêu cầu đa số đồng thuận (tức là nhà sản xuất khối) trung thực để đảm bảo tính sẵn sàng của dữ liệu. __ Do đó, nếu dữ liệu mở rộng không hợp lệ, thì dữ liệu gốc có thể không có thể phục hồi, ngay cả khi các light node đang lấy mẫu đủ các khối duy nhất (nghĩa là, ít nhất k đối với mã hóa chuẩn và k × k đối với mã hóa 2 chiều).

Như một giải pháp, _ Bằng chứng gian lận về dữ liệu mở rộng được tạo không chính xác _ bật các light node từ chối các khối có dữ liệu mở rộng không hợp lệ. Những bằng chứng như vậy yêu cầu tạo lại mã hóa và xác minh sự không khớp. Với Reed-Solomon tiêu chuẩn mã hóa, điều này đòi hỏi phải tải xuống dữ liệu gốc, tức là, O (n) byte. Ngược lại, với mã hóa Reed-Solomon 2 chiều, chỉ có các byte O (n) là bắt buộc vì chỉ cần xác minh một hàng hoặc một cột của ma trận mở rộng.

## Không gian tên cây Merkle (NMTs)

Celestia phân vùng dữ liệu khối thành nhiều không gian tên, một không gian tên cho mọi ứng dụng (ví dụ: cuộn lại) bằng cách sử dụng lớp DA. Kết quả là, mọi ứng dụng chỉ cần tải xuống dữ liệu của chính nó và có thể bỏ qua dữ liệu của các ứng dụng khác.

Để điều này hoạt động, lớp DA phải có khả năng chứng minh rằng dữ liệu hoàn tất, tức là tất cả dữ liệu cho một không gian tên nhất định sẽ được trả về. Để đạt được mục tiêu này, Celestia đang sử dụng không gian tên Merkle Trees (NMTs).

NMT là một cây Merkle với các lá được sắp xếp theo số nhận dạng không gian tên và hàm băm được sửa đổi để mọi nút trong cây bao gồm phạm vi không gian tên của tất cả các con cháu của nó. Hình sau cho thấy một ví dụ về NMT có chiều cao ba (tức là tám khối dữ liệu). Dữ liệu là được phân chia thành ba không gian tên.

![Không gian tên Cây Merkle](/img/concepts/nmt.png)

Khi một ứng dụng yêu cầu dữ liệu cho không gian tên 2, lớp DA phải cung cấp các khối dữ liệu `D3`, `D4`, `D5`, and `D6` và các nút `N2`, `N8` và  `N7`  làm bằng chứng (lưu ý rằng ứng dụng đã có gốc `N14`  từ tiêu đề của khối).

Do đó, ứng dụng có thể kiểm tra xem dữ liệu được cung cấp có phải là một phần của dữ liệu khối. Hơn nữa, ứng dụng có thể xác minh rằng tất cả dữ liệu cho không gian tên 2 đã được cung cấp. Nếu lớp DA chỉ cung cấp ví dụ các khối dữ liệu ` D4 ` và ` D5 `, nó cũng phải cung cấp các node` N12 ` và ` N11 ` làm bằng chứng. Tuy nhiên, ứng dụng có thể xác định rằng dữ liệu không đầy đủ bằng cách kiểm tra phạm vi không gian tên của hai nút, tức là cả ` N12 ` và ` N11 ` đều có con cháu một phần của không gian tên 2.

Để biết thêm chi tiết về NMT, hãy xem [ tài liệu gốc ](https://arxiv.org/abs/1905.09274).

## Xây dựng Blockchain PoS cho DA

### Cung cấp dữ liệu có sẵn

Lớp Celestia DA bao gồm một blockchain PoS. Celestia đang định đặt tên cho blockchain là [ Celestia App ](https://github.com/celestiaorg/celestia-app), một ứng dụng cung cấp các giao dịch để tạo điều kiện thuận lợi cho lớp DA và được xây dựng dựa trên [ Cosmos SDK ](https://docs.cosmos.network/v0.44/). Hình sau hiển thị các thành phần chính của Ứng dụng Celestia.

![Các thành phần chính của Ứng dụng Celestia](/img/concepts/celestia-app.png)

Ứng dụng Celestia được xây dựng trên nền tảng của [ Celestia Core ](https://github.com/celestiaorg/celestia-core), một phiên bản sửa đổi của [ thuật toán đồng thuận Tendermint ](https://arxiv.org/abs/1807.04938). Trong số những thay đổi quan trọng hơn đối với Tendermint, Celestia Core:

- Cho phép bảo vệ khối dữ liệu (sử dụng Reed-Solomon 2 chiều lược đồ mã hóa).
- Thay thế cây Merkle thông thường được Tendermint sử dụng để lưu trữ dữ liệu khối với [ Không gian tên Cây Merkle  ](https://github.com/celestiaorg/nmt) cho phép các lớp trên (tức là thực thi và giải quyết) để chỉ tải xuống những dữ liệu cần thiết (để biết thêm chi tiết, xem phần bên dưới mô tả các trường hợp sử dụng).

Để biết thêm chi tiết về những thay đổi đối với Tendermint, hãy xem [ ADRs ](https://github.com/celestiaorg/celestia-core/tree/v0.34.x-celestia/docs/celestia-architecture). Lưu ý rằng các nút Celestia Core vẫn đang sử dụng mạng Tendermint p2p.

Tương tự như Tendermint, Celestia Core được kết nối với lớp ứng dụng (tức là máy trạng thái) bởi [ ABCI ++ ](https://github.com/tendermint/tendermint/tree/master/spec/abci%2B%2B), một sự phát triển lớn của [ ABCI ](https://github.com/tendermint/tendermint/tree/master/spec/abci) (Application Blockchain Interface).

Máy trạng thái Ứng dụng Celestia là cần thiết để thực thi logic PoS và cho phép quản lý lớp DA.

Tuy nhiên, Ứng dụng Celestia là dữ liệu bất khả tri - máy trạng thái cũng không xác thực cũng như lưu trữ dữ liệu được cung cấp bởi Ứng dụng Celestia.
