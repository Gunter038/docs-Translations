# Métricas del nodo Celestia

Este tutorial es para ejecutar métricas para tus datos de instancias de nodos celestia Data Availability.

Este tutorial se centrará en ejecutar métricas para un light-node.

Este tutorial asume que ya has configurado tu light node siguiendo el [tutorial API de Nodos](./node-tutorial.md).

## Indicadores de métricas en ejecución

Puedes activar los parámetros métricos de celestia-node con el siguiente comando:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --metrics --metrics.endpoint <ip-address:port>
```
<!-- markdownlint-enable MD013 -->

Ten en cuenta que los parámetros`--metrics` permiten métricas y esperan una entrada en `--metrics.endpoint`.

Examinaremos cuál será el endpoint en la siguiente sección.

## Consideraciones de diseño de métricas endpoint

Por el momento, la arquitectura de métricas de los nodos Celestia funciona como se especifica en el siguiente [ADR](https://github.com/celestiaorg/celestia-node/blob/main/docs/adr/adr-010-incentivized-testnet-monitoring.md).

Esencialmente, las consideraciones de diseño aquí necesitarán ejecutar un recolector OpenTelemetry (OTEL) que se conecta a Celestia Light Node.

Para una visión general de OTEL, revisa la guía [aquí](https://opentelemetry.io/docs/collector/).

La documentación de ADR y OTEL te ayudará a ejecutar tu recolector en el endpoint de las métricas. Esto te permitirá procesar los datos en el recolector de un servidor Prometheus que puede ser visto en un panel de control de Grafana.

En el futuro, queremos abrir el código fuente de algunas herramientas de desarrollo alrededor de esta infraestructura para permitir que los operadores de nodos puedan monitorear sus nodos Data Availability.
