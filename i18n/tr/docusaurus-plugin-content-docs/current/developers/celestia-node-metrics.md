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

Essentially, the design considerations here will necessitate running an OpenTelemetry (OTEL) collector that connects to Celestia Light Node.

For an overview of OTEL, check out the guide [here](https://opentelemetry.io/docs/collector/).

The ADR and the OTEL docs will help you run your collector on the metrics endpoint. This will then allow you to process the data in the collector on a Prometheus server which can then be viewed on a Grafana dashboard.

In the future, we do want to open-source some developer toolings around this infrastructure to allow for node operators to be able to monitor their Data Availability nodes.
