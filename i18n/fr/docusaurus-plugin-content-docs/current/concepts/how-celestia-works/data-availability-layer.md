- - -
sidebar_label : Couche de disponibilité des données de Celestia
- - -

# Couche de disponibilité des données de Celestia

Celestia est une couche de disponibilité des données (DA) qui fournit une solution évolutive au [problème de la disponibilité des données](https://coinmarketcap.com/alexandria/article/what-is-data-availability). En raison de la nature sans permission des réseaux de blockchain, une couche de DA doit fournir un mécanisme permettant aux couches d'exécution et de consensus de vérifier, cryptographiquement, si les données de transaction sont effectivement disponibles.

Deux caractéristiques clés de la couche DA de Celestia sont [l'échantillonnage de la disponibilité des données](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) et [les arbres de Merkle "Namespaced"](https://github.com/celestiaorg/nmt) (NMTs). Les deux fonctionnalités sont des solutions novatrices de mise à l'échelle de la blockchain : le DAS permet aux light nodes de vérifier la disponibilité des données sans avoir à télécharger un bloc entier ; les NMTs permettent à des couches d'exécution et de règlement sur Celestia de télécharger des transactions qui ne sont pertinentes que pour elles.

## Échantillonnage de la disponibilité des données (DAS)

En général, les light nodes ne téléchargent que les en-têtes de blocs qui contiennent des engagements (c.-à-d. les racines Merkle) des données de bloc (c.-à-d. la liste des transactions).

Pour rendre le DAS possible, Celestia utilise un système d'encodage Reed-Salomon à deux dimensions pour encoder les données du bloc : chaque donnée de bloc est divisée en tronçon k x k , arrangé dans une matrice k × k, et étendu avec la parité de données dans une matrice étendue de 2k × 2k en appliquant plusieurs fois l'encodage Reed-Solomon.

Ensuite, les racines de Merkle 4k séparées sont calculées pour les lignes et les colonnes de la matrice étendue ; la racine de Merkle de ces racines de Merkle est utilisée comme l'engagement des données du bloc dans les en-têtes de bloc.

![Encodage Reed-Solomon (RS) 2D](/img/concepts/reed-solomon-encoding.png)

Pour vérifier que la donnée est disponible, les light nodes Celestia échantillonnent les paquets de données 2k x 2k.

Chaque light node choisit au hasard un ensemble de coordonnées uniques dans la matrice étendue et envoie une requête aux full nodes pour les paquets de données et aux preuves de Merkle correspondant à ces coordonnées. Si un light node reçoit une réponse valide pour chaque échantillon réclamé au full node, alors il y a une [haute probabilité](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075) que toutes les données du bloc soient disponibles.

Également, chaque morceau de donnée reçu avec une preuve de Merkle correcte est transmise au réseau. De ce fait, aussi longtemps que les light nodes Celestia échantillonnent ensemble assez de morceaux de données (c-à-d, au moins k x k morceaux uniques), le bloc entier peut être rétabli par les full nodes.

Pour plus de détails sur l'échantillonnage de la disponibilité des données, vous pouvez regarder cet [ article](https://arxiv.org/abs/1809.09044).

### Scalabilité

L'échantillonnage de la disponibilité des données permet à Celestia d'augmenter la capacité de la couche de disponibilité des données. L'échantillonnage de la disponibilité des données peut être effectué par des light nodes aux ressources limitées puisque chaque light node échantillonne seulement une petite partie des données de chaque bloc. Plus il y a de light nodes dans le réseau et plus ils peuvent télécharger et stocker de données ensemble.

Cela signifie qu'en augmentant le nombre de light nodes réalisant l'échantillonnage de la disponibilité des données, on permet des blocs plus importants (c-à-d avec davantage de transactions), tout en conservant possible l'échantillonnage de la disponibilité des données par des light nodes aux ressources limitées. Cependant, afin de valider en-têtes de blocs, les Light nodes Celestia doivent télécharger les 4k racines intermédiaires de Merkle.

Pour une taille de donnée de bloc de n octets, cela signifie que chaque light node doit télécharger O(n) octets. C'est pourquoi chaque amélioration de la bande passante allouée aux light nodes de Celestia a un effet quadratique sur le débit de la couche de disponibilité des données de Celestia.

### Preuves de fraude de données étendues de manière incorrecte

La nécessité de télécharger les racines de Merkle des 4k intermédiaires est une conséquence de l'usage d'un schéma d'encodage de Reed-Solomon à deux dimensions. De manière alternative, l'échantillonnage de la disponibilité des données pourrait être conçu avec un encodage Reed-Solomon standard (c-à-d en une dimension), où la donnée originelle est divisée en k morceaux de données et étendue avec k morceaux de données paritaires supplémentaires. Puisque l'engagement des données du bloc est la racine de Merkle des morceaux de données du 2k qui en résulte, les light nodes n'ont plus à télécharger O(n) octets pour valider les en-têtes de bloc.

La contrepartie de l'encodage Reed-Solomon standard est de gérer les producteurs de blocs malveillants qui produisent les données étendues de manière incorrecte.

C'est toutefois possible puisque __Celestia ne nécessite pas qu'une majorité du consensus (c.-à-d. des producteurs de blocs) soit honnête pour garantir la disponibilité des données.__ De ce fait, si la donnée étendue est invalide, la donnée originale pourrait ne pas être récupérable, même si les light nodes échantillonnaient des morceaux uniques suffisants (c.-à-d. au moins k pour un encodage standard et k × k pour un encodage à deux dimensions).

Comme solution, _les preuves de fraude de données étendues de manière incorrecte_ permettent aux light nodes de rejeter les blocs dont les données étendues sont invalides. Ces preuves nécessitent de reconstruire l'encodage et de vérifier les incohérences. Avec l'encodage Reed-Solomon standard, cela implique de télécharger les données originelles, c.-à-d. O(n) octets. A contrario, avec l'encodage Reed-Solomon à deux dimensions, seul O(n ) octets sont nécessaires puisque suffisants pour vérifier une ligne ou une colonne de la matrice étendue.

## Les Arbres de Merkle Namespaced (NMTs)

Celestia cloisonne les données d'un bloc en plusieurs espaces de noms, un pour chaque application (par exemple, rollup) en utilisant la couche d'accessibilité des données. Il en résulte que chaque application a besoin de télécharger uniquement ses propres données et peut ignorer toutes les autres applications.

Pour ce travail, la couche d'accessibilité des données doit être en mesure de prouver que les données fournies sont complètes, c.-à-d. que toutes les données d'un espace de nom sont revenues. Pour y arriver, Celestia utilise les Arbres de Merkle Namespaced (NMTs).

Un NMT est un arbre de Merkle dont les feuilles sont ordonnées par les identifiants de l'espace de nom et la fonction hash modifiée de sorte que chaque noeud de l'arbre inclue la plage d'espaces de noms de tous ses descendants. La figure suivante montre un exemple de MNT de hauteur trois (c.-à-d. huit morceaux de données). La donnée est divisée en trois espaces de noms.

![Arbre de Merkle Namespaced](/img/concepts/nmt.png)

Quand une application requiert la donnée pour l'espace de nom 2, la couche d'accessibilité des données doit fournir les morceaux de données `D3`, `D4`, `D5`, et `D6` et les noeuds `N2`,`N8` et `N7` comme preuve (notez que l'application a déjà la racine `N14` provenant de l'en-tête du bloc).

Il en résulte que l'application est en capacité de vérifier que la donnée fournie fait bien partie de la donnée du bloc. De plus, l'application peut vérifier que toute la donnée pour le namespace 2 a été fournie. Si la couche d'accessibilité des données fournit par exemple les morceaux de données `D4` et `D5`, elle doit également fournir les nodes `N12` et `N11` en tant que preuves. Cependant, l'application peut identifier que les données sont incomplètes en vérifiant la plage de l'espace de nom des deux nœuds, c.-à-d. que `N12` et `N11` ont tous les deux des parties descendantes de l'espace de nom 2.

Pour davantage de détails sur les NMTs, vous pouvez consulter cet [article](https://arxiv.org/abs/1905.09274).

## Construire une blockchain PoS pour la couche d'accessibilité

### Fournir l'Accessibilité des Données

La couche d'accessibilité des données Celestia consiste en une blockchain PoS. Celestia appelle une blockchain "[Celestia App](https://github.com/celestiaorg/celestia-app)" une application qui permet des transactions pour faciliter la couche d'accessibilité des données et est construite en utilisant le [Cosmos SDK](https://docs.cosmos.network/v0.44/). Le schéma suivant montre les différents composants d'une Celestia App.

![Principaux composants de l’app Celestia](/img/concepts/celestia-app.png)

La Celestia App est construite sur le [Celestia Core](https://github.com/celestiaorg/celestia-core), une version modifiée du [consensus algorithmique Tendermint](https://arxiv.org/abs/1807.04938). Parmi les plus importants changements apportés à Tendermint, Celestia Core:

- Permet le codage d'effacement des données d'un bloc (en utilisant le schéma d'encodage Reed-Solomon en deux dimensions).
- Replace l'arbre de Merkle habituel utilisé par Tendermint pour stocker les données avec un [arbre de Merkle Namespaced](https://github.com/celestiaorg/nmt) qui permet aux couches d'au-dessus (c.-à-d. d'exécution ou de règlement) de seulement télécharger les données nécessaires (pour plus de détails, voir la section ci-dessous décrivant les cas d'usage).

Pour plus de détails sur les changements par rapport à Tendermint, consulter [ADRs](https://github.com/celestiaorg/celestia-core/tree/v0.34.x-celestia/docs/celestia-architecture). À noter que les nœuds de Celestia Core utilisent toujours le réseau Tendermint de pair-à-pair.

De manière similaire à Tendermint, Celestia Core est connecté à la couche d'application (c.-à-d., la machine d'état) par [ABCI++](https://github.com/tendermint/tendermint/tree/master/spec/abci%2B%2B), une évolution majeure de [ABCI](https://github.com/tendermint/tendermint/tree/master/spec/abci) (Interface d'Application Blockchain).

L'état de machine Celestia App est nécessaire pour exécuter la logique PoS et pour permettre la gouvernance de la couche d'accessibilité des données.

Cependant, l’app Celestia est dite agnostique en ce qui concerne les données -- la machine d'état ne valide ni ne stocke les données qui sont rendues accessibles par l’app Celestia.
