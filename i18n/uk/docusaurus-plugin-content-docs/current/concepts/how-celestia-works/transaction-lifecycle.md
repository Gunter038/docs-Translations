- - -
sidebar_label : Рівень доступності даних Celestia
- - -

# Життєвий цикл транзакції програми Celestia

Користувачі роблять запит до програми Celestia, щоб зробити дані доступними надсилаючи транзакції `PayForDataі`. Кожна така транзакція складається з ідентифікації відправника, даних, які будуть доступні, які також називаються повідомленням, розміру даних, ідентифікатору простору імен і підпису. Кожен виробник блоків об’єднує кілька транзакцій `PayForData` в один блок.

Перш ніж запропонувати блок, виробник передає його до кінцевого автомату через ABCI++, де кожна транзакція `PayForData` розбивається на повідомлення простору імен (позначене `Msg` на малюнку нижче), тобто дані разом з ідентифікатором простору імен і виконуваною транзакцією (позначеною `e-Tx` на малюнку нижче), яка не містить даних, але лише зобов’язання, яке можна використати пізніше, щоб довести, що дані дійсно були доступні.

Таким чином, дані блоку складаються з даних, розділених на простори імен, і виконуваних транзакцій. Зверніть увагу, що тільки ці транзакції виконуються кінцевим автоматом Celestia як тільки блок буде зафіксовано.

![Lifecycle of a Celestia App Transaction](/img/concepts/tx-lifecycle.png)

Далі виробник блоку додає до заголовка блоку зобов’язання щодо даних блоку. Як описано [тут](./data-availability-layer.md#fraud-proofs-of-incorrectly-extended-data), зобов’язанням є корінь Меркла з 4k проміжних коренів Меркла (тобто по одному для кожного рядка та стовпця розширеної матриці). Щоб обчислити це зобов’язання, виробник блоку виконує такі операції:

- Він розділяє виконувані транзакції та дані простору імен на спільні частки. Кожна частка складається з деяких байтів, попередньо визначених ідентифікатором простору імен. З цією метою виконувані операції пов'язані із зарезервованим простором імен.
- Він розбудовує ці частки на квадратну матрицю (по рядках). Зверніть увагу, що частки доповнюються до наступного ступеня двійки. Квадрат результату розміром k × k називають вихідними даними.
- Він розширює оригінальні дані до квадратної матриці 2k × 2k за допомогою двовимірної схеми кодування Ріда-Соломона, описаної вище. Розширені частки (тобто з помилковими даними) пов’язані з іншим зарезервованим простором імен.
- Він обчислює зобов’язання для кожного рядка та стовпця розширеної матриці, використовуючи вищезгадані NMT.

Таким чином, зобов’язання даних блоку є коренем дерева Merkle з листям і корінням лісу піддерев Merkle з простором імен, по одному для кожного рядка та стовпця розширеної матриці.

## Перевірка наявності даних

![DA network](/img/concepts/consensus-da.png)

To enhance connectivity, the Celestia Node augments the Celestia App with a separate libp2p network, i.e., the so-called _DA network_, that serves DAS requests.

Light nodes connect to a Celestia Node in the DA network, listen to extended block headers (i.e., the block headers together with the relevant DA metadata, such as the 4k intermediate Merkle roots), and perform DAS on the received headers (i.e., ask for random data chunks).

Note that although it is recommended, performing DAS is optional -- light nodes could just trust that the data corresponding to the commitments in the block headers was indeed made available by the Celestia DA layer. In addition, light nodes can also submit transactions to the Celestia App, i.e., `PayForData` transactions.

While performing DAS for a block header, every light node queries Celestia Nodes for a number of random data chunks from the extended matrix and the corresponding Merkle proofs. If all the queries are successful, then the light node accepts the block header as valid (from a DA perspective).

If at least one of the queries fails (i.e., either the data chunk is not received or the Merkle proof is invalid), then the light node rejects the block header and tries again later. The retrial is necessary to deal with false negatives, i.e., block headers being rejected although the block data is available. This may happen due to network congestion for example.

Alternatively, light nodes may accept a block header although the data is not available, i.e., a _false positive_. This is possible since the soundness property (i.e., if an honest light node accepts a block as available, then at least one honest full node will eventually have the entire block data) is probabilistically guaranteed (for more details, take a look at the [original paper](https://arxiv.org/abs/1809.09044)).

By fine tuning Celestia's parameters (e.g., the number of data chunks sampled by each light node) the likelihood of false positives can be sufficiently reduced such that block producers have no incentive to withhold the block data.
