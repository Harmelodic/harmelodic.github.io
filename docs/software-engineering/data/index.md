# Data Overview

Data engineering is work to enable extracting and actually extract useful knowledge from data, usually "big" datasets.

Typically, systems and people do work and in the process of doing that work, data is created and managed by those
systems and people (e.g. banks and banking systems manage accounts, customer data, executing transfers, storing records
of transactions, etc.). This work produces data either as (a) the raw data itself (e.g. customer data, transaction
records, etc.) or "meta" data about the work (e.g. when customers update their data, when transactions are taken place,
who is transferring money to whom).

By taking copies of that data, and then processing it, storing it and visualising it, we can gain insights about how
systems are being used, how users behave, how well the organisation is performing, and more. We can further take this
data and make predictions about it, even doing machine learning to build statistical models to aid prediction (or
generative models to generate more data for... whatever purpose).

Therefore, there are two parts to engineering in the Data engineering ecosystem:

1. [Data Platform Engineering](./data-platform-engineering.md): Building a data platform, to allow data science to
   happen effectively and efficiently.
2. [Data Science](./data-science.md): The actual work of analysing data and doing something with that analysis.

## Data Engineers and Data Scientists and Data Mesh

Pre-2000 and until the mid-late 2010s, specialised data science and engineering people would be tasked with doing these
two parts. With data engineers doing the platform engineering, and data scientists doing the analysis. They would be
their own team or department within an organisation and interface as specialists with different departments. Data is
stored and organised in the data platform as is most liked / appropriate for the data engineers & data scientists.

However, the [Data mesh](https://en.wikipedia.org/wiki/Data_mesh) philosophy has spread in modern times, where a
dedicated data platform team builds a robust and flexible data platform. Data scientists and regular employees (who
nowadays have more programming skills than they used to) then access the data platform and perform data science work as
needed for their particular business domain. Also, the data inside the data platform is still stored centrally and
accessed in a common way, but is organised by business domain (which may involve duplication of data between domains).
The data modelling and processing for each business-domain is then owned by the engineers and data scientists of that
business domain.
