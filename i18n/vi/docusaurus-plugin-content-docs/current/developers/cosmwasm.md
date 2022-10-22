---
sidebar_label: "CosmWasm trên Rollmint\nCosmWasm là một nền tảng hợp đồng thông minh được xây dựng cho hệ sinh thái Cosmos bằng cách sử dụng WebAssembly (Wasm) để xây dựng các hợp đồng thông minh cho Cosmos-SDK. Trong hướng dẫn này, chúng ta sẽ khám phá cách tích hợp CosmWasm với Lớp khả dụng dữ liệu của Celestia bằng cách sử dụng Rollmint.\nLƯU Ý: Hướng dẫn này sẽ khám phá việc phát triển với Rollmint, vẫn đang trong giai đoạn Alpha. Nếu bạn gặp lỗi, vui lòng viết yêu cầu về Vấn đề trên Github hoặc cho chúng tôi biết trong Discord của chúng tôi. Hơn nữa, trong khi Rollmint cho phép bạn xây dựng các bản tổng hợp có chủ quyền trên Celestia, nó hiện chưa hỗ trợ bằng chứng gian lận và do đó đang chạy ở chế độ \"pessimistic\", nơi các nút sẽ cần thực hiện lại các giao dịch để kiểm tra tính hợp lệ của chuỗi (tức là một nút đầy đủ). Hơn nữa, Rollmint hiện chỉ hỗ trợ một trình tự sắp xếp duy nhất.\nBạn có thể tìm hiểu thêm về CosmWasm tại đây.\nTrong hướng dẫn này, chúng ta sẽ xem xét những điều sau:\nThiết lập sự phụ thuộc của bạn cho các hợp đồng thông minh CosmWasm của bạn\nThiết lập Rollmint trên CosmWasm\nKhởi tạo một mạng cục bộ cho chuỗi CosmWasm của bạn được kết nối với Celestia\nTriển khai hợp đồng thông minh Rust cho chuỗi CosmWasm\nTương tác với hợp đồng thông minh\nHợp đồng thông minh mà chúng tôi sẽ sử dụng cho hướng dẫn này là hợp đồng do nhóm CosmWasm cung cấp cho việc mua Nameservice.\nBạn có thể kiểm tra hợp đồng ở đây.\nCách viết hợp đồng thông minh Rust cho Nameservice nằm ngoài phạm vi của hướng dẫn này. Trong tương lai, chúng tôi sẽ bổ sung thêm nhiều hướng dẫn viết hợp đồng thông minh CosmWasm cho Celestia."
---

# CosmWasm on Rollmint

CosmWasm is a smart contracting platform built for the Cosmos ecosystem by making use of WebAssembly (Wasm) to build smart contracts for Cosmos-SDK. In this tutorial, we will be exploring how to integrate CosmWasm with Celestia's Data Availability Layer using Rollmint.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. If you run into bugs, please write a Github Issue ticket or let us know in our Discord. Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

You can learn more about CosmWasm [here](https://docs.cosmwasm.com/docs/1.0/).

In this tutorial, we will going over the following:

* [Setting up your dependencies for your CosmWasm smart contracts](./cosmwasm-dependency.md)
* [Setting up Rollmint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instantiate a local network for your CosmWasm chain connected to Celestia](./cosmwasm-environment.md)
* [Deploying a Rust smart contract to CosmWasm chain](./cosmwasm-contract-deployment.md)
* [Interacting with the smart contract](./cosmwasm-contract-interaction.md)

The smart contract we will use for this tutorial is one provided by the CosmWasm team for Nameservice purchasing.

You can check out the contract [here](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

How to write the Rust smart contract for Nameservice is outside the scope of this tutorial. In the future we will add more tutorials for writing CosmWasm smart contracts for Celestia.
