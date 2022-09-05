- - -
sidebar_label : Слой доступности данных Celestia
- - -

# Слой доступности данных Celestia

Celestia - это слой доступности данных (DA), который обеспечивает масштабируемое решение проблемы [ доступности данных](https://coinmarketcap.com/alexandria/article/what-is-data-availability). В связи с неразрешимой природой блокчейн сетей, слой DA должен обеспечить механизм для уровней исполнения и расчетов, позволяющий проверять с минимальным уровнем доверия, действительно ли данные транзакции доступны.

Двумя ключевыми особенностями уровня DA в Celestia являются [выборка доступности данных](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) и [ деревья Меркла с распределением имен](https://github.com/celestiaorg/nmt) (NMTs). Обе функции представляют собой новые решения для масштабирования блокчейна: DAS позволяет легким узлам проверять доступность данных без необходимости загрузки всего блока; NMT позволяют слоям исполнения и расчетов на Celestia загружать транзакции которые относятся только к ним.

## Выборка доступности данных (DAS)

В общем, легкие узлы загружают только заголовки блоков, которые содержат коммиты (т.е. корни Меркла) данных блока (т.е. список транзакций).

Чтобы сделать DAS возможным, Celestia использует двухмерную схему Reed-Solomon кодирования для кодирования данных блока: каждый блок данных разделяется на k × k, в матрице k × k, и расширяется с чётностью данных в 2k × 2k расширенную матрицы путем многократного использования кодирования Reed-Solomon.

Затем вычисляются 4k отдельных корней Меркла для строк и столбцов расширенной матрицы; корень Меркла из этих корней Меркла используется в качестве обязательства данных блока в заголовке блока.

![2D Reed-Soloman (RS) Encoding](/img/concepts/reed-solomon-encoding.png)

Чтобы убедиться в наличии данных, легкие узлы Celestia делают выборку фрагмента данных размером 2k × 2k.

Каждый легкий узел случайным образом выбирает набор уникальных координат в расширенной матрице и запрашивает полные узлы для получения блоков данных и соответствующие доказательства Меркла в этих координатах. Если легкие узлы получают правильный ответ на каждый запрос выборки, то с [высокой долей вероятности](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075) данные всего блока доступны.

Кроме того, каждый полученный фрагмент данных с правильным доказательством Меркла передается в сеть. В результате до тех пор, пока легкие узлы Celestia собирают вместе достаточное количество блоков данных (т.е., по крайней мере, k × k уникальных блоков), полный блок может быть восстановлен честными полными узлами.

Для получения более подробной информации о DAS ознакомьтесь с [оригинальной статьей](https://arxiv.org/abs/1809.09044).

### Масштабирование

DAS позволяет Celestia масштабировать слой DA. DAS может выполняться с помощью ограниченных ресурсов легких узлов, поскольку каждый легкий узел производит только небольшую часть данных блока. Чем больше легких узлов есть в сети, тем больше данных, которые они могут загрузить и хранить.

Это означает, что увеличение числа легких узлов, выполняющих DAS, позволяет формировать большие блоки (например, с большим количеством транзакций), но при этом DAS может быть использована для легких узлов ограниченного ресурса. Однако для того, чтобы проверить заголовки блоков, легким узлам Celestia необходимо загрузить промежуточные 4k корни Меркла.

Для размера блока данных n байт это означает, что каждый легкий узел должен загрузить O(n) байт. Таким образом, любое улучшение пропускной способности легких узлов Celestia имеет квадратичный эффект на пропускную способность DA слоя.

### Доказательства мошенничества с неверно распространенными данными

Требование загрузки промежуточных 4k корней Меркла является следствием использования двумерной схемы кодирования Рида-Соломона. В качестве альтернативы, DAS может быть разработана со стандартным (т.е. одномерным) кодированием Рида-Соломона, где исходные данные разбиваются на k фрагментов и увеличиваются на k дополнительных фрагментов данных о четности. Поскольку обязательство данных блока является корнем Меркла из 2k результирующих блоков данных, легким узлам больше не нужно загружать O(n) байт для проверки заголовков блоков.

Недостатком стандартного кодирования Рида-Соломона является работа со злонамеренными производителями блоков, которые неправильно генерируют увеличенные данные.

Это возможно, поскольку __Celestia не требует, чтобы большинство участников консенсуса (т.е. производителей блоков) были честными, чтобы гарантировать доступность данных.__ Таким образом, если увеличенные данные недействительны, исходные данные могут не подлежать восстановлению, даже если легкие узлы производят выборку достаточного количества уникальных фрагментов (т.е., по крайней мере, k для стандартного кодирования и k × k для двумерного кодирования).

В качестве решения _Доказательства мошенничества в отношении неверно сгенерированных расширенных данных_ позволяют легким узлам отклонять блоки с недействительными расширенными данными. Такие доказательства требуют восстановления кодировки и проверки несоответствия. При стандартном кодировании Рида-Соломона это требует загрузки исходных данных, т.е. O(n) байт. Напротив, при двумерном кодировании Рида-Соломона требуется только O(n ) байт, поскольку достаточно проверить только одну строку или один столбец расширенной матрицы.

## Деревья Меркла с пространством имен (NMT)

Celestia разделяет данные блока на несколько пространств имен, по одному для каждого приложения (например, rollup), использующего слой DA. В результате каждое приложение должно загружать только свои собственные данные и может игнорировать данные других приложений.

Для того чтобы это работало, слой DA должен быть в состоянии доказать, что предоставленные данные являются завершёнными, т.е. все предоставленные данные для пространства имён возвращены. Для этого Celestia использует Namespaced Merkle Trees (NMTs).

NMT - это дерево Меркла, листья которого упорядочены идентификаторами пространства имен, а хэш-функция изменена таким образом, что каждый узел дерева включает диапазон пространства имен всех своих потомков. The following figure shows an example of an NMT with height three (i.e., eight data chunks). The data is partitioned into three namespaces.

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
