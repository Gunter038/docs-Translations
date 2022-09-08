- - -
sidebar_label : Celestia's Data Availability Layer
- - -

# Рівень доступності даних Celestia

Celestia - це рівень доступності даних (DA), який забезпечує масштабоване рішення [проблеми доступності даних](https://coinmarketcap.com/alexandria/article/what-is-data-availability). У зв'язку з бездозвільною природою блокчейн мереж, рівень DA повинен забезпечувати механізм для рівнів виконання та розрахунків для перевірки з мінімізованою довірою, чи дані транзакцій дійсно доступні.

Дві ключові особливості шару DA Celestia - це [вибірка доступності даних](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) та [Дерева Меркла, розміщені в просторі імен](https://github.com/celestiaorg/nmt) (NMT). Обидві функції є новими рішеннями для масштабування блокчейну: DAS дозволяє легким вузлам перевіряти наявність даних без необхідності завантажувати весь блок; NMT дозволяє рівням виконання і розрахунків на Celestia завантажувати транзакції, які мають відношення тільки до них.

## Вибірка Доступності Даних (DAS)

В цілому, легкі вузли завантажують тільки заголовки блоків, які містять зобов'язання (тобто корені Меркла) даних блоку (тобто список транзакцій).

Щоб зробити DAS можливим, Celestia використовує 2-вимірну схему кодування Ріда-Соломона для кодування блокових даних: кожен блок даних розбивається на k × k фрагментів, розташованих у матриці k × k, і розширюється за допомогою парності даних у розширену матрицю 2k × 2k шляхом багаторазового застосування кодування Ріда-Соломона.

Потім для рядків і стовпців розширеної матриці обчислюються 4k окремих коренів Меркла; корінь Меркла з цих коренів Меркла використовується як зобов'язання даних блоку в заголовку блоку.

![2D Reed-Soloman (RS) Encoding](/img/concepts/reed-solomon-encoding.png)

Щоб переконатися, що дані доступні, легкі вузли Celestia беруть вибірку з фрагментів даних 2k × 2k.

Кожен легкий вузол випадковим чином вибирає набір унікальних координат у розширеній матриці і запитує повні вузли для отримання фрагментів даних і відповідних доведень Меркла за цими координатами. Якщо легкі вузли отримують дійсну відповідь на кожен запит вибірки, то існує [високоймовірна гарантія](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075) що дані всього блоку доступні.

Крім того, кожен отриманий фрагмент даних з коректним доказом Меркла передається в мережу. В результаті, поки легкі вузли Селестії вибирають разом достатньо блоків даних (тобто, принаймні, k × k унікальних блоків), повний блок може бути відновлений чесними повними вузлами.

Для отримання більш детальної інформації про DAS ознайомтеся з [оригінальним документом](https://arxiv.org/abs/1809.09044).

### Масштабованість

DAS дозволяє Селестії масштабувати шар DA. DAS can be performed by resource-limited light nodes since each light node only samples a small portion of the block data. The more light nodes there are in the network, the more data they can collectively download and store.

This means that increasing the number of light nodes performing DAS allows for larger blocks (i.e., with more transactions), while still keeping DAS feasible for resource-limited light nodes. However, in order to validate block headers, Celestia light nodes need to download the 4k intermediate Merkle roots.

For a block data size of n bytes, this means that every light node must download O(n) bytes. Therefore, any improvement in the bandwidth capacity of Celestia light nodes has a quadratic effect on the throughput of Celestia's DA layer.

### Fraud Proofs of Incorrectly Extended Data

The requirement of downloading the 4k intermediate Merkle roots is a consequence of using a 2-dimensional Reed-Solomon encoding scheme. Alternatively, DAS could be designed with a standard (i.e., 1-dimensional) Reed-Solomon encoding, where the original data is split into k  chunks and extended with k additional chunks of parity data. Since the block data commitment is the Merkle root of the 2k resulting data chunks, light nodes no longer need to download O(n) bytes to validate block headers.

The downside of the standard Reed-Solomon encoding is dealing with malicious block producers that generate the extended data incorrectly.

This is possible as __Celestia does not require a majority of the consensus (i.e., block producers) to be honest to guarantee data availability.__ Thus, if the extended data is invalid, the original data might not be recoverable, even if the light nodes are sampling sufficient unique chunks (i.e., at least k for a standard encoding and k × k for a 2-dimensional encoding).

As a solution, _Fraud Proofs of Incorrectly Generated Extended Data_ enable light nodes to reject blocks with invalid extended data. Such proofs require reconstructing the encoding and verifying the mismatch. With standard Reed-Solomon encoding, this entails downloading the original data, i.e., O(n) bytes. Contrastingly, with 2-dimensional Reed-Solomon encoding, only O(n ) bytes are required as it is sufficient to verify only one row or one column of the extended matrix.

## Namespaced Merkle Trees (NMTs)

Celestia partitions the block data into multiple namespaces, one for every application (e.g., rollup) using the DA layer. As a result, every application needs to download only its own data and can ignore the data of other applications.

For this to work, the DA layer must be able to prove that the provided data is complete, i.e., all the data for a given namespace is returned. To this end, Celestia is using Namespaced Merkle Trees (NMTs).

An NMT is a Merkle tree with the leafs ordered by the namespace identifiers and the hash function modified so that every node  in the tree includes the range of namespaces of all its descendants. The following figure shows an example of an NMT with height three (i.e., eight data chunks). The data is partitioned into three namespaces.

![Namespaced Merkle Tree](/img/concepts/nmt.png)

When an application requests the data for namespace 2, the DA layer must provide the data chunks `D3`, `D4`, `D5`, and `D6` and the nodes `N2`, `N8` and `N7` as proof (note that the application already has the root `N14` from the block header).

As a result, the application is able to check that the provided data is part of the block data. Furthermore, the application can verify that all the data for namespace 2 was provided. If the DA layer provides for example only the data chunks `D4` and `D5`, it must also provide nodes `N12` and `N11` as proofs. However, the application can identify that the data is incomplete by checking the namespace range of the two nodes, i.e., both `N12` and `N11` have descendants part of namespace 2.

For more details on NMTs, take a look at the [original paper](https://arxiv.org/abs/1905.09274).

## Building a PoS Blockchain for DA

### Providing Data Availability

The Celestia DA layer consists of a PoS blockchain. Celestia is dubbing this blockchain as the [Celestia App](https://github.com/celestiaorg/celestia-app), an application that provides transactions to facilitate the DA layer and is built using [Cosmos SDK](https://docs.cosmos.network/v0.44/). The following figure shows the main components of Celestia App.

![Main components of Celestia App](/img/concepts/celestia-app.png)

Celestia App is built on top of [Celestia Core](https://github.com/celestiaorg/celestia-core), a modified version of the [Tendermint consensus algorithm](https://arxiv.org/abs/1807.04938). Among the more important changes to vanilla Tendermint, Celestia Core:

- Enables the erasure coding of block data (using the 2-dimensional Reed-Solomon encoding scheme).
- Replaces the regular Merkle tree used by Tendermint to store block data with a [Namespaced Merkle tree](https://github.com/celestiaorg/nmt) that enables the above layers (i.e., execution and settlement) to only download the needed data (for more details, see the section below describing use cases).

For more details on the changes to Tendermint, take a look at the [ADRs](https://github.com/celestiaorg/celestia-core/tree/v0.34.x-celestia/docs/celestia-architecture). Notice that Celestia Core nodes are still using the Tendermint p2p network.

Similarly to Tendermint, Celestia Core is connected to the application layer (i.e., the state machine) by [ABCI++](https://github.com/tendermint/tendermint/tree/master/spec/abci%2B%2B), a major evolution of [ABCI](https://github.com/tendermint/tendermint/tree/master/spec/abci) (Application Blockchain Interface).

The Celestia App state machine is necessary to execute the PoS logic and to enable the governance of the DA layer.

However, the Celestia App is data-agnostic -- the state machine neither validates nor stores the data that is made available by the Celestia App.
