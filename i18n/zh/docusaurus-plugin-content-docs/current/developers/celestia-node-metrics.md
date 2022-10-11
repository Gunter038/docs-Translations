# Celestia节点指标

本教程用于运行celestia节点数据的度量可用性实例。

本教程将重点介绍如何运行轻节点的度量。

本教程假设您已经设置了轻节点 按照[节点API教程中的教程](./node-tutorial.md)进行操作。

## 运行指标标志

可以使用以下命令启用celestia节点度量标志 命令：

<!-- markdownlint-disable MD013 -->
```sh
celestia轻节点 启动——核心。ip<ip-address>-核心.grpc。端口<port>--指标--指标。端点<ip-address:port>
```
<!-- markdownlint-enable MD013 -->

请注意，`--metrics`标志启用度量，并期望 输入到`--metrics.端点`中。

我们将在下一节中讨论端点需要是什么。

## 指标端点设计注意事项

目前，celestia节点的架构指标 按照以下[ ADR](https://github.com/celestiaorg/celestia-node/blob/main/docs/adr/adr-010-incentivized-testnet-monitoring.md)中的规定工作。

基本上，这里的设计考虑是必要的 运行连接到Celestia的OpenTelemetry (OTEL) 采集器轻节点。

有关OTEL的概述，请查看[此处](https://opentelemetry.io/docs/collector/)指南。

ADR和OTEL文档将帮助您在度量端点上运行收集器 这将允许您在数据收集者的进程中也可以在Grafana仪表板上查看Prometheus服务器。

未来，我们确实想开源一些开发工具 此基础架构允许节点操作员能够监视 其数据可用性节点。
