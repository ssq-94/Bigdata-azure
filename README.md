# Bigdata-azure
# 1. Project Overview
![image](https://github.com/ssq-94/Bigdata-azure/assets/78969075/23fe8021-e573-4051-9893-96955279bcb7)
This is a Big Data Engineering Project on Azure. Data will be ingested from 2 data sources and ingested into the Data Lake in Azure. In the Data Lake, after a couple of data transformation and Machine Learning Model running, we make the Machine Learning result back to the Data Lake. Then we will use Azure Synapse to connect to the Data Lake, and we will generate a report from the Synapse.

# 2. About Data
2.1. Data Background
This dataset is from StackOverflow, a famous online IT developer community. The dataset records daily online posts, including post types and users' information.

The dates in the dataset have been modified to be up-to-date. The original dates were historical, but after our modification, the dates are current. Therefore, every day, you will see today's Posts data.

The 3 tables are located in 2 data sources, RDS and Azure Storage Blob Container.

- RDS: The Users and PostTypes tables are stored on RDS Postgres database. These are 2 slow change tables, we need to update them once every week. The update will follow SCD type 1, which means we only keep the new records and overwrite the old records.

- Azure Storage Blob: The daily Posts data files are in parquet format. They are stored under the storage account wcddestorageexternal, the container de-project-st, and folder Posts_today.

# 3. Business Requirements
   
3.1. Data Lake Requirements
- Create a Data Lake. It is nothing but the Azure Storage Blob but with the features of hierarchical namespace.
- Create an Azure Data Factory to finish the data ingestion and processing.
- Connect to an AWS RDS Postgres database and ingest PostTypes and Users tables from RDS to your Data Lake. But you need to ingest once every week.
- Connect to a WeCloudData Azure blob container to copy the parquet files of Posts from StackOverflow into your data lake. You need to ingest it every day.

3.2 Machine Learning Process Requirements
You need to create a Databricks notebook to process data and feed the data into a Machine Learning Model, and output the running result from the Machine Learning Model.

The purpose of this machine learning model is to read the posts' text in the Posts files, and classify what topic of this post is about. And then, we will use Spark to output a file to list all the topics for today and order the topics by their numbers.But before the output, we need a series work to clean and transform the data.


3.3. Chart Requirements
You need to create a chart on Synapse based on the output result data of the Machine Learning Model to display the top 10 topics of today.

# 4. Project Infrastructure
In this project, all the infrastructure is built on Azure.

1. Azure Data Lake: Create a data lake. The data lake will be used for: Storing the ingested Posts, PostTypes and Users files; Storing the data file for Machine Learning; Storing the output data file after the Machine learning.
2. Azure Data Factory (ADF): Finish the entire pipeline from data ingestion, transformation to Machine learning job.
3. Azure Synapse: Works as a platform to do data analysis.

# 5. Part One: Data Ingestion

![image](https://github.com/ssq-94/Bigdata-azure/assets/78969075/80bf476a-a731-49ab-bd05-8a3690db9334)

The first part of the project is Data Ingestion. In this part, you have 2 data sources to connect: the Postgres database and the Azure blob container. Use Azure Data Factory to connect to the database "stack" and the schema "raw_st" of the Postgres database on AWS RDS, to transfer the tables Users and PostTypes to your data lake. The two tables only need to be ingested once. You don't need to set a daily schedule. Also, connect to Azure to connect to the storage account wcddestorageexternal, under the container de-project-st and folder Posts_today.

# 6. Part Two: Data Transformation

![image](https://github.com/ssq-94/Bigdata-azure/assets/78969075/376244e6-d203-4ac8-8d3d-e2f03a0603e7)

The second part of the project is data transformation. The transformation process will happen in the Azure Data Factory pipeline. You will create a Databricks notebook on the data lake and run data transformation and Machine Learning Running on the Data. The output result will be used for the BI dashboards in the next step.

# 7. Part Three: Data Visualization

![image](https://github.com/ssq-94/Bigdata-azure/assets/78969075/230ad4f3-aefa-43a9-ad6a-48c5cdc1756e)

In the last part, you will use Synapse to connect to the data lake to generate a chart which can display the top 10 topics in the posts.

