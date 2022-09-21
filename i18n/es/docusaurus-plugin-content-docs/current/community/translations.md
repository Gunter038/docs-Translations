- - -
sidebar_label : Traducciones de documentos
- - -

# Soporte para traducciónes de la comunidad

Si eres un apasionado miembro de la comunidad Celestia que desea contribuir con traducciones de la página de documentación, entonces ésta guía es para ti.

## Visita nuestro proyecto Crowdin

Para empezar, ve al proyecto de Crowdin [aquí](https://crowdin.com/project/celestia-docs).

Tendrás que crear una cuenta y entonces podrás unirte al proyecto para empezar tu viaje de traducción.

Si no ves tu idioma, no dudes en solicitarlo en el canal `#translations` de Discord [aquí](https://discord.gg/celestiacommunity).

En Crowdin puedes traducir, comentar las traducciones y también dar votos positivos y votos negativos a las traducciones existentes.

¡Da tu opinión sobre las traducciones existentes para asegurarte de que sean correctas!

## Sugerencias

Aquí tienes algunos consejos para ayudarte durante tu traducción.

### Documentación de Crowdin

La documentación oficial de Crowdin está disponible [aquí](https://support.crowdin.com/online-editor).

### Guía

#### Código

Algunas páginas contienen metadatos y código informático.

Es importante tener en cuenta que William Shakespeare era un hablante inglés...¡También lo fue Alan Turing! Por eso no deberías traducir partes del código "en sí mismo".

Por ejemplo, si ves metadatos como `sidebar_label : Hola Mundo`, una traducción al francés sería `sidebar_label : Salut tout le monde`.

Tomemos otro ejemplo, no tendrías que traducir nada aquí:

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

Además, no tienes que traducir las URLs a tu idioma local.

#### Palabras específicas

Como traducirás conceptos innovadores, como la Disponibilidad de Datos muestreo, siéntase libre de discutir acerca de la mejor traducción con el resto de de la comunidad.

También, ten cuidado con el orden de fechas, el periodo y las comas con respecto a los números de un idioma a otro.
