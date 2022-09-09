- - -
sidebar_label : Слой доступности данных Celestia
- - -

# Жизненный цикл транзакции приложения Celestia

Пользователи запрашивают приложение Celestia для того, чтобы сделать данные доступными, отправив `PayForData` транзакции. Каждая такая транзакция состоит из идентификатора отправителя, данных, которые должны быть доступны, также называется сообщением, размер данных, идентификатор пространства имен и подпись. Каждый производитель блоков собирает несколько `PayForData` транзакций в блок.

Однако прежде чем предложить блок, производитель передает его в конечный автомат через ABCI++, где каждая транзакция `PayForData` разделяется на сообщение с пространством имен (обозначенное `Msg` на рисунке ниже), т.е. на данные вместе с идентификатором пространства имен, и исполняемую транзакцию (обозначенную `e-Tx` на рисунке ниже), которая не содержит данные, а только обязательство, которое может быть использовано позже, чтобы доказать, что данные действительно были доступны.

Таким образом, данные блоков состоят из данных, разделенных на пространства имен и исполняемые транзакции. Обратите внимание, что только эти транзакции выполняются конечным автоматом Celestia после совершения блока.

![Lifecycle of a Celestia App Transaction](/img/concepts/tx-lifecycle.png)

Далее производитель блока добавляет к заголовку блока обязательство данных блока. Как описано [здесь](./data-availability-layer.md#fraud-proofs-of-incorrectly-extended-data), обязательство является корнем Меркла из 4k промежуточных корней Меркла (т.е. по одному для каждой строки и столбца расширенной матрицы). Для расчета этого обязательства, производитель блоков выполняет следующие операции:

- Он разделяет исполняемые транзакции и данные с именами на части. Каждая часть состоит из нескольких байт, начинающихся с идентификатора пространства имен. Для этого исполняемые транзакции ассоциируются с зарезервированным пространством имен.
- Он упорядочивает эти части в квадратную матрицу (по строкам). Обратите внимание, что части дополняются до следующей степени двойки. Итоговый квадрат размером k × k называется исходными данными.
- Он расширяет исходные данные до квадратной матрицы 2k × 2k, используя 2-мерную систему кодирования Рида-Соломона, описанную выше. Расширенные части (т.е. содержащие данные об удалении) ассоциируются с другим зарезервированным пространством имен.
- Он вычисляет обязательство для каждой строки и столбца расширенной матрицы, используя NMT, описанное выше.

Таким образом, фиксация данных блока является корнем дерева Меркла с листьями, корнями леса поддеревьев Меркла с пространством имен по одному на каждую строку и столбец расширенной матрицы.

## Проверка доступности данных

![DA network](/img/concepts/consensus-da.png)

To enhance connectivity, the Celestia Node augments the Celestia App with a separate libp2p network, i.e., the so-called _DA network_, that serves DAS requests.

Light nodes connect to a Celestia Node in the DA network, listen to extended block headers (i.e., the block headers together with the relevant DA metadata, such as the 4k intermediate Merkle roots), and perform DAS on the received headers (i.e., ask for random data chunks).

Note that although it is recommended, performing DAS is optional -- light nodes could just trust that the data corresponding to the commitments in the block headers was indeed made available by the Celestia DA layer. In addition, light nodes can also submit transactions to the Celestia App, i.e., `PayForData` transactions.

While performing DAS for a block header, every light node queries Celestia Nodes for a number of random data chunks from the extended matrix and the corresponding Merkle proofs. If all the queries are successful, then the light node accepts the block header as valid (from a DA perspective).

If at least one of the queries fails (i.e., either the data chunk is not received or the Merkle proof is invalid), then the light node rejects the block header and tries again later. The retrial is necessary to deal with false negatives, i.e., block headers being rejected although the block data is available. This may happen due to network congestion for example.

Alternatively, light nodes may accept a block header although the data is not available, i.e., a _false positive_. This is possible since the soundness property (i.e., if an honest light node accepts a block as available, then at least one honest full node will eventually have the entire block data) is probabilistically guaranteed (for more details, take a look at the [original paper](https://arxiv.org/abs/1809.09044)).

By fine tuning Celestia's parameters (e.g., the number of data chunks sampled by each light node) the likelihood of false positives can be sufficiently reduced such that block producers have no incentive to withhold the block data.
