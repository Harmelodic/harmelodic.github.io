# Data Science

...and other bits of non-platform data engineering.

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
- Using the processing & analysis tools offered by a [Data Platform](./data-platform-engineering.md)

TODO: Talk about how to build and do the following:

- Data Analysis & Viewing
	- Data in huge tables, then do in-place data-crunching, to output new values (on schedules).
	- Visualising things using [Tableau](https://www.tableau.com/)
	  or [Looker (Google Cloud)](https://cloud.google.com/looker) or [Jupyter Notebooks](https://jupyter.org/).
	- Writing data pipelines / workflows with:
		- [Apache Airflow](https://airflow.apache.org/docs/apache-airflow/stable/index.html) batch workflows
		- [Apache Beam](https://beam.apache.org/documentation/) batch & stream pipelines
		- [Apache Camel](https://camel.apache.org) / [Spring Integration](https://spring.io/projects/spring-integration)
		  services for integrating systems together.
- Machine Learning / Artificial Intelligence
	- Building (statistical) machine learning models (using data for training and prediction, and possibly deploying for
	  operational needs)
	- Building generative AI models (using data for training, then deploying for operational needs) - but also: Ew,
	  don't do this - generative AI is yucky.
	- Think about:
		- [TensorFlow](https://en.wikipedia.org/wiki/TensorFlow) - Python library for ML (OG declarative, which was good
		  but hard. PyTorch got more popular).
		- [PyTorch](https://en.wikipedia.org/wiki/PyTorch) - Python library but normal imperative programming and
		  simpler APIs than TensorFlow.
		- [JAX](https://en.wikipedia.org/wiki/JAX_(software)) - Python library for automatic derivative calculations &
		  high-performance numerical computing.
		- [ONNX](https://en.wikipedia.org/wiki/Open_Neural_Network_Exchange) graph file - artefact for ML models.
		- Could run them on _TensorFlow inference runtime_ or on _TensorFlow Lite runtime_.
