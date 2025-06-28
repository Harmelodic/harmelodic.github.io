# Observability

When running applications, we want to _observe_ how the operations of those applications are going. We need data in
order achieve this. Typically, there are 3 different observability data types or "signals", each with their own use and
methods of gathering that data:

- Logs
- Metrics
- Traces

[OpenTelemetry (OTel)](https://opentelemetry.io/) has appeared as a set of APIs, SDKs and tools to standardise and make
it easier to handle observability.

Each type of signal needs to go through the following process:

1. Instrumentation - configuring an application to produce signals.
2. Collection - collecting signals, transforming and enriching them (if desired), and sending them to storage.
3. Storage - storing signals
4. Usage - viewing and/or triggering things based on signals (e.g. alerts)

## Logs

Logs contain information about what is happening within your application at particular times. They are mainly useful for
troubleshooting.

Logs are typically **instrumented** via:

- Using logging "facade" to provide a single way for application code to produce logs (e.g. `SLF4J` for Java, `slog` for
  Go)
- Formatting logs into the format desired, such as plain text or JSON. Logs should ideally be _structured_ to allow easy
  filtering and querying of logs when viewing them - JSON formatting is recommended to structure logs.
- Outputting logs to either the console (aka "standard out") or to a file, or directly to log storage.

Logs are **collected** using a log collector such as Fluentd or Logstash.

Logs are **stored** using log storage systems such as Elasticsearch or GCP Logging.

Logs are **used** through a log viewer, usually tied to the log storage platform, such as Kibana or GCP Log Explorer.

## Metrics

Metrics contain numeric values about the operations of your application, at particular times. They are mainly useful for
alerting (when values are not as they should be) or graphing to aid in troubleshooting.

Metrics typically come in 4 different types (especially when working with Prometheus):

- Counter - an increasing numeric value
- Gauge - a numeric value that can go up or down
- Histogram - sample observations counted and stored in "buckets" (can calculate quantiles when querying)
- Summary - sample observations counted with calculated quantiles

Metrics are typically instrumented via:

- A metrics instrumentation library to produce and store metrics in an in-memory registry (e.g. Micrometer for Java)
- An application provides an endpoint (e.g. HTTP endpoint) for accessing that registry.

Metrics are **collected** using a metrics collector that scrapes application endpoints, such as the prometheus
collector.

Metrics are **stored** in a time-series metrics storage, such as Prometheus.

Metrics are **used** in graphing/visualisation tools like Grafana, and in alerting tools like Alert Manager.

## Tracing

Traces are paths taken through systems. A trace is made up of multiple "Spans", which is data about how long a
particular process took. Traces are useful for analysing performance (seeing which spans take the longest in a trace)
and aid troubleshooting by showing the paths taken through the systems.

Traces can be made up of spans from 1 or more systems, making them useful for understanding paths through distributed
systems.

Traces are usually sampled to make tracing storage & collection more cost-effective.

Traces are **instrumented** using an instrumentation library, often provided by OpenTelemetry (historically this might
have been Zipkin or OpenTracing) and _actively sending_ them to a collector.

Traces are **collected** using a trace collector, such as the OpenTelemetry collector.

Traces are **stored** in a tracing storage solution, such as Jaeger or GCP Cloud Trace.

Traces are **used** by viewing visualised traces in a UI, usually tied to the tracing storage solution, such as Jaeger
UI or GCP Cloud Trace Viewer.
