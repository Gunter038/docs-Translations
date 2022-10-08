- - -
sidebar_label : Traductions de Docs
- - -

# Aide à la traduction communautaire

Si vous êtes un membre passionné de la communauté Celestia et que vous souhaitez contribuer à la traduction de la page de documentation, ce guide est fait pour vous.

## Visitez notre projet sur Crowdin

Pour commencer, allez sur le projet Crowdin [ici](https://crowdin.com/project/celestia-docs).

Vous devrez créer un compte et vous serez alors en mesure de rejoindre le projet afin de commencer vos traductions.

Si vous ne voyez pas votre langue, n'hésitez pas à la demander sur le canal `#translations` sur Discord [ici](https://discord.gg/celestiacommunity).

Sur Crowdin, vous pouvez traduire, commenter des traductions, et aussi donner des votes positifs et des votes négatifs aux traductions existantes.

Donnez votre avis sur les traductions existantes pour vous assurer qu'elles sont correctes !

## Conseils

Voici quelques conseils pour vous aider pendant votre traduction.

### Documentation Crowdin

Le document officiel de Crowdin est disponible [ici](https://support.crowdin.com/online-editor).

### Guide

#### Code

Certaines pages contiennent des métadonnées et du code informatique.

Il est important de garder à l'esprit que William Shakespeare était un anglophone...tout comme Alan Turing! C'est pourquoi vous ne devez pas traduire des parties du code "lui-même".

Par exemple, si vous voyez des métadonnées comme `sidebar_label : Hello World`, une traduction française serait `sidebar_label : Salut tout le monde`.

Prenons un autre exemple, vous n'aurez pas à traduire quoi que ce soit ici:

```sh
cd $HOME
rm -rf celestia-app
git clone https://github.com/celestiaorg/celestia-app.git
cd celestia-app/
APP_VERSION=$(curl -s \
  https://api.github.com/repos/celestiaorg/celestia-app/releases/latest \
  | jq -r ".tag_name")
git checkout tags/$APP_VERSION -b $APP_VERSION
make install
```

De plus, vous n'avez pas à traduire les URLs dans votre langue locale.

#### Mots spécifiques

Au fur et à mesure que vous traduirez des concepts innovants, comme Data Availability Sampling , n'hésitez pas à discuter de la meilleure traduction avec le reste de la communauté .

De plus, faites attention à l'ordre des dates, au point et aux virgules concernant les chiffres d'une langue à l'autre.
