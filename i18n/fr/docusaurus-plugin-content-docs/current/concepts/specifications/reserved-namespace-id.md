# Namespace IDs réservés

Ceci est un tableau des identifiants d'espaces de noms (namespace id) réservés sur le réseau Celestia.

<!-- markdownlint-disable MD013 -->
| nom                                     | type          | valeur               | description                                                                                                       |
| --------------------------------------- | ------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `TRANSACTION_NAMESPACE_ID`              | `NamespaceID` | `0x0000000000000001` | Transactions: demandes qui modifient l'état.                                                                      |
| `INTERMEDIATE_STATE_ROOT_NAMESPACE_ID`  | `NamespaceID` | `0x0000000000000002` | Racines d'état intermédiaires (Intermediate state roots), validées après chaque transaction.                      |
| `EVIDENCE_NAMESPACE_ID`                 | `NamespaceID` | `0x0000000000000003` | Preuve : preuves de fraude ou autres preuves d'action "slashable".                                                |
| `TAIL_TRANSACTION_PADDING_NAMESPACE_ID` | `NamespaceID` | `0x00000000000000FF` | Remplissage de la queue pour les transactions: remplissage après toutes les transactions mais avant les messages. |
| `TAIL_PADDING_NAMESPACE_ID`             | `NamespaceID` | `0xFFFFFFFFFFFFFFFE` | Remplissage des messages : remplissage après tous les messages pour remplir la case de données original.          |
| `PARITY_SHARE_NAMESPACE_ID`             | `NamespaceID` | `0xFFFFFFFFFFFFFFFF` | Parts de parité : parts étendues dans la matrice de données disponible.                                           |
<!-- markdownlint-enable MD013 -->
