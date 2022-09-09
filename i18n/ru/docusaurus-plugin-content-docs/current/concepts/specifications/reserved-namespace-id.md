# Зарезервированные идентификаторы пространства имен

Это таблица зарезервированных идентификаторов пространства имен в сети Celestia.

<!-- markdownlint-disable MD013 -->
| имя                                     | тип           | значение             | описание                                                                                   |
| --------------------------------------- | ------------- | -------------------- | ------------------------------------------------------------------------------------------ |
| `TRANSACTION_NAMESPACE_ID`              | `NamespaceID` | `0x0000000000000001` | Транзакции: запросы, которые изменяют состояние.                                           |
| `INTERMEDIATE_STATE_ROOT_NAMESPACE_ID`  | `NamespaceID` | `0x0000000000000002` | Корни промежуточного состояния, фиксируемые после каждой транзакции.                       |
| `EVIDENCE_NAMESPACE_ID`                 | `NamespaceID` | `0x0000000000000003` | Доказательства: мошеннические доказательства или другие доказательства слэш действий.      |
| `TAIL_TRANSACTION_PADDING_NAMESPACE_ID` | `NamespaceID` | `0x00000000000000FF` | Tail padding for transactions: padding after all transactions but before messages.         |
| `TAIL_PADDING_NAMESPACE_ID`             | `NamespaceID` | `0xFFFFFFFFFFFFFFFE` | Tail padding for messages: padding after all messages to fill up the original data square. |
| `PARITY_SHARE_NAMESPACE_ID`             | `NamespaceID` | `0xFFFFFFFFFFFFFFFF` | Parity shares: extended shares in the available data matrix.                               |
<!-- markdownlint-enable MD013 -->
