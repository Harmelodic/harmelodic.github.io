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

The SLI that you select should reflect aspect of the level of service desired, and not what metrics you happen to have
available.

An SLI should have a name reflecting the semantic meaning of the SLI, a
defined [gauge](https://prometheus.io/docs/concepts/metric_types/#gauge) to state how the SLI is measured, and how often
that gauge is evaluated. This clarity in definition makes it easier to implement the indicator and set
associated [SLOs](#slos):

> `Thing`: Measured by `gauge`, evaluated every `time_interval`.

For example, an SLI for an HTTP-based web service could be:

> Availability: Measured by the percentage of well-formed requests that succeed, evaluated every minute.

**Tip**: Try to be smart with your SLIs. The consumers of your service likely only care about the level of service _when
they are actively using your service_. By taking advantage of this, you can design good SLIs that tailor themselves to
the actual needs of your consumer and grant you opportunities to potentially disrupt your service but not your SLI.

### SLOs

SLOs are _Service Level Objectives_. These are the minimum expectations that we set for ourselves based using the SLIs
we've already defined. In practice, it is simply a value on the SLI gauge.

SLOs can also contain *qualifiers* which can give further scope or context about when the SLO is applicable, such as
only being applicable during a specific time window (the duration of which must be greater than or equal to the
evaluation interval used by the SLI).

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

## Examples

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

> 99.99% (aka 4 nines) availability at all times.

An SLA with a consumer of our service could contain a separate SLO that is more targeted to a specific consumer's needs:

> 99.9% (aka 3 nines) availability between the hours of 09:00 and 17:00 UTC, on weekdays.  
> When this SLO is not met, the service provider will be required to refund 1 month's subscription cost of the offered
> service.

### Responsiveness (latency)

Responsiveness is a performance factor in assessing levels of service, usually measured using latency.

We could measure average (mean or median) latency, but latency is difficult to broadly measure, as different service
offerings are subject to different factors that can affect latency (e.g. amount of work that needs to be completed, or
variability in a query parameters). This means that we typically want to exclude the requests with the longest latency
from being included in our measurements, whilst still capturing most of the consumers latency needs.

This is typically done by measuring the [percentile](https://en.wikipedia.org/wiki/Percentile) in a distribution. For
example: the 99th percentile (or P99) - which could be the latency that the bottom 99% of requests achieved
(effectively excluding the top 1% of requests that have the longest latencies) over a specific time period.

Our SLI for latency can be defined as:

> Responsiveness: Measured by the 99th percentile latency of response latencies in the last minute, evaluated every 1
> minute.

This means that all response latencies during a single minute are gathered and form a distribution, from shortest to
longest latency, and we measure the slowest latency of the fastest 99% (i.e. excluding the slowest 1%).

Our SLO for responsiveness is then the threshold for what we deem as acceptable latency for example:

> 100ms of latency.

An SLA with a consumer of our service could then contain:

> 200ms of latency, during the hours of 09:00 and 17:00 UTC, weekdays.  
> When this SLO is not met, the service provider will be required to refund 1 month's subscription cost of the offered
> service.

### Responsiveness Achievability (performant periods)

Sometimes we might not care too much about minute-by-minute responsiveness, but just know how well we achieved that
responsiveness over a longer period of time. It's effectively an SLI tracking an SLO for responsiveness (though you
might not officially/publicly/contractually have that SLO).

This is achieved through measuring "performant periods", where we assess the fraction of periods (e.g. minutes, hours)
in a larger time window (e.g. days, weeks), where service was deemed "performant" - where performance could be assessed
if latency (or a percentile of latency) is under a specific threshold (e.g. 100ms).

An SLI for _Responsiveness Achievability_ using performant periods could be:

> Responsiveness Achievability: Measured by the fraction of performant minutes (where P99 latency is under 100ms),
> evaluated every day (every 1440 minutes).

This means all response latencies during a single minute are gathered and form a distribution, from shortest to longest
latency, and we measure the slowest latency of the fastest 99% (excluding the slowest 1%). If that latency is less than
100ms, we state that this minute was "performant". The fraction of performant minutes every day is how "performant" our
service has been during that day.

SLOs for this SLI are set to the fraction of performant periods required for a service to be deemed "responsive enough"
across the evaluation period. Therefore, an SLO for this SLI could then be:

> 1400 (of 1440) performant minutes.

An SLA with a consumer could be:

> 1300 (of 1440) performant periods, on weekdays.  
> When this SLO is not met, the service provider will be required to refund 1 month's subscription cost of the offered
> service.

## Error Budgets

Error budgets as a concept is effectively "how much of a budget do I have to cause errors" or... "how much can we break
things, and everything still OK according to our SLI/SLO/SLAs".

Error budgets can appear in SLIs and SLOs - and so cannot be tied specifically to any _one_ of these concepts.

**In an SLI**, an error budget can appear when our SLI measures a subset of a dataset. For example, if we measure
latency by using the 99th percentile of response latencies over a single minute, we've effectively created an error
budget to allow 1% of response latencies in a single minute to take any amount of time (up to infinity). Not all SLIs
will have error budgets though, since it all depends on how you define your SLI.

**In an SLO**, an error budget appears as soon as we set our objective. For example, if we target 99.9% availability,
we've effectively created a 0.1% error budget for ourselves.  
Another example, if we target 99.999% availability but only between the hours 09:00 and 10:00, then we have two error
budgets: One of 0.001% unavailability between the hours 09:00 to 10:00 (3.6 seconds), and one of 100% unavailability
outside of those hours (assuming no other SLOs are set).

We can even set **an SLO for our SLOs** and create error budgets there, where we might have:

- An SLI: Availability: Measured by the percentage of well-formed requests that succeed, evaluated every minute.
- An SLO: 99.9% at all times.
- An SLO for our SLO: Meeting our Availability SLO 99% of the year.

This effectively means we can breach our SLO 1% of time (per year) which creates an additional error budget.

As you can see, this can get quite complicated, but if you end up defining SLIs and SLOs that contain error budgets,
it's a good idea to _use those error budgets_. This is because sometimes it is much easier to perform a dangerous canary
deployment that could break or incur a short period of downtime to do maintenance than it is to do all the work to
achieve 100% operational or performance targets. Essentially: Be practical, and use your error budgets to make your life
easier.

## Further reading

As mentioned, the Google SRE documentation does a much more in-depth job of
coving [Service Level Objectives](https://sre.google/sre-book/service-level-objectives/).
