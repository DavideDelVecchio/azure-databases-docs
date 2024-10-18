---
title: Quickstart - Java client library
titleSuffix: Azure Cosmos DB for NoSQL
description: Deploy a Java Spring Web application that uses the client library to interact with Azure Cosmos DB for NoSQL data in this quickstart.
author: seesharprun
ms.author: sidandrews
ms.service: azure-cosmos-db
ms.subservice: nosql
ms.devlang: java
ms.topic: quickstart-sdk
ms.date: 10/15/2024
ms.custom: devx-track-extended-java, devx-track-extended-azdevcli
zone_pivot_groups: azure-cosmos-db-quickstart-env
# CustomerIntent: As a developer, I want to learn the basics of the Java library so that I can build applications with Azure Cosmos DB for NoSQL.
---

# Quickstart: Azure Cosmos DB for NoSQL library for Java

[!INCLUDE[NoSQL](../includes/appliesto-nosql.md)]

[!INCLUDE[Developer Quickstart selector](includes/quickstart/dev-selector.md)]

Get started with the Azure Cosmos DB for NoSQL client library for Java to query data in your containers and perform common operations on individual items. Follow these steps to deploy a minimal solution to your environment using the Azure Developer CLI.

[API reference documentation](/java/api/overview/azure/cosmos-readme) | [Library source code](https://github.com/azure/azure-sdk-for-java/tree/main/sdk/cosmos/azure-cosmos) | [Package (Maven)](https://central.sonatype.com/artifact/com.azure/azure-cosmos) | [Azure Developer CLI](/azure/developer/azure-developer-cli/overview)

## Prerequisites

[!INCLUDE[Developer Quickstart prerequisites](includes/quickstart/dev-prereqs.md)]

## Setting up

Deploy this project's development container to your environment. Then, use the Azure Developer CLI (`azd`) to create an Azure Cosmos DB for NoSQL account and deploy a containerized sample application. The sample application uses the client library to manage, create, read, and query sample data.

::: zone pivot="devcontainer-codespace"

[![Open in GitHub Codespaces](https://img.shields.io/static/v1?style=for-the-badge&label=GitHub+Codespaces&message=Open&color=brightgreen&logo=github)](https://codespaces.new/azure-samples/cosmos-db-nosql-java-quickstart?template=false&quickstart=1&azure-portal=true)

::: zone-end

::: zone pivot="devcontainer-vscode"

[![Open in Dev Container](https://img.shields.io/static/v1?style=for-the-badge&label=Dev+Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/azure-samples/cosmos-db-nosql-java-quickstart)

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
    azd init --template cosmos-db-nosql-java-quickstart
    ```

    > [!NOTE]
    > This quickstart uses the [azure-samples/cosmos-db-nosql-java-quickstart](https://github.com/azure-samples/cosmos-db-nosql-java-quickstart) template GitHub repository. The Azure Developer CLI will automatically clone this project to your machine if it is not already there.

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

The client library is available through Maven, as the `azure-spring-data-cosmos` package.

1. Navigate to the `/src/web` folder and open the **pom.xml** file.

1. If it doesn't already exist, add an entry for the `azure-spring-data-cosmos` package.

    ```xml
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-spring-data-cosmos</artifactId>
    </dependency>
    ```

1. Also, add another dependency for the `azure-identity` package if it doesn't already exist.

    ```xml
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-identity</artifactId>
    </dependency>
    ```

## Object model

| Name | Description |
| --- | --- |
| `EnableCosmosRepositories` | This type is a method decorator used to configure a repository to access Azure Cosmos DB for NoSQL. |
| `CosmosRepository` | This class is the primary client class and is used to manage data within a container. |
| `CosmosClientBuilder` | This class is a factory used to create a client used by the repository. |
| `Query` | This type is a method decorator used to specify the query that the repository executes. |

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

First, this sample creates a new class that inherits from `AbstractCosmosConfiguration` to configure the connection to Azure Cosmos DB for NoSQL.

```java
@Configuration
@EnableCosmosRepositories
public class CosmosConfiguration extends AbstractCosmosConfiguration {

}
```

Within the configuration class, this sample creates a new instance of the `CosmosClientBuilder` class and configures authentication using a `DefaultAzureCredential` instance.

```java
    @Bean
    public CosmosClientBuilder getCosmosClientBuilder() {
        DefaultAzureCredential credential = new DefaultAzureCredentialBuilder()
            .build();
            
        return new CosmosClientBuilder()
            .endpoint("<azure-cosmos-db-nosql-account-endpoint>")
            .credential(credential);
    }
```

### Get a database

In the configuration class, the sample implements a method to return the name of the existing database named *`cosmicworks`*.

```java
@Override
protected String getDatabaseName() {
    return "cosmicworks";
}
```

### Get a container

Use the `Container` method decorator to configure a class to represent items in a container. Author the class to include all of the members you want to serialize into JSON. In this example, the type has a unique identifier, and fields for category, name, quantity, price, and clearance.

```java
@Container(containerName = "products", autoCreateContainer = false)
public class Item {
    private String id;
    private String name;
    private Integer quantity;
    private Boolean sale;

    @PartitionKey
    private String category;

    // Extra members omitted for brevity
}
```

### Create an item

Create an item in the container using `repository.save`.

```java
Item item = new Item(
    "70b63682-b93a-4c77-aad2-65501347265f",
    "gear-surf-surfboards",
    "Yamba Surfboard",
    12,
    false
);
Item created_item = repository.save(item);
```

### Read an item

Perform a point read operation by using both the unique identifier (`id`) and partition key fields. Use `repository.findById` to efficiently retrieve the specific item.

```java
PartitionKey partitionKey = new PartitionKey("gear-surf-surfboards");
Optional<Item> existing_item = repository.findById("70b63682-b93a-4c77-aad2-65501347265f", partitionKey);
if (existing_item.isPresent()) {
    // Do something
}
```

### Query items

Perform a query over multiple items in a container by defining a query in the repository's interface. This sample uses the `Query` method decorator to define a method that executes this parameterized query:

```nosql
SELECT * FROM products p WHERE p.category = @category
```

```java
@Repository
public interface ItemRepository extends CosmosRepository<Item, String> {

    @Query("SELECT * FROM products p WHERE p.category = @category")
    List<Item> getItemsByCategory(@Param("category") String category);

}
```

Fetch all of the results of the query using `repository.getItemsByCategory`. Loop through the results of the query.

```java
List<Item> items = repository.getItemsByCategory("gear-surf-surfboards");
for (Item item : items) {
    // Do something
}
```

## Related content

- [.NET Quickstart](quickstart-dotnet.md)
- [Node.js Quickstart](quickstart-nodejs.md)
- [java Quickstart](quickstart-java.md)
- [Go Quickstart](quickstart-go.md)

## Next step

> [!div class="nextstepaction"]
> [Tutorial: Build a Java web app](tutorial-java-web-app.md)
