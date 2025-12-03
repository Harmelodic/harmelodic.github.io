# Data Science Overview

Data science is the job of extracting useful knowledge from data.

Typically, systems and people do work and in the process of doing that work, data is created and managed by those
systems and people (e.g. banks and banking systems manage accounts, customer data, executing transfers, storing records
of transactions, etc.). This work produces data either as (a) the raw data itself (e.g. customer data, transaction
records, etc.) or "meta" data about the work (e.g. when customers update their data, when transactions are taken place,
who is transferring money to whom).

By taking copies of that data, and then processing it, storing it and visualising it, we can gain insights about how
systems are being used, how users behave, how well the organisation is performing, and more. We can further take this
data and make predictions about it, even doing machine learning to build statistical models to aid prediction (or
generative models to generate more data for... whatever purpose).

Therefore, there are two parts to engineering in the Data Science ecosystem:

1. Data Platform Engineering: Building a data platform, to allow data science to happen effectively and efficiently.
2. Data Science: The actual work of analysing data and doing something with that analysis.

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

## Data Platform Engineering

In order for data science to happen, we need the following:

- A place to store data somewhere and access it.
- A way to get data into that data storage, and transform it into a format that is usable.
- Various means to use the data for data science purposes, mainly:
	- Further data processing / transformations as necessary.
	- Visualisation for human viewing and decision-making.
	- Modelling (statistical modelling, mathematical modelling, machine learning prediction & generation) for
	  data-driven predictions and analysis.
	- Operational work (don't recommend, as you end up with coupling between operational and data systems which creates
	  rigidity in the architecture, which is painful when wanting you want to change either architecture. Instead,
	  either build operational needs directly or find a point of decoupling (e.g. use the data platform to build an ML
	  model and then run the model as part of the operational needs), depending on your use case).

### Platform storage and access

We need to store data somewhere for doing data analysis. We tend to not directly access the raw data itself (that is
stored within databases and other systems that are doing the work for us), but instead take copies and store them
somewhere. That somewhere could be a:

- [Data lake](https://en.wikipedia.org/wiki/Data_lake) - where raw data is stored in blob storage. This data could be
  structured, semi-structured or unstructured (i.e. databases, structured files, or unstructured files).
- [Data warehouse](https://en.wikipedia.org/wiki/Data_warehouse) - where data is stored in a structured way, ready for
  analysis or further processing.

Generally, I don't recommend building a data lake. It's a lot of engineering effort to build what often becomes a "data
swamp" of huge amounts of data that is stored but never used, or is very difficult to use because much of it is
unstructured or semi-structured.

Instead, I recommend building a Data Warehouse, and then iteratively expanding on its storage and access capabilities as
needs arise. The foundations of this data warehouse should probably be data in tables.

In order to build this, you could use something as simple as [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL), but
is typically done now with cloud-based data warehouse services and tools like:

- GCP's [BigQuery](https://docs.cloud.google.com/bigquery/docs/introduction)
- AWS's [Redshift](https://docs.aws.amazon.com/redshift/) (named after "shifting" away from "Big Red" (Oracle))
- Azure's [Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/overview-what-is)
- Oracle's... stuff.
- Probably some others.

Once you have storage in place, you then need to think about:

- Data Governance (principles and rules for how data is used):
	- Legal responsibilities & accountability.
	- Principles & Rules to ensure safe and ethical data usage.
	- Principles & Rules to ensure "proper" data engineering is done.
	- Data Classification to classify data (Secret, Confidential, Internal, Public - or more fine-grained)
- Access Model
	- Who/What can access what data
	- Usually some RBAC or ABAC access control system
	- Access model based on Data Classification and Business Domain.
- Organising data in the data platform
	- Recommend following the Data Mesh approach where data is organised by business domain (where some common data is
	  likely available for all business domains, but can be (internal to the data platform) extracted, transformed and
	  used according to the business domain's needs).
	- Alternatively, attempt to model data canonically (global and unified) and make it all available for people to use
	  as they need - which is theoretically more flexible, but canonical models are fragile and difficult to maintain
	  and the flexibility becomes problematic as data governance needs change and grow.

### Platform ingestion and processing

In order to get data into a data platform, you need to "ingest" it... or "extract" it from somewhere. This is typically
done through some data pipelines, based on ETL or ELT:

- ETL = Extract Transform Load - where you extract data from sources, transform it in some way, and load it into a
  target location.
- ELT = Extract Load Transformation - where you extract data from sources, load it into a centrally-managed location
  and then perform transformations on that data to create new models that work better for visualisations,
  statistics, etc.
	- Could use [DBT](https://docs.getdbt.com/docs/introduction) for doing data transformations in ELT systems.

Once data is in place in the warehouse, it can go through further ETL or ELT processing as is necessary by data
scientists / engineers for their business needs.

TODO: Talk about Batch vs Real-time ingestion and processing.

TODO: Mention data processing technologies such as and how they are used:

- [Apache Airflow](https://airflow.apache.org/docs/apache-airflow/stable/index.html) - batch-oriented workflow
  management
- [Apache Beam](https://beam.apache.org/documentation/) - batch & stream data processing
- [Apache Camel](https://camel.apache.org/) - For building integration & transformation systems
- [Apache Kafka](https://kafka.apache.org/) - Messaging system
- [Apache NiFi - User Guide](https://nifi.apache.org/docs/nifi-docs/html/user-guide.html) - diagrammatic flow-based data
  processing.
- [Apache Tika - Metadata Extraction](https://tika.apache.org/) - Extracting metadata from unstructured files (pictures,
  emails, pdfs)
- [Cloudera](https://www.cloudera.com/) - Managed Apache Software for Data Platform building
- [GCP Dataflow](https://cloud.google.com/dataflow) - GCP-managed Apache Airflow
- [GCP Pub/Sub](https://cloud.google.com/pubsub) - GCP-managed messaging system
- [Spring Integration](https://spring.io/projects/spring-integration) - For building integration & transformation
  systems

## Data Science and further engineering

Much of data science is done using the following programming languages:

- [Python](https://www.python.org/), for general programming, using (for example):
	- [Pandas](https://pandas.pydata.org/) for working doing data analysis and manipulation.
    - [NumPy](https://numpy.org/) for optimised, mathematical functions (both "simple" and complex).
	- [matplotlib](https://matplotlib.org/) or [plotly](https://plotly.com/python/) for general visualisations.
	- [pyvis](https://pyvis.readthedocs.io/en/latest/index.html) for network visualisations.
	- [networkx](https://networkx.org/documentation/stable/reference/index.html) for network manipulation and analysis.
	- [Jupyter (Notebooks)](https://jupyter.org/) for doing notebook-style development.
	- [PyTorch](https://pytorch.org/) for "deep learning".
- SQL, for defining relational data structures, doing transformations and creating views.
- [R](https://www.r-project.org/) for doing statistics and data visualisation, using packages
  from [Tidyverse](https://tidyverse.org/)

TODO: Talk about how to build and do the following:

- Data Analysis & Viewing
	- Data in huge tables, then do in-place data-crunching, to output new values (on schedules).
	- Dashboards using things like [Tableau](https://www.tableau.com/)
	  or [Looker (Google Cloud)](https://cloud.google.com/looker).
- Machine Learning / Artificial Intelligence
	- Building (statistical) machine learning models (using data for training and prediction, and possibly deploying for
	  operational needs)
	- Building generative AI models (using data for training, then deploying for operational needs) - but also: Ew,
	  don't do this - generative AI is yucky.
