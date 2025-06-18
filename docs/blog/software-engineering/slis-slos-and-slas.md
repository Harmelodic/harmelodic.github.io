# SLIs, SLOs and SLAs

This document is intended to cover the fundamentals for those starting out with SLIs, SLOs and SLAs.

The Google SRE documentation does a much more in-depth job of
coving [Service Level Objectives](https://sre.google/sre-book/service-level-objectives/).

## Concepts

When providing services for use, they likely won't be functional and available 100% of the time. Therefore, it's useful
to measure the "level of service" currently provided, set an objective/goals relating to that, and maybe even define an
agreement with someone on what "level of service" you will offer.

### SLIs

SLIs are _Service Level Indicators_. These are the actual metrics that you measure to define your level of service.
Typically, an SLI will measure your Availability for you service, but you can also consider performance-based metrics
that measure "Latency" or "Throughput" depending on the service. The SLI that you select should reflect aspect of the
level of service desired, and not what metrics you happen to have available. How you measure these metrics, and how
often should be part of the definition of the SLI.

Typically, I like to define SLIs as:

> "Thing": Measured by "timeseries metric", evaluated over a "time period".

For example, an SLI for an HTTP-based web service could be:

> Availability: Measured by the number of well-formed requests that succeed, evaluated every minute.

This way, we capture: (a) the semantic meaning of the SLI, (b) how we measure it, and (c) how often it is evaluated.
This clarity makes it easier to implement the indicator and set associated SLOs.

**Tip**: Try to be smart with your SLIs. The consumers of your service likely only care about the level of service _when
they are actively using your service_. By taking advantage of this, you can design good SLIs that tailor themselves to
the actual needs of your consumer and grant you opportunities to potentially disrupt your service but not your SLI.

### SLOs

SLOs are _Service Level Objectives_. These are the expectations that we set for ourselves based using the SLIs we've
already defined. SLOs can also contain *qualifiers* which can give further scope or context about when the SLO
is applicable, such as only being applicable during a specific time window.

For Availability SLOs, this is typically defined in "nines", where "3 nines" is the same as 99.9% availability (because
there are 3 nines in the percentage).

For performance-based SLOs, this is typically a threshold that is acceptable for the given SLO. For example, for a
latency SLI, the threshold could be 300ms.

Since the SLI defines how often

SLOs are not targets or goals for us to achieve _eventually_, but the expectations of _right now_. As such, not
achieving an SLO should be treated as a serious incident and prioritised accordingly.

**Tip**: When introducing an SLO for the first time, start by setting the SLO to something that is already being
achieved, then plan and develop improvements to the level of service, then change the SLO to reflect new expectations.
Setting SLOs too aggressively in the beginning is _literally_ setting unfair expectations and leads to a reactionary way
of working that is unpleasant.

### SLAs

SLAs are _Service Level Agreements_. These are contracts between you and others on what level of service you will offer.

SLAs are usually made up one or multiple SLOs (that could be more lenient than other internally-used SLOs and/or more
tailored to that a specific consumers needs) and detail the consequences when SLOs are not achieved. These consequences
are often financial.

## Example implementations

### Availability

Availability is an operational factor in assessing levels of service.

You could think of availability as simply being "uptime" of your service but that massively limits your ability to
disrupt your service, since any disruption counts against your availability, even if no consumers of your service were
affected. Instead, we can be more selective and consumer-focused when thinking about availability and define it by how
much consumers were actually affected.

Our SLI for availability can be defined as:

> Availability: Measured by the percentage of well-formed requests that succeed, evaluated every minute.

This means that our availability is only affected when well-formed requests do not succeed. Ill-formed requests that
don't succeed don't count against our availability, and neither does our service being offline for any period of time
when no well-formed requests are being sent.  
Evaluating every minute also means that if we have variable usage patterns of our service, we're not

We can define two SLOs could be:

> 99.99% availability at all times.

An SLA with a consumer of our service could contain a separate SLO that is more targeted to a specific consumer's needs:

> 99.9% availability between the hours of 09:00 and 17:00 UTC, on weekdays.
> When this SLO is not met, the service provider will be required to refund 1 month's subscription cost of the offered
> service.

### Latency

Latency is a performance factor in assessing levels of service.

We could measure average (mean or median) latency, but latency is difficult to broadly measure, as different service
offerings are subject to different factors that can affect latency (e.g. amount of work that needs to be completed, or
variability in a query parameters). This means that we typically want to exclude the requests with the longest latency
from being included in our measurements, whilst still capturing most of the consumers latency needs.

This is typically done by measuring the [percentile](https://en.wikipedia.org/wiki/Percentile) in a distribution. For
example: the 99th percentile (or P99) - which could be the latency that the bottom 99% of requests achieve (effectively
excluding the top 1% of requests that have the longest latencies).

Our SLI for latency can be defined as:

> Latency: Measured by the 99th percentile of requests, evaluated every 1 minute.

This means that all requests during a single minute are gathered and each requests latency forms a distribution, from
shortest to longest latency, and we measure the slowest latency of the fastest 99% (excluding the slowest 1%).

Our SLO for latency is then the threshold for what we deem as acceptable latency for example:

> 100ms of latency.

An SLA with a consumer of our service could then contain:

> 200ms of latency, during the hours of 09:00 and 17:00 UTC, weekdays.
> When this SLO is not met, the service provider will be required to refund 1 month's subscription cost of the offered
> service.

### Performant periods

Performant periods is a performance factor in assessing levels of service. It could be seen as an alternative (or
supplement) in measuring service performance to measuring Latency.

Sometimes you might want to capture latency as part of metric, but not make it the metric itself. This is the case
with "performant periods", where we want to count the number of periods (e.g. minutes, hours) during a larger time
window (e.g. days, weeks), where service was deemed "performant" - where performance is measured by latency (or a
percentile of latency) is under a specific threshold (e.g. 100ms).

This can give a more "time-based" view of performance rather than a "request-based" view, which can be useful if we want
to expect a more consistent performance across _time_, rather than a consistent performance across all requests -
effectively discounting or minimising any fluctuations in request _rate_ caused by heavy or light load on the system
from affecting our view of "service performance".

An SLI for performant periods could be:

> Performant minutes: Measured by the fraction of minutes where P99 latency is under 100ms, evaluated every day.

This means all requests during a single minute are gathered and each requests latency forms a distribution, from
shortest to longest latency, and we measure the slowest latency of the fastest 99% (excluding the slowest 1%). If that
latency is less than 100ms, we state that this minute was "performant". The fraction of performant minutes every day is
how "performant" our service has been during that day.

An SLO for this SLI could then be:

> 1400 of 1440 performant minutes.

An SLA with a consumer

> 475 of 480 performant periods, during the hours of 09:00 and 17:00 UTC, weekdays.
> When this SLO is not met, the service provider will be required to refund 1 month's subscription cost of the offered
> service.

## Further reading

As mentioned, the Google SRE documentation does a much more in-depth job of
coving [Service Level Objectives](https://sre.google/sre-book/service-level-objectives/).
