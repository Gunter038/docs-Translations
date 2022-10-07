# Celestia Düğüm Metrikleri

Bu eğitim Celestia düğümü Veri Kullanılabilirliği oluşumunuz (celestia-node Data Availability instance) için metrikleri çalıştırmak içindir.

Bu eğitim, light-node için çalışan metriklere odaklanacaktır.

Bu eğitim light node kurulumunu [Node API eğitimi](./node-tutorial.md) ile çoktan yaptığınızı varsayar.

## Metrik bayraklarını çalıştırma

Celestia düğümü metrik bayraklarını aşağıdaki komutla etkinleştirebilirsiniz:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --metrics --metrics.endpoint <ip-address:port>
```
<!-- markdownlint-enable MD013 -->

`--metrics` işaretlerinin metrikleri etkinleştirdiğini ve `--metrics.endpoint` bir girdi beklediğini unutmayın.

Endpoint ne olması gerektiğini aşağıdaki bölümde ele alacağız.

## Metrik endpoint tasarımında dikkat edilecek noktalar

Şu anda, celestia-node metriklerinin mimarisi, aşağıdaki [ADR](https://github.com/celestiaorg/celestia-node/blob/main/docs/adr/adr-010-incentivized-testnet-monitoring.md) belirtildiği gibi çalışmaktadır.

Esasen, buradaki tasarım hususları, Celestia'ya bağlanan bir OpenTelemetry (OTEL) toplayıcısının çalıştırılmasını gerektirecektir. Light Node.

OTEL'e genel bir bakış için [buradaki](https://opentelemetry.io/docs/collector/) kılavuza bakın.

ADR ve OTEL dokümanları, toplayıcınızı metrik endpoint'de çalıştırmanıza yardımcı olacaktır. Bu daha sonra toplayıcıdaki verileri bir Prometheus sunucusunda işlemenize ve ardından Grafana panosunda görüntülenebilmenize olanak tanır.

Gelecekte, düğüm operatörlerinin Veri Kullanılabilirliği düğümlerini izleyebilmelerini olanak sağlamak için bu altyapı etrafında bazı geliştirici araçlarını açık kaynaklı hale getirmek istiyoruz.
