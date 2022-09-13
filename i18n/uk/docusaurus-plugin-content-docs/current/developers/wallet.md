- - -
sidebar_label : Створення Гаманця
- - -

# Гаманець

## Створення гаманця

Спочатку створіть файл конфігурації CLI програми:

```sh
celestia-appd config keyring-backend test
```

Ви можете обрати будь-яку назву гаманця. У нашому прикладі ми використали «validator» як назву гаманця:

```sh
celestia-appd keys add validator
```

Збережіть мнемонічний вихід, оскільки це єдиний спосіб відновити гаманець валідатора, якщо ви його втратите!

Щоб перевірити всі свої гаманці, ви можете запустити:

```sh
celestia-appd keys list
```

## Поповнити гаманець

Для публічної адреси Celestia ви можете поповнити раніше створений гаманець через [Discord](https://discord.gg/celestiacommunity), надіславши це повідомлення на канал #faucet:

```text
$request celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Дочекайтесь, щоб побачити, що токени успішно відправлені. Щоб перевірити, чи токени успішно надійшли до визначеного гаманця, виконайте наведену нижче команду, замінивши публічну адресу на свою власну:

```sh
celestia-appd start
celestia-appd query bank balances celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
