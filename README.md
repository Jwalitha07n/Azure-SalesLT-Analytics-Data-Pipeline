# Transferring, cleaning, and transforming data from on-premises environments to Delta Lake storage in Azure, enabling seamless analytics, reporting, and visualization.

#### <ins>Project Objective</ins>
Utilising Azure Data Factory and Databricks to transform and analyse on-premise data, as well as Synapse and PowerBI for analysis and reporting.

The goal of this project is to use a cloud provider and distributed computing to design the data found in on-premises databases in order to produce insights and reports. There is a structured data in an on-premise website database. It's possible that there is much data for the on-premises computing capability to handle the necessary transformation. Additionally, because infrastructure setup takes time, money, and planning, it may not always be possible. Additionally, the infrastructure might soon become obsolete or superfluous. Thus, moving to cloud is the easiest way to achieve the desired results without much overhead.

#### <ins>Procedural Diagram</ins>
<img width="547" alt="image" src="https://github.com/user-attachments/assets/07ac0deb-75ef-41a4-869a-6fb57e2a1da8">


The AdventureWorksLT2022 database, which is accessible to the public, serves as the on-premises database in this instance. This is used since it's lightweight, helps keep expenses under control, and emphasises the process above processing power. In a real-world scenario, the dataset would be substantially larger, but the architecture employed in this project could still manage it effectively. This is the high-level architecture diagram.


#### Components and Data flow:

•	SQL Server: A Self-Hosted Integration Runtime (SHIR) connects the AdventureWorksLT2022 database, which is housed on an on-premises SQL Server, to Azure Data Factory (ADF), allowing for safe data transfer and conversion into Azure Data Lake Storage. The SQL Server instance is managed using SQL Server Management Studio (SSMS), which also allows you to create users and configure permissions for safe access. ADF authenticates and connects to the database using the SQL Server credentials, which are safely kept in Azure Key Vault. In addition to preserving effective transformations within ADF prior to loading data into the Azure Data Lake, this guarantees safe and smooth data transport.


<img width="857" alt="image" src="https://github.com/user-attachments/assets/4e744e24-dab6-4a0c-a56f-98ea27fa8d76">


•	Azure Data Factory is used to import data from SQL Server and store it in Azure Data Lake. It is configured to carry out preliminary transformations on the incoming tables and uses automated pipelines. To enable read and write operations, the ADF resource was added to the Data Lake's access control (IAM) regulations. The tables had the default snappy compression and were transformed to parquet format. The pipeline was used to automatically store the tables in the appropriate folder structure. The pipeline that consumes data from the on-premises server and completes the operations in the pipeline to generate the final transformed data in the gold container is depicted in the diagram below.


<img width="949" alt="image" src="https://github.com/user-attachments/assets/3ce127da-d9ac-49bb-95de-373ecfb7da25">



•      Azure Data Lake Gen 2: Bronze, Silver, and Gold are the three containers that have been made. The phases of business logic and requirements are represented by the three levels. Since parquet format offers substantial performance, storage, and query optimisation advantages in big data processing and analytics scenarios, the first data ingested from SQL Server is converted to it and stored in the bronze container. The bronze data undergoes the next level change and is then kept in the silver container. The data is saved in the gold container once the silver layer has undergone the last modification. The data in the gold and silver containers is stored in parquet format files using the delta lake abstraction layer to enable versioning. The gold layer's data is perfect for analysis and reporting. Azure Synapse Analytics, which can then be linked to PowerBI to create visualisations, can use it.



<img width="953" alt="image" src="https://github.com/user-attachments/assets/64c1f038-f66b-450f-8f12-da9a9667e4aa">


<img width="958" alt="image" src="https://github.com/user-attachments/assets/a9be2307-2061-4923-97aa-8c683131bf91">


<img width="952" alt="image" src="https://github.com/user-attachments/assets/4e040187-868d-45a8-9e76-e60762281a86">

•	Databricks: Databricks was the preferred compute engine since it offers the capabilities of Apache Spark without requiring environment setup, and because it is cloud native, it was a good fit for this project's needs. Since the email address used for this project was already included in the data lake's IAM policy, it was easy to access the storage using the credential passthrough capability. An access token created with Databricks and stored as a secret in the Key vault allowed access to the Data Factory, and the "mount storage" notebook was used to mount the storage. Two stages of transformation were carried out. The data in the bronze container underwent the first transformation. The silver container held the output from this stratum. The silver data, which was kept in the gold container, underwent the second level. The premise is that, in a real-world corporate setting, there might be several stages of transformation, and the final converted data—the data in the gold container—is then used for reporting and analytics. Although the AdventureWorksLT table's data is already organised and largely useful, the Modified date column has been changed from the UTC timestamp to the YYYY-MM-DD format for demonstration purposes using the bronze to silver transformation. The column names in the silver to gold transformation step were previously two words combined, each beginning with a capital letter to being separated by an ‘_’.



•	Azure Synapse Analytics was selected because it combines big data, data integration, and data warehousing into one platform. It is based on an incredibly powerful analytics tool and a massively parallel processing architecture that shares many features with Azure Data Factory. It speeds up the process of creating a database and makes it possible to query it. Because it enables automatic pause and resume capabilities and on-demand provisioning, costs can be minimised. It is simple to interface with other Azure services, including the Azure data lake.

<img width="960" alt="image" src="https://github.com/user-attachments/assets/1b215531-34d6-405c-bfef-c35afd6192fb">


<img width="950" alt="image" src="https://github.com/user-attachments/assets/09fd39dd-ca79-41a3-860a-d645a113cd58">


<img width="932" alt="image" src="https://github.com/user-attachments/assets/83ad8834-5392-44c1-9aee-234ab320cc30">


PowerBI: Azure Synapse may now be used to analyse the data. Dashboards can be made with that. I've done it in this project using PowerBI. The synaptic database must be linked to PowerBI. By selecting the Data Source option in PowerBI and then providing the database name and the serverless endpoint of the Synapse in the source, we can establish a connection using the Azure Synapse option under Azure. Instead of utilising credentials, we can connect by using the Sign in option. This necessitates using the same email address for both the PowerBI sign-in and the Azure account. Otherwise, Azure Active Directory can be used to invite a guest and grant that user the necessary permissions. In this instance, I was forced to employ that mode. Direct query or direct loading of the data onto the device are both options. The models tab must be used to model the data after it has been loaded. It must be done manually because PowerBI relationships are typically incorrect. The manage relationship option from the ribbon's visualisations can be used to accomplish this. To enable all the fields to be updated across all PowerBI visualisations, the cross-filter direction must be set to both after the relationships are established. 


![table_data_load_powerbi](https://github.com/DataCounsel/Azure-Data-Engineering/assets/71335870/16300aad-a37c-42f0-8852-f175e37b74db)

Data Loaded

<img width="709" alt="image" src="https://github.com/user-attachments/assets/401811cd-c025-4c2c-98bc-587aa4175901">

Model view where we can manage relationships

