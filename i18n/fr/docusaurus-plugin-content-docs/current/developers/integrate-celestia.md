---
sidebar_label: Intégrer Celestia
---

# Intégrer Celestia

> Ce document s'adresse aux prestataires de services tiers, tels que les dépositaires et explorateurs intégrant le réseau Celestia.

## Notes à l'attention des fournisseurs de service Celestia

Celestia est une chaine standard basée sur le Cosmos-SDK. Nous utilisons la dernière version de Tendermint et du Cosmos-SDK, avec seulement quelques modifications mineures. Cela signifie que nous :

- Utilisons les modules par défaut du Cosmos-SDK : authentification, banque, distribution, staking, slashing, mint, ibchost, genutil, evidence, ibctransfer, params, gov (limité dans certaines capacités TBD), mise à niveau, vesting, feegrant, capacité et paiement.
- Utilisons les schémas standards de clés numériques fournis par le Cosmos-SDK et par Tendermint, ceux-ci étant secp256k1 pour les transactions des utilisateurs et tm-ed25519 pour la signature et la vérification des messages de consensus.

Bien que les modules utilisés soient encore sujets à des changements, Celestia vise à être le plus minimal possible.

### Détention et gestion des clés

Celestia prend en charge de nombreux systèmes de gestion des clés existants, car nous comptons sur les bibliothèques du Cosmos-SDK et de Tendermint pour signer et vérifier les transactions. [documentation Cosmos-SDK](https://docs.cosmos.network/master/basics/accounts.html#keys-accounts-addresses-and-signatures)

### Terminaux RPC et requête

Dans Celestia App, seuls les terminaux standards RPC pour Tendermint et le Cosmos-SDK sont exposés. Nous n'ajoutons ni ne soustrayons actuellement aucune fonctionnalité de base, mais cela pourrait changer à l'avenir. Il en va de même pour la requête des données de la chaine.

Dans Celestia-node, le client du node de disponibilité des données, il y a une API JSON-RPC qui vous permet d'interagir directement avec la couche de disponibilité des données de Celestia. Le guide pour cela peut être trouvé [ici](https://docs.celestia.org/developers/node-tutorial).

### Compatibilité

Linux, en particulier Ubuntu 20.04 LTS, est l'OS le mieux éprouvé. Celestia est possiblement compatible avec d'autres OS, mais cela n'a pas encore été testé. Certaines des bibliothèques de cryptographie utilisées pour effacer les données ne sont pas garanties de fonctionner sur d'autres plates-formes.

### Synchronisation

Puisque nous utilisons Tendermint et le Cosmos-SDK, la synchronisation de la chaine peut être effectuée par n'importe quelle méthode supportée par ces bibliothèques. Cela inclut la synchronisation rapide, la synchronisation d'état et la synchronisation simple.

### Exceptions notables relatives aux autres blockchains

Par rapport aux autres chaines basées sur Tendermint, Celestia aura un temps de génération des blocs significativement plus long, d'environ 30* secondes. La raison de cette longueur est d'optimiser la bande passante utilisée par les clients légers qui échantillonnent la chaine, et pas parce que nous aurions modifié le consensus Tendermint de manière importante. Les validateurs vont probablement download et upload des blocs relativement grands. Il est à noter que bien que ces blocs soient grands, il n'y a généralement que très peu d'exécution de l'état de la blockchain sur Celestia. Cela signifie que la bande passante requise sera probablement plus grande que celle d'un full node d'une blockchain typique basée sur le Cosmos-SDK, et les exigences de calcul devraient être de la même importance.

*Sujet à changement
