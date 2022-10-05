---
sidebar_label: Arabica Geliştirici Ağı
---

# Arabica Devnet
<!-- markdownlint-disable MD013 -->

![arabica-devnet](/img/arabica-devnet.png)

Arabica Geliştirici Ağı, Celestia Labs tarafından yalnızca geliştiricilere yönelik olarak sunulan, üstün performans ve en son güncellemeleri içeren, toplamaların ve uygulamaların test edilmesini sağlayan test ağıdır.

Arabica, doğrulayıcı veya mutabakat seviyesinde testlere odaklanmaz, bunun için Mamaki Test Ağı kullanılmaktadır. Eğer bir doğrulayıcıysanız, doğrulayıcı işlemlerinizi Mamaki üzerinde [buradan](./mamaki-testnet.md) denemenizi öneririz.

Arabica, üzerine kurulu tüm Celestia ürünlerinin en son güncellemelerini aldığından, birçok değişikliğe tabi olabilir. Bu nedenle, önceden uyarıyoruz, Arabica beklenmedik bir şekilde bozulabilir, ancak sürekli güncelleneceği göz önünde bulundurulduğunda, yazılımdaki en son değişiklikleri test etmeye devam etmenin yararlı bir yoludur.

Geliştiriciler, buna rağmen dilerlerse bağımsız toplamalarını Mamaki Test Ağı'na kurabilirler. Ancak bunu yapmayı seçtikleri takdirde bilmedilirler ki Mamaki, doğrulayıcılarla koordineli şekilde Sert Çatallanma (zorunlu) Güncellemeleri gerçekleşene kadar her zaman için Arabica Test Ağı'nın gerisinden gelecektir.

## Entegrasyonlar

This guide contains the relevant sections for how to connect to Arabica, depending on the type of node you are running.

Your best approach to participating is to first determine which node you would like to run. Each node guides will link to the relevant network in order to show you how to connect to them.

You have a list of options on the type of nodes you can run in order to participate in Arabica:

Data Availability:

* [Bridge Node](./bridge-node.md)
* [Full Storage Node](./full-storage-node.md)
* [Light Node](./light-node.md)

Select the type of node you would like to run and follow the instructions on each respective page. Whenever you are asked to select the type of network you want to connect to in those guides, select `Arabica` in order to refer to the correct instructions on this page on how to connect to Arabica.

## RPC endpoints

There is a list of RPC endpoints you can use to connect to Arabica Devnet:

* [https://limani.celestia-devops.dev](https://limani.celestia-devops.dev)

## Arabica Devnet faucet

> USING THIS FAUCET DOES NOT ENTITLE YOU TO ANY AIRDROP OR OTHER DISTRIBUTION OF MAINNET CELESTIA TOKENS. MAINNET CELESTIA TOKENS DO NOT CURRENTLY EXIST AND THERE ARE NO PUBLIC SALES OR OTHER PUBLIC DISTRIBUTIONS OF ANY MAINNET CELESTIA TOKENS.

You can request from Arabica Devnet Faucet on the #arabica-faucet channel on Celestia's Discord server with the following command:

```text
$request <CELESTIA-ADDRESS>
```

Where `<CELESTIA-ADDRESS>` is a `celestia1******` generated address.

> Note: Faucet has a limit of 10 tokens per week per address/Discord ID

## Explorers

There is an explorer you can use for Arabica:

* [https://explorer.celestia.observer/arabica](https://explorer.celestia.observer/arabica)
