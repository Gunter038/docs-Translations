- - -
sidebar_label : Lớp Data Availability của Celestia
- - -

# Vòng đời một giao dịch của ứng dụng Celestia

Người dùng yêu cầu Ứng dụng Celestia cung cấp dữ liệu bằng cách gửi ` PayForData ` giao dịch. Mỗi giao dịch như vậy bao gồm danh tính của người gửi, dữ liệu được cung cấp, cũng được gọi là thông báo, kích thước dữ liệu, không gian tên ID và một chữ ký. Mỗi người tạo khối tổng hợp nhiều giao dịch `PayForData` vào một khối.

Trước khi đề xuất khối, người tạo khối chuyển nó sang máy trạng thái thông qua ABCI++, nơi mỗi giao dịch `PayForData`được chia thành một message được namespace ( được biểu thị bằng `Msg` trong hình bên dưới), ví dụ, data cùng với namespace ID, và giao dịch có thể thực thi ( được biểu thị bằng `e-Tx` trong hình bên dưới) và không chứa dữ liệu, nhưng chỉ một cam kết có thể được sử dụng sau này để chứng minh rằng data đã thực sự được khả dụng.

Do đó, dữ liệu khối bao gồm dữ liệu được phân chia thành các namespaces và các giao dịch có thể thực thi. Lưu ý rằng chỉ những giao dịch này mới được thực thi bởi máy trạng thái Celestia sau khi khối được cam kết.

![Vòng đời một giao dịch của Celestia App](/img/concepts/tx-lifecycle.png)

Tiếp theo, người tạo khối thêm vào tiêu đề khối một cam kết về dữ liệu khối. Như đã được mô tả [tại đây](./data-availability-layer.md#fraud-proofs-of-incorrectly-extended-data), cam kết là gốc Merkle của rễ Merkle trung gian 4k (tức là một cho mỗi hàng và cột của ma trận mở rộng). Để tính toán cam kết này, nhà sản xuất khối thực hiện các hoạt động sau:

- Nó phân chia các giao dịch thực thi và dữ liệu được namespace thành các phần. Mỗi phần bao gồm một vài bytes với tiền tố là một namespace ID. Để đạt được điều này, các giao dịch có thể thực thi được liên kết với một namespace riêng.
- It arranges these shares into a square matrix (row-wise). Note that the shares are padded to the next power of two. The outcome square of size k × k is referred to as the original data.
- It extends the original data to a 2k × 2k square matrix using the 2-dimensional Reed-Solomon encoding scheme described above. The extended shares (i.e., containing erasure data) are associated with another reserved namespace.
- It computes a commitment for every row and column of the extended matrix using the NMTs described above.

Thus, the commitment of the block data is the root of a Merkle tree with the leaves the roots of a forest of Namespaced Merkle subtrees, one for every row and column of the extended matrix.

## Kiểm tra tính khả dụng của dữ liệu

![Mạng lưới DA](/img/concepts/consensus-da.png)

To enhance connectivity, the Celestia Node augments the Celestia App with a separate libp2p network, i.e., the so-called _DA network_, that serves DAS requests.

Light nodes connect to a Celestia Node in the DA network, listen to extended block headers (i.e., the block headers together with the relevant DA metadata, such as the 4k intermediate Merkle roots), and perform DAS on the received headers (i.e., ask for random data chunks).

Note that although it is recommended, performing DAS is optional -- light nodes could just trust that the data corresponding to the commitments in the block headers was indeed made available by the Celestia DA layer. In addition, light nodes can also submit transactions to the Celestia App, i.e., `PayForData` transactions.

While performing DAS for a block header, every light node queries Celestia Nodes for a number of random data chunks from the extended matrix and the corresponding Merkle proofs. If all the queries are successful, then the light node accepts the block header as valid (from a DA perspective).

If at least one of the queries fails (i.e., either the data chunk is not received or the Merkle proof is invalid), then the light node rejects the block header and tries again later. The retrial is necessary to deal with false negatives, i.e., block headers being rejected although the block data is available. This may happen due to network congestion for example.

Alternatively, light nodes may accept a block header although the data is not available, i.e., a _false positive_. This is possible since the soundness property (i.e., if an honest light node accepts a block as available, then at least one honest full node will eventually have the entire block data) is probabilistically guaranteed (for more details, take a look at the [original paper](https://arxiv.org/abs/1809.09044)).

By fine tuning Celestia's parameters (e.g., the number of data chunks sampled by each light node) the likelihood of false positives can be sufficiently reduced such that block producers have no incentive to withhold the block data.
