# delta-azure-ml
A popular data engineering pattern is to use Azure Databricks and Delta Lake format for preparing data for analytics. Currently, there is no official support for reading this data in Azure Machine Learning. This repository highlights a proposed work-around to leverage the underlying Parquet files as a FileDataset in Azure Machine Learning.
