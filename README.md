# Dynamically train and score Form Recognizer model to extract key-value pairs from different type of forms at scale using REST API with Python

In my first blog about the automated form processing, I described how you can extract key-value pairs from your forms in real-time using the Azure Form Recognizer cognitive service. We successfully implemented that solution for many customers. 
In this part 2 of the blog about the Form Recognizer Cognitive service, we will discuss common requirements across many customers we came across and how we address them as the product itself evolves. 
Often, after a successful PoC or MVP, our customers realize that, not only they need this real time solution to process their forms but, they also have a huge backlog of forms they would like to ingest into their relational or NoSQL databases, in a batch fashion. In the backlog, they different types of forms and they don’t want to build a model for each type.
In this blog, we’ll describe how to automatically (re)train existing models or create new ones extract the key-value pairs of off different type of forms and at scale.

# Business Problem

Most organizations are now aware of how valuable the data they have in different format (pdf, images, videos…) in their closets are. They are looking for best practices and most cost-effective ways and tools to digitize those assets.  By extracting the data from those forms and combining it with existing operational systems and data warehouses, they can build powerful AI and ML models to get insights from it to deliver value to their customers and business users.
With the [Form Recognizer Cognitive Service](https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/overview), we help organizations to harness their data, automate processes (invoice payments, tax processing …), save money and time and get better accuracy.

![alt text](https://github.com/issaghaba/FormRecognizer/blob/main/images/BusinessProblem.png)

In this section, we’ll describe how to dynamically ingest millions of forms, of different types using Azure Services.
Your backlog of forms might be in your on-premise environment or in a (s)FTP server. We assume that you were able to upload them into an Azure Data Lake Store Gen 2, using [Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-portal), [Storage Explorer](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) or [AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-blobs). Therefore, the solution we’ll describe here will focus on the data ingestion from the data lake to the (No)SQL database.
Our product team published a great tutorial on how to [Train a Form Recognizer model and extract form data by using the REST API with Python](https://docs.microsoft.com/en-us/azure/cognitive-services/form-recognizer/quickstarts/python-train-extract?tabs=v2-0). The solution described here demonstrates the approach for one model and one type of forms. Often, our customers have different type of forms coming from their many clients and customers. The value-add of the post is show you how to automatically train a model with new and different type of forms using a meta-data driven approach. If you do not have an existing model, the program will create one for you and give you the model id.
The REST AP requires some parameters as input. For security reasons, some of these parameters will be store in Azure Key Vault and others, less sensitive, like the blob folder name, will be in a parametrization table in an Azure SQL DB. 
For each form type, Data engineers or data scientists will populate the param table. Will use Azure data factory to iterate over the list of forms type and pass the relevant parameters to an Azure Databricks notebook to (re)train the model.


![alt text](https://github.com/issaghaba/FormRecognizer/blob/main/images/HighLevelArchitecture.png)
