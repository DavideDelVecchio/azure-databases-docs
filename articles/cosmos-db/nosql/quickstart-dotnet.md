---
title: Quickstart - .NET client library
titleSuffix: Azure Cosmos DB for NoSQL
description: Deploy a .NET web application that uses the client library to interact with Azure Cosmos DB for NoSQL data in this quickstart.
author: seesharprun
ms.author: sidandrews
ms.service: azure-cosmos-db
ms.subservice: nosql
ms.devlang: csharp
ms.topic: quickstart-sdk
ms.date: 10/15/2024
ms.custom: devx-track-csharp, devx-track-dotnet, devx-track-extended-azdevcli
zone_pivot_groups: azure-cosmos-db-quickstart-env
# CustomerIntent: As a developer, I want to learn the basics of the .NET library so that I can build applications with Azure Cosmos DB for NoSQL.
---

# Quickstart: Azure Cosmos DB for NoSQL library for .NET

[!INCLUDE[NoSQL](../includes/appliesto-nosql.md)]

[!INCLUDE[Developer Quickstart selector](includes/quickstart/dev-selector.md)]

Get started with the Azure Cosmos DB for NoSQL client library for .NET to query data in your containers and perform common operations on individual items. Follow these steps to deploy a minimal solution to your environment using the Azure Developer CLI.

[API reference documentation](/dotnet/api/microsoft.azure.cosmos) | [Library source code](https://github.com/Azure/azure-cosmos-dotnet-v3) | [Package (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) | [Azure Developer CLI](/azure/developer/azure-developer-cli/overview)

## Prerequisites

[!INCLUDE[Developer Quickstart prerequisites](includes/quickstart/dev-prereqs.md)]

## Setting up

Deploy this project's development container to your environment. Then, use the Azure Developer CLI (`azd`) to create an Azure Cosmos DB for NoSQL account and deploy a containerized sample application. The sample application uses the client library to manage, create, read, and query sample data.

::: zone pivot="devcontainer-codespace"

[![Open in GitHub Codespaces](https://img.shields.io/static/v1?style=for-the-badge&label=GitHub+Codespaces&message=Open&color=brightgreen&logo=github)](https://codespaces.new/azure-samples/cosmos-db-nosql-dotnet-quickstart?template=false&quickstart=1&azure-portal=true)

::: zone-end

::: zone pivot="devcontainer-vscode"

[![Open in Dev Container](https://img.shields.io/static/v1?style=for-the-badge&label=Dev+Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/azure-samples/cosmos-db-nosql-dotnet-quickstart)

::: zone-end

::: zone pivot="devcontainer-codespace"

> [!IMPORTANT]
> GitHub accounts include an entitlement of storage and core hours at no cost. For more information, see [included storage and core hours for GitHub accounts](https://docs.github.com/billing/managing-billing-for-github-codespaces/about-billing-for-github-codespaces#monthly-included-storage-and-core-hours-for-personal-accounts).

::: zone-end

::: zone pivot="devcontainer-vscode"

::: zone-end

1. Open a terminal in the root directory of the project.

1. Authenticate to the Azure Developer CLI using `azd auth login`. Follow the steps specified by the tool to authenticate to the CLI using your preferred Azure credentials.

    ```azurecli
    azd auth login
    ```

1. Use `azd init` to initialize the project.

    ```azurecli
    azd init --template cosmos-db-nosql-dotnet-quickstart
    ```

    > [!NOTE]
    > This quickstart uses the [azure-samples/cosmos-db-nosql-dotnet-quickstart](https://github.com/azure-samples/cosmos-db-nosql-dotnet-quickstart) template GitHub repository. The Azure Developer CLI will automatically clone this project to your machine if it is not already there.

1. During initialization, configure a unique environment name.

    > [!TIP]
    > The environment name will also be used as the target resource group name. For this quickstart, consider using `msdocs-cosmos-db`.

1. Deploy the Azure Cosmos DB account using `azd up`. The Bicep templates also deploy a sample web application.

    ```azurecli
    azd up
    ```

1. During the provisioning process, select your subscription and desired location. Wait for the provisioning process to complete. The process can take **approximately five minutes**.

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

The client library is available through NuGet, as the `Microsoft.Azure.Cosmos` package.

1. Open a terminal and navigate to the `/src/web` folder.

    ```bash
    cd ./src/web
    ```

1. If not already installed, install the `Microsoft.Azure.Cosmos` package using `dotnet add package`.

    ```bash
    dotnet add package Microsoft.Azure.Cosmos --version 3.*
    ```

1. Also, install the `Azure.Identity` package if not already installed.

    ```bash
    dotnet add package Azure.Identity --version 1.*
    ```

1. Open and review the **src/web/Cosmos.Samples.NoSQL.Quickstart.Web.csproj** file to validate that the `Microsoft.Azure.Cosmos` and `Azure.Identity` entries both exist.

## Object model

| Name | Description |
| --- | --- |
| <xref:Microsoft.Azure.Cosmos.CosmosClient> | This class is the primary client class and is used to manage account-wide metadata or databases. |
| <xref:Microsoft.Azure.Cosmos.Database> | This class represents a database within the account. |
| <xref:Microsoft.Azure.Cosmos.Container> | This class is primarily used to perform read, update, and delete operations on either the container or the items stored within the container. |
| <xref:Microsoft.Azure.Cosmos.PartitionKey> | This class represents a logical partition key. This class is required for many common operations and queries. |

## Code examples

- [Authenticate the client](#authenticate-the-client)
- [Get a database](#get-a-database)
- [Get a container](#get-a-container)
- [Create an item](#create-an-item)
- [Get an item](#read-an-item)
- [Query items](#query-items)

[!INCLUDE[Developer Quickstart sample explanation](includes/quickstart/dev-sample-primer.md)]

### Authenticate the client

[!INCLUDE[Developer Quickstart authentication explanation](includes/quickstart/dev-auth-primer.md)]

This sample creates a new instance of the `CosmosClient` class and authenticates using a `DefaultAzureCredential` instance.

```csharp
DefaultAzureCredential credential = new();

CosmosClient client = new(
    accountEndpoint: "<azure-cosmos-db-nosql-account-endpoint>",
    tokenCredential: new DefaultAzureCredential()
);
```

### Get a database

Use `client.GetDatabase` to retrieve the existing database named *`cosmicworks`*.

```csharp
Database database = client.GetDatabase("cosmicworks");
```

### Get a container

Retrieve the existing *`products`* container using `database.GetContainer`.

```csharp
Container container = database.GetContainer("products");
```

### Create an item

Build a C# record type with all of the members you want to serialize into JSON. In this example, the type has a unique identifier, and fields for category, name, quantity, price, and sale.

```csharp
public record Product(
    string id,
    string category,
    string name,
    int quantity,
    decimal price,
    bool clearance
);
```

Create an item in the container using `container.UpsertItem`. This method "upserts" the item effectively replacing the item if it already exists.

```csharp
Product item = new(
    id: "68719518391",
    category: "gear-surf-surfboards",
    name: "Yamba Surfboard",
    quantity: 12,
    price: 850.00m,
    clearance: false
);

ItemResponse<Product> response = await container.UpsertItemAsync<Product>(
    item: item,
    partitionKey: new PartitionKey("gear-surf-surfboards")
);
```

### Read an item

Perform a point read operation by using both the unique identifier (`id`) and partition key fields. Use `container.ReadItem` to efficiently retrieve the specific item.

```csharp
ItemResponse<Product> response = await container.ReadItemAsync<Product>(
    id: "68719518391",
    partitionKey: new PartitionKey("gear-surf-surfboards")
);
```

### Query items

Perform a query over multiple items in a container using `container.GetItemQueryIterator`. Find all items within a specified category using this parameterized query:

```nosql
SELECT * FROM products p WHERE p.category = @category
```

```csharp
string query = "SELECT * FROM products p WHERE p.category = @category"

var query = new QueryDefinition(query)
  .WithParameter("@category", "gear-surf-surfboards");

using FeedIterator<Product> feed = container.GetItemQueryIterator<Product>(
    queryDefinition: query
);
```

Parse the paginated results of the query by looping through each page of results using `feed.ReadNextAsync`. Use `feed.HasMoreResults` to determine if there are any results left at the start of each loop.

```csharp
List<Product> items = new();
while (feed.HasMoreResults)
{
    FeedResponse<Product> response = await feed.ReadNextAsync();
    foreach (Product item in response)
    {
        items.Add(item);
    }
}
```

## Clean up resources

[!INCLUDE[Developer Quickstart cleanup](includes/quickstart/dev-cleanup.md)]

## Related content

- [Node.js Quickstart](quickstart-nodejs.md)
- [Python Quickstart](quickstart-python.md)
- [Java Quickstart](quickstart-java.md)
- [Go Quickstart](quickstart-go.md)

## Next step

> [!div class="nextstepaction"]
> [Tutorial: Develop a .NET console application](tutorial-dotnet-console-app.md)
