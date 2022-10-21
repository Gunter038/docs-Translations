---
sidebar_label: Türler
---

# Kelime Türleri

Sonraki adımlar için oluşturduğumuz mesajlar tarafından kullanılacak türler oluşturacağız.

## Yapı Kelime Türleri

```sh
ignite scaffold map wordle word submitter --no-message
```

Bu tür, `word`  ve `submitter` olmak üzere iki değeri olan  `Wordle`  adlı bir haritadır. `submitter`, Kelimeyi (Wordle) gönderen kişinin adresidir.

İkinci tip ise  `Guess` (tahmin) tipidir. Çözüm gönderen her adres için en son tahmini saklamamızı sağlar.

```sh
ignite scaffold map guess word submitter count --no-message
```

Burada, bu adresin kaç tane tahmin gönderdiğini saymak için `count` u'da depoluyoruz.
