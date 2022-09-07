# 保留命名空间 IDs

这是 Celestia 网络上保留的命名空间 IDs 表。

<!-- markdownlint-disable MD013 -->
| 名称                                      | 类型            | 值                    | 说明                                                                                         |
| --------------------------------------- | ------------- | -------------------- | ------------------------------------------------------------------------------------------ |
| `TRANSACTION_NAMESPACE_ID`              | `NamespaceID` | `0x0000000000000001` | Transactions: requests that modify the state.                                              |
| `INTERMEDIATE_STATE_ROOT_NAMESPACE_ID`  | `NamespaceID` | `0x0000000000000002` | Intermediate state roots, committed after every transaction.                               |
| `EVIDENCE_NAMESPACE_ID`                 | `NamespaceID` | `0x0000000000000003` | Evidence: fraud proofs or other proof of slashable action.                                 |
| `TAIL_TRANSACTION_PADDING_NAMESPACE_ID` | `NamespaceID` | `0x00000000000000FF` | Tail padding for transactions: padding after all transactions but before messages.         |
| `TAIL_PADDING_NAMESPACE_ID`             | `NamespaceID` | `0xFFFFFFFFFFFFFFFE` | Tail padding for messages: padding after all messages to fill up the original data square. |
| `PARITY_SHARE_NAMESPACE_ID`             | `NamespaceID` | `0xFFFFFFFFFFFFFFFF` | Parity shares: extended shares in the available data matrix.                               |
<!-- markdownlint-enable MD013 -->
