# Carbon Emissions of Travelling

This is a case study in looking at the carbon emissions of travelling, specifically from Stockholm to Brussels - since
that was a journey that someone I know was going to take.

The primary concern was whether to take an aeroplane or several train trips.

We looked up some data and did some napkin-maths and found some interesting results. This is a typed-up version of those
results.

## TL;DR

For a central-to-central, one-way journey from Stockholm to Brussels, these were overall stats. The "Aeroplane" journey
includes bus/train transport to and from the relevant airports and is a direct flight.

| Factor           | Aeroplane          | Train    |
|------------------|--------------------|----------|
| Travel Time      | 5 hours 15 minutes | 21 hours |
| Cost (Euros)     |                    |          |
| Carbon Emissions | 197.17kg           | 16.44kg  |

## Figuring it out

Data for carbon emissions from [this _Our World In Data_ post](https://ourworldindata.org/travel-carbon-footprint). It's
using data from the UK Government, Department for Energy Security and Net Zero (2022).

> Assumptions:
>
> I understand that Stockholm to Brussels does not go through the UK and so the data is not quite appropriate, and the
> data is from 2022 (it's now 2025, as I write this). However, I figured the data for carbon emissions for different
> kinds of transport is not going to be too dissimilar between the UK and the rest of Europe, and I don't think there
> has been any major changes to carbon emissions for any of these transport costs for the last 3 years.

> Since high-speed electric trains are not covered in the UK data, I'm going to use the data for "Eurostar (to Paris)",
> since the Eurostar is a high-speed electric train.
> 
> Distances of journeys gathered from raileurope.com and flightsfrom.com

## Aeroplane journey

| Leg of Journey                                                                  | Transport                  | Time   | Cost   | Emissions (kg of carbon) |
|---------------------------------------------------------------------------------|----------------------------|--------|--------|--------------------------|
| _Stockholm Central_ bus station to _Stockholm Arlanda_ airport                  | Coach (bus) (Flygbussarna) | 45m    | €11.59 | 1.08kg (40km)            |
| Time before flight (check-in, security, baggage, wait at gate)                  | Walking                    | 1h 30m | Free   | Negligible               |
| _Stockholm Arlanda_ airport to _Brussels International_ (Zaventem) airport      | Short-haul flight          | 2h 10m |        | 195.565kg (1295km)       |
| Exit airport, walk to airport train station, wait for train                     | Walking                    | 30m    | Free   | Negligible               |
| _Brussels International_ (Zaventem) airport to _Brussels Central_ train station | National Rail              | 20m    | €14    | 0.525kg (15km)           |

- Total Travel Time: 5 hours 15 minutes
- Total Cost:
- Total Emissions costs: 197.17kg

## Train journey

| Leg of Journey                                                       | Transport                          | Time          | Cost | Emissions (kg of carbon) |
|----------------------------------------------------------------------|------------------------------------|---------------|------|--------------------------|
| _Stockholm Central_ train station to _Hamburg Central_ train station | Nightjet High Speed Electric Train | 13 hours      |      | 3.244kg (811km)          |
| Wait for train change                                                | Walking                            | Up to 1 hour? | Free | Negligible               |
| _Hamburg Central_ train station to _Cologne Central_ train station   | National Rail                      | 4 hours       |      | 12.460kg (356km)         |
| Wait for train change                                                | Walking                            | Up to 1 hour? | Free | Negligible               |
| _Cologne Central_ train station to _Brussels Central_ train station  | Nightjet High Speed Electric Train | 2 hours       |      | 0.740kg (185km)          |

- Total Travel Time: 21 hours.
- Total Cost:
- Total Emissions costs: 16.44kg
