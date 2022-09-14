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

![2D Reed-Soloman (RS) Encoding](/img/concepts/reed-solomon-encoding.png)

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

## Namespaced Merkle Trees (NMTs)

Celestia partitions the block data into multiple namespaces, one for every application (e.g., rollup) using the DA layer. As a result, every application needs to download only its own data and can ignore the data of other applications.

For this to work, the DA layer must be able to prove that the provided data is complete, i.e., all the data for a given namespace is returned. To this end, Celestia is using Namespaced Merkle Trees (NMTs).

An NMT is a Merkle tree with the leafs ordered by the namespace identifiers and the hash function modified so that every node  in the tree includes the range of namespaces of all its descendants. The following figure shows an example of an NMT with height three (i.e., eight data chunks). The data is partitioned into three namespaces.

![Namespaced Merkle Tree](/img/concepts/nmt.png)

When an application requests the data for namespace 2, the DA layer must provide the data chunks `D3`, `D4`, `D5`, and `D6` and the nodes `N2`, `N8` and `N7` as proof (note that the application already has the root `N14` from the block header).

As a result, the application is able to check that the provided data is part of the block data. Furthermore, the application can verify that all the data for namespace 2 was provided. If the DA layer provides for example only the data chunks `D4` and `D5`, it must also provide nodes `N12` and `N11` as proofs. However, the application can identify that the data is incomplete by checking the namespace range of the two nodes, i.e., both `N12` and `N11` have descendants part of namespace 2.

For more details on NMTs, take a look at the [original paper](https://arxiv.org/abs/1905.09274).

## Building a PoS Blockchain for DA

### Providing Data Availability

The Celestia DA layer consists of a PoS blockchain. Celestia is dubbing this blockchain as the [Celestia App](https://github.com/celestiaorg/celestia-app), an application that provides transactions to facilitate the DA layer and is built using [Cosmos SDK](https://docs.cosmos.network/v0.44/). The following figure shows the main components of Celestia App.

![Main components of Celestia App](/img/concepts/celestia-app.png)

Celestia App is built on top of [Celestia Core](https://github.com/celestiaorg/celestia-core), a modified version of the [Tendermint consensus algorithm](https://arxiv.org/abs/1807.04938). Among the more important changes to vanilla Tendermint, Celestia Core:

- Enables the erasure coding of block data (using the 2-dimensional Reed-Solomon encoding scheme).
- Replaces the regular Merkle tree used by Tendermint to store block data with a [Namespaced Merkle tree](https://github.com/celestiaorg/nmt) that enables the above layers (i.e., execution and settlement) to only download the needed data (for more details, see the section below describing use cases).

For more details on the changes to Tendermint, take a look at the [ADRs](https://github.com/celestiaorg/celestia-core/tree/v0.34.x-celestia/docs/celestia-architecture). Notice that Celestia Core nodes are still using the Tendermint p2p network.

Similarly to Tendermint, Celestia Core is connected to the application layer (i.e., the state machine) by [ABCI++](https://github.com/tendermint/tendermint/tree/master/spec/abci%2B%2B), a major evolution of [ABCI](https://github.com/tendermint/tendermint/tree/master/spec/abci) (Application Blockchain Interface).

The Celestia App state machine is necessary to execute the PoS logic and to enable the governance of the DA layer.

However, the Celestia App is data-agnostic -- the state machine neither validates nor stores the data that is made available by the Celestia App.
