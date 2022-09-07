- - -
бічна панель_мітка: Переклади  документів
- - -

# Підтримка спільноти перекладу

Якщо ви є частіною  спільноти Селестія , та хотили б зробити внесок для перекладу сторінки з документацією-  тоді це керівництво для вас.

## Відвідайте наш проект Crowdin

Перейдіть до проекту Crowdin [тут](https://crowdin.com/project/celestia-docs).

Вам потрібно створити обліковий запис, а потім ви зможете приєднатися до проекту для того, щоб почати подорож по перекладам.

Якщо ви не бачите своєї мови, ви можете попросити про неї в діскорді проекту, [тут](https://discord.gg/celestiacommunity).

В Crowdin ви можете перекладати, коментувати переклади, та також віддавати голоси і знижувати голоси за переклади.

Дайте свою думку щодо існуючих перекладів, щоб переконатися, що вона правильна!

## Рекомендації

Ось кілька порад, які допоможуть вам у вашому перекладі.

### Документація Crowdin

Офіційний документ Crowdin доступний в огляді [тут](https://support.crowdin.com/online-editor).

### Інструкція

#### Код

На деяких сторінках містяться метадані та комп'ютерний код.

It is important to keep in mind that William Shakespeare was an English speaker...So was Alan Turing! That is why you should not translate parts of the code "itself".

For instance, if you see metadata like `sidebar_label : Hello World`, a French translation would be `sidebar_label : Salut tout le monde`.

Let's take another example, you wouldn't have to translate anything here:

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

Крім того, вам не потрібно перекладати URL на вашу локальну мову.

#### Специфічні слова

As you will translate innovative concepts, like Data Availability Sampling, feel free to discuss about the best translation with the rest of the community.

Also, be careful with date order, period and commas regarding numbers from a language to another.
