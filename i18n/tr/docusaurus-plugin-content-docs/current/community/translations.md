- - -
topluluk çeviri desteği
- - -

# Topluluk Çeviri Desteği

Eğer dökümantasyon sayfasının çevrilmesine katkıda bulunmak isteyen tutkulu bir Celestia topluluğu üyesiyseniz, bu yazı sizin için bir kılavuzdur.

## Crowdin Projemizi Ziyaret Edin

Başlamak için Crowdin projesine [buradan](https://crowdin.com/project/celestia-docs) ulaşabilirsiniz.

Çeviri yolculuğunuza başlamak için bir hesap oluşturmanız gerekecek ve ardından projeye katılabileceksiniz.

Dilinizi göremiyorsanız, <https://discord. gg/8ssKPmAJG9>discord</a> daki #translations kanalından dilinizi istemekten çekinmeyin.

Crowdin'de çeviri yapabilir, çevirilere yorum yapabilir ve hem de mevcut çevirilere olumlu ve olumsuz oy verebilirsiniz.

Doğru olduğundan emin olmak için mevcut çeviriler hakkında görüşlerinizi bildirin!

## İpuçları

İşte çeviriniz sırasında size yardımcı olacak birkaç ipucu.

### Crowdin dokümantasyonu

Resmi Crowdin dökümantasyonları [burada](https://support.crowdin.com/online-editor) mevcuttur.

### Kılavuz

#### Kod

Bazı sayfalar meta veriler ve bilgisayar kodu içerir.

William Shakespeare'in bir yazar olduğunu akılda tutmak önemlidir. İngilizce konuşan... Alan Turing de öyleydi! Bu yüzden "kendi" kodunun bazı kısımlarını çevirmemelisiniz.

Mesela, sidebar_label : Hello World, gibi metaveri görürseniz Fransızca çevirisi sidebar_label : Salut tout le monde olacaktır.

Başka bir örnek verelim, burada hiçbir şeyi tercüme etmenize gerek yok:

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

Ayrıca, URL'leri yerel dilinize çevirmeniz gerekmez.

#### Belirli kelimeler

Veri Kullanılabilirliği Örneklemesi gibi yenilikçi kavramları çevireceğiniz için, topluluğun geri kalanıyla en iyi çeviri hakkında tartışmaktan çekinmeyin.

Ayrıca, bir dilden diğerine sayılara ilişkin tarih sırasına, döneme ve virgül işaretlerine dikkat edin.
