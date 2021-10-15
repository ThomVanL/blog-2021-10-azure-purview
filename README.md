# Azure Purview

## Keeping an eye on your data estate.

Last year, on December 3rd 2020, Microsoft [announced the public preview](https://www.microsoft.com/security/blog/2020/12/03/manage-govern-and-get-more-value-out-of-your-data-with-azure-purview/) of Azure Purview at the "_Azure Data and Analytics digital_" event. At the time I didn't have the time to closely inspect what the service was all about. At first glance, it became clear that the service was a data governance tool, one I quickly marked as "one to watch".

Azure Purview has been generating lots of buzz, too. I would receive updates, just about every month since its public preview announcement, about new features that had been implemented. 

At the end of August 2021, I had signed up for an event called "_Maximize the value of your data in the Cloud_", which would take place on the 28th of September 2021. During this event, Rohan Kumar (Corporate VP Azure Data) would [announce the general availability](https://azure.microsoft.com/en-us/blog/govern-your-data-wherever-it-resides-with-azure-purview/) of Azure Purview. That was the last straw, I had to make some time so I could take a closer look! 🙃

Feel free to read the [full blog post](https://thomasvanlaere.com/posts/2021/10/azure-purview/)!

## Getting started with Azure Purview

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FThomVanL%2Fblog-2021-10-azure-purview%2Fmain%2Fazuredeploy.json)

To make it easier to test some of Azure Purview's features, I have created an ARM template that deploys the following resources:

- An Azure CosmosDB account with a SQL API database and an empty collection
  - Serverless compute tier
  - You could import a sample DB set, such as [CosmicWorks](https://github.com/AzureCosmosDB/CosmicWorks).
- An Azure SQL Database, using the AdventureWorkLT sample db
  - Serverless compute tier
  - ⚠️ When testing connections in the GUI you may encounter a timeout, this is likely due to the serverless compute's warm-up time. If this happens, simply give it another attempt.
- An Azure Data Lake Gen 2 storage account.
- An Azure Data Factory with a configured sample pipeline, datasets and linked services
  - The sample pipeline will export all tables from the AdventureWorksLT database to the data lake.
- A Key vault, with some secrets and access policies
  - Secrets
    - Azure Cosmos DB primary readonly key
    - Azure SQL Server admin password
    - Azure Storage Account primary key
  - Access policies
    - Azure Data Factory managed identity: allow secret get and list.
    - Azure Purview managed identity: allow secret get and list.
- An __empty__ Azure Purview account
  - You will need to register data sources, credential links with Azure Key Vault and a link with Data Factory to get data lineage support.

All of the above is useful for demonstration purposes only and not production ready, as you would likely need to apply additional security hardening techniques. At any rate, with all of those resources deployed we can have a look at some of Azure Purview's foundational elements...

