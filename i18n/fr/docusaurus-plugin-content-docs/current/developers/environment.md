- - -
sidebar_label : Configuration de l'environnement
- - -

# Environnement de développement

Ce tutoriel va passer en revue la configuration de votre environnement de développement pour exécuter le logiciel Celestia. Cet environnement peut être utilisé pour le développement, la construction de binaires et l'exécution de nodes.

## Installation des dépendances

Une fois que vous avez configuré votre instance, ssh dans l'instance pour commencer à installer les dépendances nécessaires pour exécuter un node.

Tout d'abord, assurez-vous de mettre à jour le système d'exploitation:

```sh
# Si vous utilisez le gestionnaire de package APT
sudo apt update && sudo apt upgrade -y

# Si vous utilisez le gestionnaire de package YUM 
sudo yum update
```

Ce sont des paquets essentiels qui sont nécessaires pour exécuter de nombreuses tâches telles que le téléchargement de fichiers, la compilation et la surveillance du node :

<!-- markdownlint-disable MD013 -->
```sh
# Si vous utilisez le gestionnaire APT
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# Si vous utilisez le gestionnaire YUM
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

## Installation de Golang

Celestia-app et Celestia-node sont écrits en [Golang](https://go.dev/), il faut donc installer Golang pour les construire et les exécuter.

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Maintenant nous devons ajouter le répertoire `/usr/local/go/bin` à `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Pour vérifier que Go a été installé correctement, exécutez:

```sh
go version
```

La sortie doit être la version installée :

```sh
go version go1.19.1 linux/amd64
```
