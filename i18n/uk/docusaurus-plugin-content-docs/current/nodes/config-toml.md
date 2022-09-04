- - -
sidebar_label : Config.toml Guide
- - -

# Аналіз Config.toml

- [Аналіз Config.toml](#configtoml-breakdown)
  - [Передумови](#pre-requisites)
  - [Розуміння config.toml](#understanding-configtoml)
    - [[Core]](#core)
    - [[P2P]](#p2p)
      - [Bootstrap](#bootstrap)
      - [Взаємні однорангові ноди](#mutual-peers)
    - [[Сервіси]](#services)
      - [TrustedHash і TrustedPeer](#trustedhash-and-trustedpeer)

## Передумови

Будь ласка, переконайтеся, що ви встановили та ініціалізували ноду Celestia

## Розуміння config.toml

Після ініціалізації для будь-якого типу ноди ви знайдете `config.toml` за таким шляхом (розташування за замовчуванням):

- `$HOME/.celestia-bridge/config.toml` for bridge node
- `$HOME/.celestia-light/config.toml` for light node

Проаналізуймо секції, які використовують найчастіше.

### [Core]

Цей розділ потрібен для мостової ноди Celestia. За замовчуванням `Remote = false`. Що стосується девнету, ми збираємося використовувати опцію віддаленого ядра, і це також можна встановити за допомогою прапорця командного рядка `--core.remote`.

### [P2P]

#### Bootstrap

Bootstrap допомагає новим нодам швидше знаходити однорангові ноди в мережі. За замовчуванням `Bootstrapper = false`, а `BootstrapPeers` порожній. Якщо ви хочете, щоб ваша нода була завантажувачем, активуйте `Bootstrapper = true`. `BootstrapPeers` вже надаються за замовчуванням під час ініціалізації. Якщо ви хочете додати свій власний вручну, вам потрібно надати кілька адрес однорангових нод.

#### Взаємні однорангові ноди

Метою цієї конфігурації є встановлення двонапрямного зв’язку. Зазвичай це стосується мостових нод Celestia. In addition, you need to change the field `PeerExchange` from false to true.

### [Services]

#### TrustedHash and TrustedPeer

`TrustedHash` is needed to properly initialize a Celestia Bridge Node with an already-running `Remote` Celestia Core node. Celestia Light Node will take a genesis hash as the trusted one, if no hash is manually provided during initialization phase.

`TrustedPeers` is the array of Bridge Nodes' peers that Celestia Light Node trusts. By default, bootstrap peers becomes trusted peers for Celestia Light Nodes if a user is not setting the trusted peer params in config file.

Any Celestia Bridge Node can be a trusted peer for the Light one. However, the Light node by design can not be a trusted peer for another Light Node.
