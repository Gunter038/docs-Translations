- - -
celestia node kurulumu
- - -

# Celestia Node

Bu rehber, celestia-node'u oluşturma ve kurma konusunu ele almaktadır. [Burada](./environment.md) bu eğitim, içinde bulunan şartlarda kurma adımlarını tamamladığınızı varsayar.

## Celestia Node Kurulumu

### Arabica Kurulumu

Arabica Devnet için celestia-node kurulumu, ağ ile uyumlu olacak belirli bir sürümün yüklenmesi anlamına gelir.

Aşağıdaki komutları çalıştırarak celestia node Binary'i yükleyin:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
```

Binary'inin çalıştığını doğrulayın ve sürümü `celestia
version` komutuyla kontrol edin:

```console
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

### Mamaki kurulumu

Mamaki Testnet için celestia-node kurulumu, ağ ile uyumlu olacak belirli bir sürümün yüklenmesi anlamına gelir.

Aşağıdaki komutları çalıştırarak celestia düğümü Binary'ileri yükleyin:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Binary'inin çalıştığını doğrulayın ve sürümü `celestia
version` komutuyla kontrol edin:

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Network Selection

You can perform network selection in celestia-node between Arabica and Mamaki. However, you should note that networks work best on the celestia-node versions mentioned above.

```sh
celestia light init
celestia light start --node.network arabica
```
