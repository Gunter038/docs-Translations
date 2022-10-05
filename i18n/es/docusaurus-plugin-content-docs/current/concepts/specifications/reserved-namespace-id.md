# IDs Reservados de Nombres de Dominio

Esta es una tabla de IDs reservados de espacio de nombres en la Red Celestia.

<!-- markdownlint-disable MD013 -->
| Nombre                                  | Tipo                   | Valor                | Descripción                                                                                                       |
| --------------------------------------- | ---------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `TRANSACTION_NAMESPACE_ID`              | `ID Nombre de dominio` | `0x0000000000000001` | Transacciones: solicitudes que modifican el estado.                                                               |
| `INTERMEDIATE_STATE_ROOT_NAMESPACE_ID`  | `ID Nombre de dominio` | `0x0000000000000002` | Raíces de estado intermedias, comprometidas tras cada transacción.                                                |
| `EVIDENCE_NAMESPACE_ID`                 | `ID Nombre de dominio` | `0x0000000000000003` | Conocimiento: pruebas de fraude u otra prueba de acción de corte.                                                 |
| `TAIL_TRANSACTION_PADDING_NAMESPACE_ID` | `ID Nombre de dominio` | `0x00000000000000FF` | Relleno de cola para las transacciones: relleno después de todas las transacciones pero antes de los mensajes.    |
| `TAIL_PADDING_NAMESPACE_ID`             | `ID Nombre de dominio` | `0xFFFFFFFFFFFFFFFE` | Relleno de cola para mensajes: relleno después de todos los mensajes para rellenar el recuadro de datos original. |
| `PARITY_SHARE_NAMESPACE_ID`             | `ID Nombre de dominio` | `0xFFFFFFFFFFFFFFFF` | Paridad de acciones: acciones extendidas en la matriz de datos disponible.                                        |
<!-- markdownlint-enable MD013 -->
