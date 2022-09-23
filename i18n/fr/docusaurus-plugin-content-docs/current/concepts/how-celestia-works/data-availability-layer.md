- - -
sidebar_label : Couche de disponibilité des données de Celestia
- - -

# Couche de disponibilité des données de Celestia

Celestia est une couche de disponibilité des données (DA) qui fournit une solution évolutive au [problème de la disponibilité des données](https://coinmarketcap.com/alexandria/article/what-is-data-availability). En raison de la nature sans permission des réseaux de blockchain, une couche de DA doit fournir un mécanisme permettant aux couches d'exécution et de consensus de vérifier, cryptographiquement, si les données de transaction sont effectivement disponibles.

Deux caractéristiques clés de la couche DA de Celestia sont [l'échantillonnage de la disponibilité des données](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) et [les arbres de Merkle Namespaced](https://github.com/celestiaorg/nmt) (NMTs). Les deux fonctionnalités sont des solutions novatrices de mise à l'échelle de la blockchain : la DAS permet aux lights nodes de vérifier la disponibilité des données sans avoir à télécharger un bloc entier ; Les NMTs permettent à des couches d'exécution et de règlement sur Celestia de télécharger des transactions qui ne sont pertinentes que pour elles.

## Échantillonnage de la disponibilité des données (DAS)

En général, les lights nodes ne téléchargent que les en-têtes de blocs qui contiennent des engagements (c.-à-d. les racines Merkle) des données de bloc (c.-à-d. la liste des transactions).

Pour rendre la DAS possible, Celestia utilise un système d'encodage Reed-Salomon à deux dimensions pour encoder les données du bloc : chaque donnée de bloc est divisée en tronçon k x k , arrangé dans une matrice k × k, et étendu avec la parité de données dans une matrice étendue de 2k × 2k en appliquant plusieurs fois l'encodage Reed-Solomon.

Then, 4k separate Merkle roots are computed for the rows and columns of the extended matrix; the Merkle root of these Merkle roots is used as the block data commitment in the block header.

![2D Reed-Soloman (RS) Encoding](/img/concepts/reed-solomon-encoding.png)

Pour vérifier que la donnée est disponible, les light nodes Celestia échantillonnent les paquets de données 2k x 2k.

Chaque light node choisit au hasard un ensemble de coordonnées uniques dans la matrice étendue et envoit une requête aux full nodes pour les paquets de données et aux preuves de Merkle correspondant à ces coordonnées. Si un light node reçoit une réponse valide pour chaque échantillon réclamé au full node , alors il y a une [haute probabilité](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075) que toute la donnée du bloc soit disponible.

Egalement, chaque morceau de donnée reçu avec une preuve de Merkle correcte est transmise au réseau. De ce fait, aussi longtemps que les light nodes Celestia échantillonnent ensemble assez de morceaux de données (c-à-d, au moins k x k morceaux uniques), le bloc entier peut être rétabli par les full nodes.

Pour plus de détails sur l'échantillonnage de la disponibilité des données, vous pouvez regarder cet [ article](https://arxiv.org/abs/1809.09044).

### Scalabilité

L'échantillonnage de la disponibilité des données permet à Celestia d'augmenter la capacité de la couche de disponibilité des données. L'échantillonnage de la disponibilité des données peut être effectuée par des light nodes aux ressources limitées puisque chaque light node échantillonne seulement une petite partie des données de chaque bloc. Plus il y a de light nodes dans le réseau et plus ils peuvent télécharger et stocker de données ensemble.

Cela signifie qu'en augmentant le nombre de light nodes réalisant l'échantillonnage de la disponibilité des données, on permet des blocs plus importants (c-à-d avec davantage de transactions), tout en conservant possible l'échantillonnage de la disponibilité des données par des light nodes aux ressources limitées. Cependant, pour valider les en-tête de blocs, les light nodes Celestia ont besoin de télécharger les racines de Merkle des 4k intermédiaires.

Pour une taille de donnée de bloc de n octets, cela signifie que chaque light node doit télécharger O(n) octets. C'est pourquoi chaque amélioration de la bande passante allouée aux light nodes de Celestia a un effet quadratique sur le débit de la couche de disponibilité des données de Celestia.

### Preuves frauduleuses de Données Etendues Incorrectes

La nécessité de télécharger les racines de Merkle des 4k intermédiaires est une conséquence de l'usage d'un schéma d'encodage de Reed-Solomon à deux dimensions. De manière alternative, l'échantillonnage de la disponibilité des données pourrait être conçu avec un encodage Reed-Solomon standard (c-à-d en une dimension), où la donnée originelle est divisée en k morceaux de données et étendue avec k morceaux de données paritaires supplémentaires. Puisque l'engagement des données du bloc est la racine de Merkle des morceaux de données du 2k qui en résulte, les light nodes n'ont plus à télécharger O(n) octets pour valider les en-têtes de bloc.

La contrepartie de l'encodage Reed-Solomon standard est de gérer les producteurs de bloc malveillants qui produisent les données étendues de manière incorrecte.

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
