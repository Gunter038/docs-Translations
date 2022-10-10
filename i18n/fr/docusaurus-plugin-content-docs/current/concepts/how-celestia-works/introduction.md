# Introduction

Celestia est un réseau de blockchain modulaire dont le but est de construire [une couche de disponibilité de données](https://blog.celestia.org/celestia-a-scalable-general-purpose-data-availability-layer-for-decentralized-apps-and-trust-minimized-sidechains/) scalable, permettant la prochaine génération d'architectures de blockchains scalables - les [blockchains modulaires](https://celestia.org/learn/). Celestia scale en [découplant l'exécution du consensus](https://arxiv.org/abs/1905.09274) et en introduisant un nouveau paradigme, [l'échantillonnage de la disponibilité des données](https://arxiv.org/abs/1809.09044).

La première implique que Celestia n'est responsable que de l'ordre des transactions et de la garantie de la disponibilité des données ; c'est similaire à [réduire le consensus à la diffusion atomique](https://en.wikipedia.org/wiki/Atomic_broadcast#Equivalent_to_Consensus).

Cette dernière offre une solution efficace au [problème de disponibilité des données](https://coinmarketcap.com/alexandria/article/what-is-data-availability) en ne demandant qu'aux light nodes aux ressources limitées d'échantillonner un petit nombre de morceaux aléatoires de chaque bloc pour vérifier la disponibilité des données.

Il est intéressant de noter qu'un plus grand nombre de light nodes participant à l'échantillonnage augmente la quantité de données que le réseau peut traiter en toute sécurité, permettant à la taille du bloc d'augmenter sans augmenter également le coût de vérification de la chain.
