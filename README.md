# delta-azure-ml
A popular data engineering pattern is to use Azure Databricks and Delta Lake format for preparing data for analytics. Currently, there is no official support for reading this data in Azure Machine Learning. This repository highlights a proposed work-around to leverage the underlying Parquet files as a FileDataset in Azure Machine Learning.

As answered in the [Delta Lake and Delta Engine guide](https://docs.microsoft.com/en-us/azure/databricks/delta/delta-faq#can-i-access-delta-tables-outside-of-databricks-runtime), in the Frequently Asked Questions section, external reads of delta tables *is possible*. However, there is a critical step for ensuring the legitimacy of the data while accessing it without being able to understand the transaction log. You can use `VACUUM` with a retention of `ZERO HOURS` to clean up any stale Parquet files that are not currently part of any table. This operations puts the Parquet files present in DBFS into a consistent state such that they can now be read by external tools.

![](/images/delta-azdb-azml.png)

# Prerequisites

* [Azure Data Lake Store Gen2 storage account](https://docs.microsoft.com/en-us/azure/storage/blobs/create-data-lake-storage-account)
* [Azure Databricks workspace and cluster](https://docs.microsoft.com/en-us/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal?tabs=azure-portal)
* [Mount the ADLS Gen2 storage account to your Databricks cluster](https://docs.microsoft.com/en-us/azure/databricks/data/data-sources/azure/adls-gen2/azure-datalake-gen2-sp-access)
* [Azure Machine Learning workspace](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-manage-workspace?tabs=azure-portal)
* [Azure Machine Learning compute instance](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-create-attach-compute-studio#compute-instance)
* [Register ADLS Gen2 storage account as a Datastore in Azure Machine Learning workspace]()

| :exclamation:  In order to ensure the validity of the data read in Azure ML, you must run `VACUUM` with a retention of `ZERO HOURS`.  |
|---------------------------------------------------------------------------------------------------------------------------------------|

# Step 1: Import notebooks

1. From the Azure Databricks workspace, import the notebook located here: `databricks/create-delta-tables.dbc` and fill in the following values.

    * Client ID
    * Client Secret
    * Tenant ID
    * Container name
    * Storage account name

2. From the Azure Machine Learning compute instance, import the notebook located here: `azureml/read-delta-data.ipynb` and fill in the following values:

    * Datastore name (same name as when you registered ADLS Gen2 storage account in prerequisites)

# Step 2: Create a sample Delta Table in Databricks

* This notebook will walk through various sections of creating a sample Delta Table. At various points through the Databricks notebook, switch to the Azure ML notebook to see how the data changes.

# Step 3: Read sample Delta data in Azure ML

| :warning: WARNING                                                                                        |
|:---------------------------------------------------------------------------------------------------------|
| Without running `VACUUM` with retention of `ZERO HOURS` there is a risk the data will not be accurate.   |

* This notebook will walk through various sections of creating a FileDataset that points to the Parquet files stored in ADLS Gen2.
* Before using this work-around, be sure that you run `VACUUM ` with a retention of `ZERO HOURS`.

