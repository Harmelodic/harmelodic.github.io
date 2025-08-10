# Carbon Emissions of Travelling

This is a kind of "case study" looking at the carbon emissions of travelling, specifically from Stockholm to
Brussels.

We figured the aeroplane was faster and cheaper, but we wanted to see what the difference in carbon emissions were when
compared to the train journey equivalent.

We looked up some data and did some napkin-maths and found some interesting results. This is a typed-up version of those
results.

## Overview / TL;DR

For a central-to-central, one-way journey from Stockholm to Brussels, these were overall stats. The "Aeroplane" journey
includes bus/train transport to and from the relevant airports and is a direct flight.

| Factor           | Aeroplane          | Train               |
|------------------|--------------------|---------------------|
| Travel Time      | 5 hours 15 minutes | 21 hours 30 minutes |
| Cost (Euros)     | €100               | €231                |
| Carbon Emissions | 197.17kg           | 16.444kg            |

So by train, it _takes over 4x longer_ and _costs over 2x more_, but it results in over **10 times less carbon
emissions**, and about 75% of that is a single national rail train (diesel / electric) from Hamburg to Cologne,
because (high-speed) electric trains have such low emissions.

## Aeroplane journey

| Leg of Journey                                                 | Transport                  | Carbon Emissions   | Time   | Cost |
|----------------------------------------------------------------|----------------------------|--------------------|--------|------|
| _Stockholm Central_ to _Stockholm Arlanda_                     | Coach (bus) (Flygbussarna) | 1.08kg (40km)      | 45m    | €12  |
| Time before flight (check-in, security, baggage, wait at gate) | Walking                    | Negligible         | 1h 30m | Free |
| _Stockholm Arlanda_  to _Brussels International_ (Zaventem)    | Short-haul flight          | 195.565kg (1295km) | 2h 10m | €74  |
| Exit airport, walk to airport train station, wait for train    | Walking                    | Negligible         | 30m    | Free |
| _Brussels International_ (Zaventem) to _Brussels Central_      | National Rail              | 0.525kg (15km)     | 20m    | €14  |

- Total Emissions costs: 197.17kg
- Total Travel Time: 5 hours 15 minutes
- Total Cost: €100

## Train journey

| Leg of Journey                           | Transport                          | Carbon Emissions | Time                | Cost |
|------------------------------------------|------------------------------------|------------------|---------------------|------|
| _Stockholm Central_ to _Hamburg Central_ | High-speed Electric Train (Night)  | 3.244kg (811km)  | 12 hours 30 minutes | €156 |
| Wait for train change                    | Walking                            | Negligible       | 2 hours(-ish)       | Free |
| _Hamburg Central_ to _Cologne Central_   | National Rail                      | 12.460kg (356km) | 4 hours             | €43  |
| Wait for train change                    | Walking                            | Negligible       | 1 hours(-ish)       | Free |
| _Cologne Central_ to _Brussels Central_  | Nightjet High-speed Electric Train | 0.740kg (185km)  | 2 hours             | €32  |

- Total Emissions costs: 16.444kg
- Total Travel Time: 21 hours 30 minutes
- Total Cost: €231

## Data sources

Data for carbon emissions from [this _Our World In Data_ post](https://ourworldindata.org/travel-carbon-footprint). It's
using data from the UK Government, Department for Energy Security and Net Zero (2022).

I understand that Stockholm to Brussels does not go through the UK and so the data is not quite appropriate, and the
data is from 2022 (it's now 2025, as I write this). However, I figured the data for carbon emissions for different
kinds of transport is not going to be too dissimilar between the UK and the rest of Europe, and I don't think there
has been any major changes to carbon emissions for any of these transport costs for the last 3 years.

Since high-speed electric trains are not covered in the UK data, I'm going to use the data for "Eurostar (to Paris)",
since the Eurostar is a high-speed electric train.

### Rail time, distance and cost

Distances of journeys gathered from [Rail Europe](https://www.raileurope.com).

Night Train from Stockholm to Hamburg is a night train, booked through [SJ](https://www.sj.se) (Sweden national train
operator), with a bed _in a sleeping compartment_ as going just in a seat is quite uncomfortable for such a long
journey. A seat normally costs 525kr (€48), whereas a sleeping compartment costs around 1735kr (€156).

Wait times based on actual wait times on this journey that we've experienced.

Other train prices gathered from [Rail Europe](https://www.raileurope.com) as the "30 days in advance" cost, at time of
writing, then rounded up to the nearest euro.

- Rail Europe, [Stockholm -> Hamburg distance](https://www.raileurope.com/en/destinations/stockholm-hamburg-train)
- Nachtzug.net, [Stockholm -> Hamburg night train time](https://nachtzug.net/en/stockholm-hamburg/)
- Rail Europe, [Hamburg -> Cologne](https://www.raileurope.com/en/destinations/hamburg-cologne-train), €42.11
- Rail Europe, [Cologne -> Brussels](https://www.raileurope.com/en/destinations/cologne-brussels-train), €31.47

### Flight time, distance and cost

Flight distance taken from FlightsFrom, [ARN -> BRU](https://www.flightsfrom.com/ARN-BRU).

Flight time and prices taken from [SkyScanner](https://www.skyscanner.se) as the average price of a flight, ~30 days
from time of writing (10th August 2025):

- 7th September: €78
- 8th September: €72
- 9th September: €61
- 10th September: €62
- 11th September: €74
- 12th September: €87
- 13th September: €80

Average (Mean): €74 (rounded up to the nearest euro)
