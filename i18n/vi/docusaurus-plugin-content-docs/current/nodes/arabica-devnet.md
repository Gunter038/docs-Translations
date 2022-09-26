---
sidebar_label: Arabica Devnet
---

# Arabica Devnet
<!-- markdownlint-disable MD013 -->

![arabica-devnet](/img/arabica-devnet.png)

Arabica Devnet là một testnet mới từ Celestia Labs được tập trung hoàn toàn vào việc cung cấp cho các nhà phát triển hiệu suất nâng cao và các bản nâng cấp mới nhất để thử nghiệm các rollup và ứng dụng của họ.

Arabica không tập trung vào validator hoặc thử nghiệm mức độ đồng thuận, thay vào đó, đó là những gì Mamaki Testnet được sử dụng. Nếu bạn là validator, chúng tôi khuyên bạn chỉ nên thử nghiệm các hoạt động validator của bạn trên Mamaki [tại đây](./mamaki-testnet.md).

Vì Arabica sẽ có những cập nhật mới nhất từ tất cả các sản phẩm của Celestia được triển khai trên nó, nó có thể có nhiều thay đổi. Do đó, như một lời cảnh báo công bằng, Arabica có thể sập bất ngờ nhưng miễn là nó được cập nhật liên tục, đó là một cách hữu ích để tiếp tục kiểm tra những thay đổi mới nhất trong phần mềm.

Các nhà phát triển vẫn có thể triển khai trên Mamaki Testnet các rollup chuyên dụng của họ nếu họ chọn làm như vậy, tuy nhiên nó sẽ luôn tụt hậu so với Arabica Devnet cho đến khi Mamaki trải qua nâng cấp hardfork với sự phối hợp với các Validator.

## Tích hợp

Hướng dẫn này gồm các phần liên quan về cách kết nối với Arabica, tùy thuộc vào loại node bạn đang chạy.

Cách tốt nhất để tham gia là trước tiên xác định bạn muốn chạy node nào. Mỗi hướng dẫn node sẽ liên kết đến mạng liên quan để hướng dẫn bạn cách kết nối với chúng.

Bạn có một danh sách các lựa chọn về loại node bạn có thể chạy để tham gia Arabica:

Data Availability:

* [Bridge Node](./bridge-node.md)
* [Full Storage Node](./full-storage-node.md)
* [Light Node](./light-node.md)

Chọn loại node bạn muốn chạy và làm theo hướng dẫn trên mỗi trang tương ứng. Bất cứ khi nào bạn được yêu cầu chọn loại mạng bạn muốn kết nối trong các hướng dẫn, hãy chọn `Arabica` để có thể tham khảo hướng dẫn chính xác trên trang này về cách kết nối với Arabica.

## RPC Endpoints

Đây là danh sách các RPC endpoints bạn có thể sử dụng để kết nối với Arabica Devnet:

* [https://limani.celestia-devops.dev](https://limani.celestia-devops.dev)

## Vòi Arabica Devnet

> SỬ DỤNG VÒI NÀY SẼ KHÔNG GIÚP BẠN NHẬN BẤT KÌ AIRDROP NÀO HOẶC CÁC ĐỢT PHÂN PHỐI TOKENS CELESTIA MAINNET. TOKENS CELESTIA MAINNET HIỆN KHÔNG TỒN TẠI VÀ HIỆN KHÔNG CÓ BẤT KÌ ĐỢT BÁN CÔNG KHAI HOẶC ĐỢT PHÂN PHỐI TOKEN CELESTIA MAINNET NÀO.

Bạn có thể yêu cầu từ Vòi Arabica Devnet trên kênh #arabica-faucet trong Máy chủ Discord của Celestia với lệnh sau:

```text
$request <CELESTIA-ADDRESS>
```

Thay `<CELESTIA-ADDRESS>` bằng địa chỉ `celestia1******` được tạo.

> Lưu ý: Vòi có giới hạn 10 tokens một tuần đối với mỗi địa chỉ ví/Discord ID

## Trình khám phá khối

Có một explorer bạn có thể dùng cho Arabica:

* [https://explorer.celestia.observer/arabica](https://explorer.celestia.observer/arabica)
