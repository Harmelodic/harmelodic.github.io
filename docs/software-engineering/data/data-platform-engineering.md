# Data Platform Engineering

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

## Platform storage and access

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

## Platform ingestion and processing

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
