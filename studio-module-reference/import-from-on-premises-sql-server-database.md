---
title: "Import from On-Premises SQL Server Database | Microsoft Docs"
ms.custom: ""
ms.date: 09/20/2017
ms.reviewer: ""
ms.service: "machine-learning"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: de609cf3-6501-4c04-b719-b9f52c166d65
caps.latest.revision: 12
author: "jeannt"
ms.author: "jeannt"
manager: "cgronlun"
---
# Import from On-Premises SQL Server Database
This article describes how to use the [Import Data](import-data.md) module in Azure Machine Learning Studio, to import data from an on-premises SQL Server database into a machine learning experiment.  
    
 Azure Machine Learning can access an on-premises SQL Server database if the data is provided using a Microsoft Data Management Gateway. Therefore, before you use [Import Data](import-data.md),  you must meet these requirements:
 
 + Install a Microsoft Data Management Gateway that can access the data source
 + Register the gateway in your Machine Learning workspace
 + Configure the [Import Data](import-data.md) to identify the gateway
 
After the gateway connection is established, you can then specify additional properties, such as the server and database names, authentication method, and a database query.  
  
## How to Install a Microsoft Data Management Gateway  

 To access an on-premises SQL Server database in Azure Machine Learning, you need to download and install the **Microsoft Data Management Gateway**, and then register the gateway in Machine Learning Studio.  
  
 For details about installing and registering the gateway, see [Install the Microsoft Data Management Gateway](https://azure.microsoft.com/documentation/articles/machine-learning-use-data-from-an-on-premises-sql-server/#install-the-microsoft-data-management-gateway), and **Step 1: Create a gateway**, in the article [Perform advanced analytics with Azure Machine Learning using data from an on-premises SQL server](https://azure.microsoft.com/documentation/articles/machine-learning-use-data-from-an-on-premises-sql-server/).  
  
## How to Import Data from an On-premises SQL Server Database  

After a Data Management Gateway has been installed on a computer where it can access your SQL Server database, and you have registered the gateway in Machine Learning Studio, you must configure the [Import Data](import-data.md) module.  
 > [!TIP] 
 > Before you start, disable your browser's pop-up blocker for the site, `studio.azureml.net`. If you are using the Google Chrome browser, you must download and install one of the plug-ins that are available at the Google Chrome WebStore: [Click Once App Extension](https://chrome.google.com/webstore/search/clickonce?_category=extensions).
 
 ### Use the Data Import Wizard

The module features a new wizard to help you choose a storage option,  select from among existing subscriptions and accounts, and quickly configure all options.

1. Add the **Import Data** module to your experiment. You can find the module under **Data Input and Output**.

2. Click **Launch Import Data Wizard** and follow the prompts.

3. When configuration is complete, to actually copy the data into your experiment, right-click the module, and select **Run Selected**. 

If you need to edit an existing data connection, the wizard loads all previous configuration details so that you don't have to start again from scratch.

### Manually set properties in the Import Data module
  
1.  Add the [Import Data](import-data.md) module to your experiment. You can find this module in the [Data Input and Output](data-input-and-output.md) group in the **experiment items** list in Azure Machine Learning Studio.   
  
2.  For **Data source**, select **On-Premises SQL Database**.  
  
3.  Set the following options specific to the SQL Server database.  
  
    -   For **Data gateway**, select the gateway you created.  
  
    -   Enter the **Database server name** and **Database name** for the database.  
  
    -   Click **Enter values** under **User name and password** and enter your database credentials. You can use Windows Integrated Authentication or SQL Server Authentication depending upon how your on-premises SQL Server is configured. 
    
        > [!IMPORTANT] 
        > The credential manager must be launched from within the same network as the SQL Server instance and the gateway client. Credentials cannot be passed across domains.
  
    -   Type or paste into **Database query** a SQL statement that describes the data you want to read. Always validate the SQL statement and verify the query results beforehand, using a tool such as Visual Studio Server Explorer or SQL Server Data Tools.  
  
    -   If the dataset is not expected to change between runs of the experiment, select the **Use cached results** option. When this is selected, if there are no other changes to module parameters, the experiment will load the data the first time the module is run, and thereafter use a cached version of the dataset.  
  
4.  Run the experiment.  
  
### Results

As [Import Data](import-data.md) loads the data into Studio, some implicit type conversion might be performed, depending on the data types used in the source database. For more information about data types, see [Module Data Types](machine-learning-module-data-types.md). 

When complete, click the output dataset and select **Visualize** to see if the data was imported successfully.  
  
Optionally, you can change the dataset and its metadata using the tools in Studio:  
  
-   Use [Edit Metadata](edit-metadata.md) to change column names, convert a column to a different data type, or to indicate which columns are labels or features.  
  
-   Use [Select Columns in Dataset](select-columns-in-dataset.md) to select a subset of columns.  
  
-   Use [Partition and Sample](partition-and-sample.md) to separate the dataset by criteria, or get the top *n* rows.  
  
##  <a name="TechnicalNotes"></a> Technical Notes  
 This section contains some advanced configuration options and answers to commonly asked questions.  
  
-   **How can I filter data as it is being read from the source?**  
  
     The [Import Data](import-data.md) module does not support filtering as data is being read. We recommend that you create a view or define a query that generates only the rows you need.  
  
    > [!NOTE]
    >  If you find that you have loaded more data than you need, you can overwrite the cached dataset by reading a new dataset, and saving it with the same name as the older, larger data.  
  
-   **Why do I get an error message similar to “Type Decimal is not supported”?**  
  
     When reading data from a SQL database, you might encounter an error message reporting an unsupported data type.  
  
     If the data you get from the SQL database includes data types that are not supported in Azure Machine Learning, you should cast or convert the decimals to a supported data type before reading the data. The reason is that [Import Data](import-data.md) cannot automatically perform any conversions that would result in a loss of precision.  
  
     For more information about supported data types, see [Module Data Types](machine-learning-module-data-types.md).  
  
-   **Why are some characters not displayed correctly?**  
  
     Azure Machine Learning supports the UTF-8 encoding. If string columns in your database use a different encoding, the characters might not be imported correctly.  
  
     One option is to export the data to a CSV  file in Azure storage, and use the option **CSV with encoding** to specify parameters for custom delimiters, the code page, and so forth.  
  
-   **I have set up a Data Management Gateway on my on-premises server. Can I share the same gateway between workspaces?**  
  
     While you can set up multiple Data Management Gateways in a single workspace (for example, one each for development, testing, production, *etc.*), a gateway can’t be shared across workspaces. You will need a separate gateway for each workspace.  
  
-   **I have set up a Data Management Gateway on my on-premises server that I use for Power BI / Azure Data Factory. Can I use the same gateway for Machine Learning?**  
  
     Machine Learning requires a separate Data Management Gateway. If you already have a gateway that's being used for Power BI or Azure Data Factory, then you need a separate server where a gateway for Machine Learning is installed. You can't install multiple gateways on a single server.  
  
-   **I want to be able to export data to my on-premises SQL server. Can I use the gateway with the [Export Data](export-data.md) module to write data to my on-premises SQL server?**?  
  
     Currently, Azure Machine Learning only supports importing data. We're evaluating whether you'll be able to write to your on-premises database in the future. In the meantime, you can use Azure Data Factory to copy data from the cloud to your on-premises database.  
  
-   **I have a data source that is not Microsoft SQL Server (Oracle, Teradata, *etc.*). Can I read the data in Azure Machine Learning using the on-premises option in the [Import Data](import-data.md) module?**  
  
     Currently the Azure Machine Learning [Import Data](import-data.md) module supports only Microsoft SQL Server. You can use Azure Data Factory to copy your on-premises data into cloud storage such as Azure Blob Storage or Azure Database, and then use your cloud data source in the [Import Data](import-data.md) module.  
  
##  <a name="parameters"></a> Module Parameters  
  
|Name|Range|Type|Default|Description|  
|----------|-----------|----------|-------------|-----------------|  
|Data source|List|Data Source Or Sink|Azure Blob Storage|Data source can be HTTP, FTP, anonymous HTTPS or FTPS, a file in Azure BLOB storage, an Azure table, an Azure SQL Database, an on-premises SQL Server database, a Hive table, or an OData endpoint.|  
|Data gateway|any|DataGatewayName|none|Data gateway name|  
|Database server name|any|String|none|On-premises SQL Server|  
|Database name|any|String|none|On-premises SQL Server database instance|  
|User name and password|any|SecureString|none|User name and password|  
|Database query|any|StreamReader|none|On-premises SQL query|  
  
##  <a name="Outputs"></a> Outputs  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|Results dataset|[Data Table](data-table.md)|Dataset with downloaded data|  
  
##  <a name="exceptions"></a> Exceptions  
  
|Exception|Description|  
|---------------|-----------------|  
|[Error 0027](errors/error-0027.md)|An exception occurs when two objects have to be the same size, but they are not.|  
|[Error 0003](errors/error-0003.md)|An exception occurs if one or more of inputs are null or empty.|  
|[Error 0029](errors/error-0029.md)|An exception occurs when an invalid URI is passed.|  
|[Error 0030](errors/error-0030.md)|an exception occurs in when it is not possible to download a file.|  
|[Error 0002](errors/error-0002.md)|An exception occurs if one or more parameters could not be parsed or converted from the specified type to the type required by the target method.|  
|[Error 0048](errors/error-0048.md)|An exception occurs when it is not possible to open a file.|  
|[Error 0015](errors/error-0015.md)|An exception occurs if the database connection has failed.|  
|[Error 0046](errors/error-0046.md)|An exception occurs when it is not possible to create a directory on specified path.|  
|[Error 0049](errors/error-0049.md)|An exception occurs when it is not possible to parse a file.|  
  
## See Also  
 [Import Data](import-data.md)   
 [Export Data](export-data.md)   
 [Import from Web URL via HTTP](import-from-web-url-via-http.md)   
 [Import from Hive Query](import-from-hive-query.md)   
 [Import from Azure SQL Database](import-from-azure-sql-database.md)   
 [Import from Azure Table](import-from-azure-table.md)   
 [Import from Azure Blob Storage](import-from-azure-blob-storage.md)   
 [Import from Data Feed Providers](import-from-data-feed-providers.md)