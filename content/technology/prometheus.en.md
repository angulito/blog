+++
title = "Build your first 101 monitoring system"
date = "2020-05-09"
tags = [
    "monitoring",
    "SRE",
]
categories = [
    "technology",
]
description = ""
draft = false
comments = true
+++

In this post you can follow a quick tutorial about how you can easily set up your first monitoring system using tools like Prometheus, Grafana and Alertmanager. On the first part you'll learn about what are exactly these tools and how to use them. On the second part, I'll point out you to my Github project that contains the necessary documentation to set up your first monitoring system from scratch in ~30min.

# Content

- [Theory: What monitoring tools we are going to use]({{< relref "#what-monitoring-tools-we-are-going-to-use" >}})
  - [Prometheus]({{< relref "#prometheus" >}})
    - [Metrics]({{< relref "#metrics" >}})
    - [PromQL]({{< relref "#promql" >}})
  - [Alertmanager]({{< relref "#alertmanager" >}})
  - [Grafana]({{< relref "#grafana" >}})
- [Practice: How to setup your monitoring system]({{< relref "#how-to-setup-your-monitoring-system" >}})
- [Buy me to a beer]({{< relref "#buy-me-a-beer">}})

## What monitoring tools we are going to use

The following tech stack is one of a lot of possibilities that you currently have. What we want to achieve is having a monitoring system and be able to represent metrics while having alerts of our services. We can gather information from both an infrastructure and business perspective.

### Prometheus

[Prometheus](https://prometheus.io/docs/introduction/overview/) is an **open-source** monitoring and alerting system. It gathers information from instrumented services and jobs. In order to instrument a service just expose an HTTP endpoint with metrics in Prometheus format. Then using Prometheus service discovery, we will be able to discover and automatically scrape and ingest the metrics. On the other hand, if we have a short-lived job, we will need to use the [Pushgateway](https://prometheus.io/docs/practices/pushing/) service. The push gateway service is responsible to expose the metrics to Prometheus for scraping. Here you have some diagrams about how it works:

How to scrape services:
{{< figure src="/technology/prometheus/scrapping.png" title="Prometheus scrapes a service" >}}

How to scrape prometheus Pushgateway as it contains short-lived jobs metrics:
{{< figure src="/technology/prometheus/scrapping-push-gateway.png" title="Prometheus Gateway for short-lived jobs" >}}

#### Metrics

A metric is a data value that:

- Contains system characteristics.
- Provides a sequence of values recorded over time.

Example of a metric for a web server:

- Total set of requests received.

In Prometheus, a metric is composed by:

- Metric name: It needs to represent what the metric refers to. There is a set of [best practices for naming metrics that are available](https://prometheus.io/docs/practices/naming/).
- Metric type: Depending on what data we want to represent with our metric, we will need to use one or another metric type. The most used metric types are counters, gauges and histograms. Take a look at the [different types that are available](https://prometheus.io/docs/concepts/metric_types/).
- Labels: Each metric can have labels, which adds dimensions to the metric. For each new label value, a new metric value will be created. Keep in mind, if [a metric has tons of dimensions](https://www.robustperception.io/cardinality-is-key) then querying that data will be expensive.
- Metric description: In order to provide more information about what they represent. This information is not collected by Prometheus, but we can see it when hitting the metrics endpoint for our service.

Here you have a diagram about the metric structure:

{{< figure src="/technology/prometheus/metric.png" title="Metric structure" >}}

#### PromQL

Prometheus uses its own query language: [PromQL](https://prometheus.io/docs/prometheus/latest/querying/basics/). There you can use:

- [Aggregation operators](https://prometheus.io/docs/prometheus/latest/querying/operators/#aggregation-operators):
  - sum, min, max, avg, count…
- [Functions](https://prometheus.io/docs/prometheus/latest/querying/basics/#functions):
  - histogram_quantile(φ float, b instant-vector): compute φ-cuantil (0 ≤ φ ≤ 1)
  - rate(v range-vector): mean of the rate increase per second in the time series in a vector range
  - increase(v range-vector): increase in the time series in a vector range.
  - ...

### Alertmanager

Alertmanager is a Prometheus’ toolkit that we can use to handle alerts through rules, just querying prometheus database setting thresholds. Alertmanager has an easy [notification system](https://prometheus.io/docs/alerting/configuration/), supporting a lot of services like Slack and Jira.

{{< figure src="/technology/prometheus/alertmanager.png" title="Alertmanager" >}}

### Grafana

Grafana allows us to query, visualize, alert and understand our metrics no matter where they are stored. Thanks to Grafana, we can create, explore, and share dashboards querying our Prometheus metrics. You can look at a GUI example at [https://play.grafana.org](https://play.grafana.org).

## How to setup your monitoring system

In order to build your first monitoring system using these mentioned tools above, I created a Github project that contains a detailed but quick tutorial about how you can do it at [https://github.com/angulito/prometheus-101](https://github.com/angulito/prometheus-101). Don't hesitate to contact me or open a new pull request in case you have any question for me.

# Buy me a beer

Do you like this post? Is it helpful for you? You can buy me a beer to help me to digest better the usage of these new technologies!

[![buy me a beer](/img/beer.png)](https://www.paypal.me/angulito/2)
