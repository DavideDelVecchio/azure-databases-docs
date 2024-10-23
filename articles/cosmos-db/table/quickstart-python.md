---
title: Quickstart - Python client library
titleSuffix: Azure Cosmos DB for Table
description: Deploy a Python web application that uses the Azure SDK for Python to interact with Azure Cosmos DB for Table data in this quickstart.
author: seesharprun
ms.author: sidandrews
ms.reviewer: sasinnat
ms.service: azure-cosmos-db
ms.subservice: table
ms.devlang: python
ms.topic: quickstart-sdk
ms.date: 10/23/2024
ms.custom: devx-track-python, devx-track-extended-azdevcli
# CustomerIntent: As a developer, I want to learn the basics of the Python library so that I can build applications with Azure Cosmos DB for Table.
---

# Quickstart: Azure Cosmos DB for Table library for Python

[!INCLUDE[Table](../includes/appliesto-table.md)]

[!INCLUDE[Developer Quickstart selector](includes/quickstart/dev-selector.md)]

This quickstart shows how to get started with the Azure Cosmos DB for Table from a Python application. The Azure Cosmos DB for Table is a schemaless data store allowing applications to store structured table data in the cloud. You learn how to create tables, rows, and perform basic tasks within your Azure Cosmos DB resource using the Azure SDK for Python.

[API reference documentation](/python/api/azure-data-tables) | [Library source code](https://github.com/Azure/azure-sdk-for-python/blob/main/sdk/tables/azure-data-tables) | [Package (PyPI)](https://pypi.org/project/azure-data-tables) | [Azure Developer CLI](/azure/developer/azure-developer-cli/overview)

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Azure Developer CLI](/azure/developer/azure-developer-cli/install-azd)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Python 3.12](https://www.python.org/downloads/)

## Initialize the project

Use the Azure Developer CLI (`azd`) to create an Azure Cosmos DB for Table account and deploy a containerized sample application. The sample application uses the client library to manage, create, read, and query sample data.

1. Open a terminal in an empty directory.

1. If you're not already authenticated, authenticate to the Azure Developer CLI using `azd auth login`. Follow the steps specified by the tool to authenticate to the CLI using your preferred Azure credentials.

    ```azurecli
    azd auth login
    ```

1. Use `azd init` to initialize the project.

    ```azurecli
    azd init --template cosmos-db-table-python-quickstart
    ```

1. During initialization, configure a unique environment name.

1. Deploy the Azure Cosmos DB account using `azd up`. The Bicep templates also deploy a sample web application.

    ```azurecli
    azd up
    ```

1. During the provisioning process, select your subscription, desired location, and target resource group. Wait for the provisioning process to complete. The process can take **approximately five minutes**.

1. Once the provisioning of your Azure resources is done, a URL to the running web application is included in the output.

    ```output
    Deploying services (azd deploy)
    
      (✓) Done: Deploying service web
    - Endpoint: <https://[container-app-sub-domain].azurecontainerapps.io>
    
    SUCCESS: Your application was provisioned and deployed to Azure in 5 minutes 0 seconds.
    ```

1. Use the URL in the console to navigate to your web application in the browser. Observe the output of the running app.

    :::image type="content" source="media/quickstart/dev-web-application.png" alt-text="Screenshot of the running web application.":::

### Install the client library

The client library is available through PyPi, as the `azure-data-tables` package.

1. Open a terminal and navigate to the `/src` folder.

    ```bash
    cd ./src
    ```

1. If not already installed, install the `azure-data-tables` package using `pip install`.

    ```bash
    pip install azure-data-tables
    ```

1. Open and review the **src/requirements.txt** file to validate that the `azure-data-tables` entry exists.

## Object model

| Name | Description |
| --- | --- |
| [`TableServiceClient`](/python/api/azure-data-tables/azure.data.tables.tableserviceclient) | This type is the primary client type and is used to manage account-wide metadata or databases. |
| [`TableClient`](/python/api/azure-data-tables/azure.data.tables.tableclient) | This type represents the client for a table within the account. |

## Code examples

- [Authenticate the client](#authenticate-the-client)
- [Get a table](#get-a-table)
- [Create an item](#create-an-item)
- [Get an item](#get-an-item)
- [Query items](#query-items)

The sample code in the template uses a table named `cosmicworks-products`. The `cosmicworks-products` table contains details such as name, category, quantity, price, a unique identifier, and a sale flag for each product. The container uses a *unique identifier** as the row key and *category* as a partition key.

### Authenticate the client

This sample creates a new instance of the `TableServiceClient` type.

```python
client = TableServiceClient("<azure-cosmos-db-table-account-endpoint>", credential=credential)
```

### Get a table

This sample creates an instance of the `TableClient` type using the `GetTableClient` function of the `TableServiceClient` type.

```python
table = client.get_table_client("<azure-cosmos-db-table-name>")
```

### Create an item

The easiest way to create a new item in a table is to TODO

```python
TODO
```

Create an item in the collection using TODO

```python
TODO
```

### Get an item

You can retrieve a specific item from a table using TODO

```python
TODO
```

### Query items

After you insert an item, you can also run a query to get all items that match a specific filter by using TODO

```python
TODO
```

Parse the paginated results of the query by TODO

```python
TODO
```

## Clean up resources

When you no longer need the sample application or resources, remove the corresponding deployment and all resources.

```azurecli
azd down
```

## Related content

- [.NET Quickstart](quickstart-dotnet.md)
- [Node.js Quickstart](quickstart-nodejs.md)
- [Java Quickstart](quickstart-java.md)
- [Go Quickstart](quickstart-go.md)
