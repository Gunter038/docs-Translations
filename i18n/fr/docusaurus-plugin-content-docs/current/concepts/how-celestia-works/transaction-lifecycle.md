- - -
sidebar_label : Couche de disponibilité des données de Celestia
- - -

# Cycle de vie d'une transaction Celestia App

Les utilisateurs demandent à l'application Celestia de rendre les données disponibles en envoyant des transactions `PayForData`. Chacune de ces transactions de ce type se compose de l'identité de l'expéditeur, des données à mettre à disposition aussi appelé le message, la taille des données, le namespace ID, et une signature. Chaque producteur de bloc effectue en lot plusieurs transactions `PayForData` dans un bloc.

Avant de proposer le bloc, le producteur le transmet à la machine à états via ABCI++, où chaque transaction `PayForData` est divisée en un message espacé par un nom (désigné par `Msg` dans la figure ci-dessous), c'est-à-dire les données avec le namespace ID, et une transaction exécutable (désignée par `e-Tx` dans la figure ci-dessous) qui ne contient pas les données, mais seulement un engagement qui peut être utilisé ultérieurement pour prouver que les données ont bien été mises à disposition.

Ainsi, les données du bloc se composent de données partitionnées en namespaces et en transactions exécutables. Notez que seules ces transactions sont exécutées par la machine à états de Celestia une fois le bloc validé.

![Cycle de vie d'une transaction Celestia App](/img/concepts/tx-lifecycle.png)

Ensuite, le producteur du bloc ajoute à l'en-tête du bloc un engagement des données du bloc. Comme décrit [ici](./data-availability-layer.md#fraud-proofs-of-incorrectly-extended-data), le "commitment" est la racine de Merkle des 4k racines de Merkle intermédiaires (c'est-à-dire une pour chaque ligne et colonne de la matrice étendue). Pour calculer cet engagement, le producteur de blocs effectue les opérations suivantes :

- Il divise les transactions exécutables et les données namespaced en parts. Chaque part est constituée de quelques octets préfixés par un namespace ID. À cette fin, les transactions exécutables sont associées à un namespace réservé.
- Il classe ces parts dans une matrice carrée (par rangées). Notez que les parts sont complétées à la prochaine puissance de deux. Le carré de résultats de taille k × k est appelé donnée d'origine (original data).
- Il étend les données originales à une matrice carrée de 2k × 2k en utilisant le schéma de codage de Reed-Solomon à 2 dimensions décrit ci-dessus. Les parts étendues (c'est-à-dire contenant des données d'effacement) sont associées à un autre namespace réservé.
- Il calcule un engagement pour chaque ligne et chaque colonne de la matrice étendue en utilisant les NMT décrites ci-dessus.

Ainsi, l'engagement des données du bloc est la racine d'un arbre de Merkle dont les feuilles sont les racines d'une forêt de sous-arbres de de Merkle Namespaced , un pour chaque ligne et colonne de la matrice étendue.

## Vérification de la disponibilité des données

![DA network](/img/concepts/consensus-da.png)

Pour améliorer la connectivité, le Node Celestia améliore l'App Celestia avec un réseau libp2p séparé, c'est-à-dire le réseau dit _DA_, qui répond aux requêtes de DAS.

Les light nodes se connectent à un Node Celestia dans le réseau de DA, écoutent les en-têtes de blocs étendus (c'est-à-dire les en-têtes de blocs accompagnés des métadonnées de DA pertinentes, telles que les racines de Merkle intermédiaires 4k), et exécutent un DAS sur les en-têtes reçus (c'est-à-dire demander des morceaux de données aléatoires).

Notez que, bien qu'elle soit recommandée, l'exécution de la DAS est facultative -- les light nodes pourraient simplement faire confiance aux données correspondant d'engagements dans les en-têtes de bloc, qui ont été effectivement rendus disponibles par la couche de DA de Celestia. De plus, les light nodes peuvent également soumettre des transactions à Celestia App, c'est-à-dire des transactions `PayForData`.

Lors de l'exécution du DAS pour un en-tête de bloc, chaque light node demande aux nodes Celestia un certain nombre de morceaux de données aléatoires provenant de la matrice étendue et des preuves de Merkle correspondantes. Si toutes les requêtes sont réussies, alors le light node accepte l'en-tête de bloc comme valide (du point de vue de DA).

Si au moins l'une des requêtes échoue (c'est-à-dire que le morceau de données n'est pas reçu ou que la preuve de Merkle n'est pas valide), le light node rejette l'en-tête de bloc et réessaie plus tard. Le nouvel essai est nécessaire pour traiter les faux négatifs, c'est-à-dire que les en-têtes de bloc sont rejetés bien que les données du bloc soient disponibles. Cela peut être dû par exemple à la congestion du réseau.

Alternativement, les light nodes peuvent accepter un en-tête de bloc bien que les données ne soient pas disponibles, c'est-à-dire un _faux positif_. Cela est possible car la propriété de solidité (c'est-à-dire que si un light node honnête accepte un bloc comme disponible, alors au moins un full node honnête aura éventuellement les données du bloc entier) est garantie de manière probabiliste (pour plus de détails, jetez un coup d'œil au [document original](https://arxiv.org/abs/1809.09044)).

En affinant les paramètres de Celestia (par exemple, le nombre de blocs de données échantillonnés par chaque light node), la probabilité de faux positifs peut être suffisamment réduite pour que les producteurs de blocs ne soient pas incités à retenir les données des blocs.
