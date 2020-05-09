+++
title = "Construye tu primer 101 sistema de monitorización"
date = "2020-05-09"
tags = [
"monitorización",
"SRE",
]
categories = [
"tecnología",
]
description = ""
draft = false
comments = true
+++

En esta entrada puedes seguir un rápido tutorial sobre como configurar fácilmente tu primer sistema de monitorización usando herramientas como Prometheus, Grafana y Alertmanager. En la primera parte, podrás aprender sobre qué son exactamente estas herramientas. En la segunda parte, te redirigiré a una guía práctica (proyecto de Github) que contiene la documentación necesaria para configurar el sistema de monitorización desde cero en aproximadamente 30 minutos.

# Contenido

- [Teoría: Qué herramientas de monitorización vamos a usar]({{< relref "#que-herramientas-de-monitorizacion-vamos-a-usar" >}})
  - [Prometheus]({{< relref "#prometheus" >}})
    - [Métricas]({{< relref "#metricas" >}})
    - [PromQL]({{< relref "#promql" >}})
  - [Alertmanager]({{< relref "#alertmanager" >}})
  - [Grafana]({{< relref "#grafana" >}})
- [Práctica: Como configurar tu sistema de monitorización]({{< relref "#como-configurar-tu-sistema-de-monitorizacion" >}})
- [Invítame a una caña]({{< relref "#invítame-a-una-caña">}})

## Qué herramientas de monitorización vamos a usar

Hemos elegido un stack tecnológico dentro de muchas posibilidades que tenemos. Lo que queremos conseguir es tener un sistema de monitorización para que seamos capaces de representar métricas además de tener alertas de nuestros servicios. Entonces, podremos conseguir información tanto de la perspectiva de infrastructura como de negocio.

### Prometheus

[Prometheus](https://prometheus.io/docs/introduction/overview/) es un sistema de monitorización y alertas de **código libre**. Él reúne información de servicios y jobs instrumentados. Para instrumentar un servicio, simplemente tenemos que exponer un endpoint HTTP con métricas en el formato de Prometheus. Es entonces cuando usando el servicio de autodiscovery de Prometheus podemos almacenar las métricas recolectada de todos nuestros objetivos. Por otro lado, si tenemos jobs de vida corta, necesitaremos usar el servicio de [Pushgateway](https://prometheus.io/docs/practices/pushing/). En este caso, el servicio de Pushgateway será el responsable de exponer las métricas a los servicios. Aquí puedes ver algunos diagramas de como funciona:

Cómo reunir métricas sobre servicios:
{{< figure src="/technology/prometheus/scrapping.png" title="Prometheus escanea la información de los servicios" >}}

Cómo Prometheus Pushgatway reúne las métricas de los jobs de vida corta:
{{< figure src="/technology/prometheus/scrapping-push-gateway.png" title="Prometheus Gateway recibe las métricas de los jobs de vida corta" >}}

#### Metricas

Una métrica es un dato que:

- Contiene características del sistema.
- Provee una secuencia de valores almacenados a lo largo de un periodo.

Ejemplo de una métrica para un servidor web:

- Número total de peticiones recibidas.

En Prometheus, una métrica está compuesta por:

- Nombre de la métrica: Necesita representar a qué refiere una métrica. Hay un [listado de mejores prácticas disponible](https://prometheus.io/docs/practices/naming/).
- Tipo de métrica: Dependiendo de qué datos queremos representar con nuestra métrica, necesitaremos un tipo de métrica u otra. En general, los tipos de métrica más usados son los contadores (counter), medidores (gauge) y los histogramas (histogram). Echa un vistazo a los [diferentes tipos de métricas disponibles](https://prometheus.io/docs/concepts/metric_types/).
- Etiquetas: Cada métrica tiene unas etiquetas (labels), las cuales añaden dimensiones a la métrica. Para cada nueva etiqueta, un nuevo valor de la métrica se creará. Ten en cuenta que si [una métrica tiene muchas dimensiones](https://www.robustperception.io/cardinality-is-key), esto hará que la consulta de datos sea muy cara.
- Descripción de la métrica: Para proveer más información sobre lo que representa. Esta información no es recolectada por Prometheus, pero la podemos ver cuando consultamos el endpoint de métricas de nuestro servicio.

Aquí puedes ver un diagrama sobre la estructura de una métrica:

{{< figure src="/technology/prometheus/metric.png" title="Estructura de una métrica" >}}

#### PromQL

Prometheus usa su propio lenguaje para consultar los datos: [PromQL](https://prometheus.io/docs/prometheus/latest/querying/basics/). En él, puedes usar:

- [Operadores de agregación](https://prometheus.io/docs/prometheus/latest/querying/operators/#aggregation-operators):
  - sum, min, max, avg, count…
- [Funciones](https://prometheus.io/docs/prometheus/latest/querying/basics/#functions):
  - histogram_quantile(φ float, b instant-vector): calcula el φ-cuantil (0 ≤ φ ≤ 1)
  - rate(v range-vector): media de la tasa de incremento por segundo en una serie temporal dado un rango de vectores.
  - increase(v range-vector): incremento en la serie temporal dado un rango de vectores.
  - ...

### Alertmanager

Alertmanager es un kit de herramientas de Prometheus que podemos usar para manegar alertas a través de reglas simplemente consultado la base de datos de Prometheus y estableciendo unos límites. Alertmanager tiene un simple [sistema de notificación](https://prometheus.io/docs/alerting/configuration/), el cual soporta muchos servicios como Slack y Jira.

{{< figure src="/technology/prometheus/alertmanager.png" title="Alertmanager" >}}

### Grafana

Grafana nos permite consultar, visualizar, alertar y entender nuestras métricas sin importar dónde están almacenadas. Gracias a Grafana, podemos crear, explorar y compartir dashboards consultado nuestras métricas. Puedes echar un vistazo a un ejemplo visual [https://play.grafana.org](https://play.grafana.org).

## Como configurar tu sistema de monitorización

Para construir tu primer sistema de monitorización usando las herramientas mencionadas anteriormente, he creado un projecto de Github qu contiene un detallado pero rápido tutorial acerca de cómo puedes hacerlo [https://github.com/angulito/prometheus-101](https://github.com/angulito/prometheus-101). No dudes en contactarme o abrir una pull request si tienes preguntas o sugerencias.

# Invítame a una caña

¿Te ha gustado este post? ¿Te ha sido de ayuda? ¡Puedes invitarme a una cañeja para ayudarme a digerir mejor el uso de estas nuevas tecnologías!

[![invitame a una cerveza](/img/beer-es.png)](https://www.paypal.me/angulito/2)
