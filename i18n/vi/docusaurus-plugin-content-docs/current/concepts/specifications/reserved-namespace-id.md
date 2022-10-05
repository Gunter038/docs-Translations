# Namespace IDs riêng

Đây là bảng các namespace IDs riêng trên Mạng Celestia.

<!-- markdownlint-disable MD013 -->
| tên                                     | loại          | giá trị              | mô tả                                                                                         |
| --------------------------------------- | ------------- | -------------------- | --------------------------------------------------------------------------------------------- |
| `TRANSACTION_NAMESPACE_ID`              | `NamespaceID` | `0x0000000000000001` | Giao dịch: Yêu cầu thay đổi trạng thái.                                                       |
| `INTERMEDIATE_STATE_ROOT_NAMESPACE_ID`  | `NamespaceID` | `0x0000000000000002` | Gốc trạng thái trung gian, được cam kết sau mỗi giao dịch.                                    |
| `EVIDENCE_NAMESPACE_ID`                 | `NamespaceID` | `0x0000000000000003` | Bằng chứng: fraud proofs hoặc các loại proof khác của hành động có thể gây ra slash.          |
| `TAIL_TRANSACTION_PADDING_NAMESPACE_ID` | `NamespaceID` | `0x00000000000000FF` | Phần đệm đuôi cho các giao dịch: phần đệm sau tất cả các giao dịch nhưng trước phần messages. |
| `TAIL_PADDING_NAMESPACE_ID`             | `NamespaceID` | `0xFFFFFFFFFFFFFFFE` | Phần đệm đuôi cho messages: phần đệm sau tất cả messages để lấp đầy ô dữ liệu ban đầu.        |
| `PARITY_SHARE_NAMESPACE_ID`             | `NamespaceID` | `0xFFFFFFFFFFFFFFFF` | Chia sẻ chẵn lẻ: chia sẻ mở rộng trong ma trận dữ liệu có sẵn.                                |
<!-- markdownlint-enable MD013 -->
