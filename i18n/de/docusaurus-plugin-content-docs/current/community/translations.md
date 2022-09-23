- - -
sidebar_label : Docs Translations
- - -

# Community Translation Support

If you are a passionate Celestia community member who would like to contribute to translating the documentation page, then this is the guide for you.

## Besuche unser Crowdin Projekt

Um zu beginnen, geh zum Crowdin Projekt [hier](https://crowdin.com/project/celestia-docs).

Du musst zuerst einen Account anlegen, dann kannst du dem Projekt beitreten und deine Reise als Übersetzer beginnen.

If you don't see your language, feel free to ask for it on the `#translations` channel on Discord [here](https://discord.gg/celestiacommunity).

On Crowdin you can translate, comment on translations, and also give upvotes and downvotes to existing translations.

Gib deine Meinung zu bereits existierenden Übersetzungen um sicherzustellen, dass sie richtig sind!

## Tipps

Hier sind ein paar Tipps, die dir beim Übersetzen helfen.

### Crowdin Dokumentation

Die offizielle Crowdin Dokumentation ist [Hier](https://support.crowdin.com/online-editor) verfügbar.

### Anleitung

#### Code

Some pages contain metadata and computer code.

Es ist wichtig, daran zu denken, dass William Shakespeare Englisch gesprochen hat... genauso wie Alan Turing! Darum solltest du nicht Teile des Codes "selbst" übersetzen.

Zum Beispiel, wenn du Metadaten wie `siedebar_label : Hello World`, eine Deutsche Übersetzung wäre `sidebar_label : Hallo Welt`.

Hier ein anderes Beispiel, dort müsste man überhaupt nichts übersetzen:

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

Des weiteren solltest du keine URLs in deine Sprache übersetzen.

#### Besondere Wörter

As you will translate innovative concepts, like Data Availability Sampling, feel free to discuss about the best translation with the rest of the community.

Also, be careful with date order, period and commas regarding numbers from a language to another.
