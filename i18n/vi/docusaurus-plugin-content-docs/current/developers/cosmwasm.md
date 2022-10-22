---
sidebar_label: "CosmWasm trên Rollmint\nCosmWasm là một nền tảng hợp đồng thông minh được xây dựng cho hệ sinh thái Cosmos bằng cách sử dụng WebAssembly (Wasm) để xây dựng các hợp đồng thông minh cho Cosmos-SDK. Trong hướng dẫn này, chúng ta sẽ khám phá cách tích hợp CosmWasm với Lớp khả dụng dữ liệu của Celestia bằng cách sử dụng Rollmint.\nLƯU Ý: Hướng dẫn này sẽ khám phá việc phát triển với Rollmint, vẫn đang trong giai đoạn Alpha. Nếu bạn gặp lỗi, vui lòng viết yêu cầu về Vấn đề trên Github hoặc cho chúng tôi biết trong Discord của chúng tôi. Hơn nữa, trong khi Rollmint cho phép bạn xây dựng các bản tổng hợp có chủ quyền trên Celestia, nó hiện chưa hỗ trợ bằng chứng gian lận và do đó đang chạy ở chế độ \"pessimistic\", nơi các nút sẽ cần thực hiện lại các giao dịch để kiểm tra tính hợp lệ của chuỗi (tức là một nút đầy đủ). Hơn nữa, Rollmint hiện chỉ hỗ trợ một trình tự sắp xếp duy nhất.\nBạn có thể tìm hiểu thêm về CosmWasm tại đây.\nTrong hướng dẫn này, chúng ta sẽ xem xét những điều sau:\nThiết lập sự phụ thuộc của bạn cho các hợp đồng thông minh CosmWasm của bạn\nThiết lập Rollmint trên CosmWasm\nKhởi tạo một mạng cục bộ cho chuỗi CosmWasm của bạn được kết nối với Celestia\nTriển khai hợp đồng thông minh Rust cho chuỗi CosmWasm\nTương tác với hợp đồng thông minh\nHợp đồng thông minh mà chúng tôi sẽ sử dụng cho hướng dẫn này là hợp đồng do nhóm CosmWasm cung cấp cho việc mua Nameservice.\nBạn có thể kiểm tra hợp đồng ở đây.\nCách viết hợp đồng thông minh Rust cho Nameservice nằm ngoài phạm vi của hướng dẫn này. Trong tương lai, chúng tôi sẽ bổ sung thêm nhiều hướng dẫn viết hợp đồng thông minh CosmWasm cho Celestia."
---

# CosmWasm trên Rollmint

CosmWasm là một nền tảng hợp đồng thông minh được xây dựng cho Cosmos hệ sinh thái bằng cách sử dụng WebAssembly (Wasm) để xây dựng các hợp đồng thông minh cho Cosmos-SDK. Trong hướng dẫn này, chúng ta sẽ khám phá cách tích hợp CosmWasm với Lớp khả dụng dữ liệu của Celestia bằng Rollmint.

> LƯU Ý: Hướng dẫn này sẽ khám phá việc phát triển với Rollmint, mà vẫn đang trong giai đoạn Alpha. Nếu bạn gặp lỗi, vui lòng viết phiếu vấn đề trên Github hoặc cho chúng tôi biết trong Discord của chúng tôi. Hơn nữa, trong khi Rollmint cho phép bạn xây dựng các bản tổng hợp có chủ quyền trên Celestia, nó hiện chưa hỗ trợ bằng chứng gian lận và do đó đang chạy ở chế độ "bi quan", nơi các node sẽ cần thực hiện lại các giao dịch để kiểm tra tính hợp lệ của chuỗi (tức là một full node). Hơn nữa, Rollmint hiện chỉ hỗ trợ một trình tự sắp xếp duy nhất.

Bạn có thể tìm hiểu thêm về CosmWasm [ tại đây ](https://docs.cosmwasm.com/docs/1.0/).

Trong hướng dẫn này, chúng ta sẽ xem xét những điều sau:

* [Thiết lập sự phụ thuộc của bạn cho các hợp đồng thông minh CosmWasm của bạn](./cosmwasm-dependency.md)
* [Thiết lập Rollmint trên CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Khởi tạo mạng cục bộ cho chuỗi CosmWasm của bạn được kết nối với Celestia](./cosmwasm-environment.md)
* [Triển khai hợp đồng thông minh Rust cho chuỗi CosmWasm](./cosmwasm-contract-deployment.md)
* [Tương tác với contract](./cosmwasm-contract-interaction.md)

Hợp đồng thông minh mà chúng tôi sẽ sử dụng cho hướng dẫn này là hợp đồng được cung cấp bởi nhóm CosmWasm để mua dịch vụ Nameservice.

Bạn có thể xem hợp đồng [ tại đây ](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

Cách viết hợp đồng thông minh Rust cho Nameservice nằm ngoài phạm vi của hướng dẫn này. Trong tương lai chúng tôi sẽ bổ sung thêm nhiều bài hướng dẫn viết CosmWasm hợp đồng thông minh cho Celestia.
