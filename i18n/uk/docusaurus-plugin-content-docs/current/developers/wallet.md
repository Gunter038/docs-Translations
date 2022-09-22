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

For the public celestia address, you can fund the previously created wallet via [Discord](https://discord.gg/celestiacommunity) by sending this message to #arabica-faucet channel:

```text
$request celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Дочекайтесь, щоб побачити, що токени успішно відправлені. Щоб перевірити, чи токени успішно надійшли до визначеного гаманця, виконайте наведену нижче команду, замінивши публічну адресу на свою власну:

```sh
celestia-appd start
celestia-appd query bank balances celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
