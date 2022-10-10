- - -
sidebar_label : Blockchains Monolithique vs Modulaires
- - -

# Blockchains monolithiques vs modulaires

Les Blockchains instancient [les machines d'état répliquées](https://dl.acm.org/doi/abs/10.1145/98163.98167): les nodes dans un réseau distribué permissionless appliquent une séquence ordonnée de transactions déterministes à un état initial donnant lieu à un état final commun. Cela signifie que les blockchains nécessitent les quatre fonctions suivantes :

- __L'exécution__ implique l'exécution de transactions qui mettent à jour l'état correctement. Ainsi, l'exécution doit s'assurer que seules les transactions valides sont exécutées, c'est-à-dire, des transactions qui entraînent des transitions de state machine valides.
- __Le règlement__ implique un environnement pour les couches d'exécution pour vérifier les preuves, résoudre les litiges de fraude et faire le lien entre les autres couches d'exécution.
- __Le consensus__ consiste à se mettre d'accord sur l'ordre des transactions.
- __La disponibilité des données__ (DA : Data Availability) implique de rendre les données de transaction disponibles. Notez que l'exécution, le règlement et le consensus requièrent une DA.

Les blockchains traditionnels, c'est-à-dire _les blockchains monolithiques_, implémentent les quatre fonctions ensemble dans une seule couche de consensus de base (consensus layer). Le problème avec les blockchains monolithiques est que la couche de consensus doit effectuer beaucoup de tâches différentes et ne peut pas être optimisée pour une seule de ces fonctions. En conséquence, le paradigme monolithique limite le débit du système.

![Modulaire VS Monolithique](/img/concepts/monolithic-modular.png)

En guise de solution, les blockchains modulaires découplent ces fonctions parmi plusieurs couches spécialisées dans le cadre d'une pile modulaire. Grâce à la flexibilité qu'offre la spécialisation, il y a de nombreuses possibilités dans lesquelles cette pile peut être organisée. Par exemple, l'une de ces modalités est la séparation des quatre fonctions en trois couches spécialisées.

La couche de base (base layer) se compose de la DA et du consensus et est donc appelée la couche de Consensus et de DA (ou, par souci de concision, la couche de DA), tandis que le règlement et l'exécution sont placés au-dessus dans leurs propres couches. Par conséquent, chaque couche peut être spécialisée pour n'exécuter que sa fonction de manière optimale et ainsi, augmenter le débit du système. En outre, ce paradigme modulaire permet plusieurs couches d'exécution, c'est-à-dire [rollups](https://vitalik.ca/general/2021/01/05/rollup.html), d'utiliser les mêmes couches de règlement et de DA.
