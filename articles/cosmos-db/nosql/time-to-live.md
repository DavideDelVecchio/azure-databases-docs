---
title: Expire data in Azure Cosmos DB with Time to Live
description: With TTL, Microsoft Azure Cosmos DB automatically purges documents from the system after a period of time.
author: markjbrown
ms.author: mjbrown
ms.service: azure-cosmos-db
ms.subservice: nosql
ms.topic: how-to
ms.date: 08/08/2024
---

# Time to Live (TTL) in Azure Cosmos DB

[!INCLUDE[NoSQL](../includes/appliesto-nosql.md)]

With **Time to Live** or TTL, Azure Cosmos DB deletes items automatically from a container after a certain time period. By default, you can set time to live at the container level and override the value on a per-item basis. After you set the TTL at a container or at an item level, Azure Cosmos DB will automatically remove these items after the time period, since the time they were last modified. Time to live value is configured in seconds. When you configure TTL, the system automatically deletes the expired items based on the TTL value, without needing a delete operation explicitly issued by the client application. The maximum value for TTL is 2,147,483,647 seconds, the approximate equivalent of 24,855 days or 68 years.

Expired items are deleted as a background task. An item will no longer appear in query responses immediately after the TTL expires, even if it hasn't yet been permanently deleted from the container. If the container does not have enough request units (RUs) to perform the deletion, the data deletion will be delayed. The data will be deleted once sufficient RUs are available to complete the deletion.

For provisioned throughput accounts, the deletion of expired items uses leftover RUs that haven't been consumed by user requests.

For serverless accounts, the deletion of expired items is charged in RUs at the same rate as delete item operations.

> [!NOTE]
> This content is related to Azure Cosmos DB transactional store TTL. If you are looking for analytical store TTL, that enables NoETL HTAP scenarios through [Azure Synapse Link](../synapse-link.md), please click [here](../analytical-store-introduction.md#analytical-ttl).

## Time to live for containers and items

The time to live value is set in seconds, and is interpreted as a delta from the time that an item was last modified. You can set time to live on a container or an item within the container:

1. **Time to Live on a container** (set using `DefaultTimeToLive`):

   - If missing (or set to null), items aren't expired automatically.

   - If present and the value is set to *"-1,"* it's equal to infinity, and items don’t expire by default.

   - If present and the value is set to some *nonzero* number *"n,"* items will expire *"n"* seconds after their last modified time.

2. **Time to Live on an item** (set using `ttl`):

   - This Property is applicable only if `DefaultTimeToLive` is present and it isn't set to null for the parent container.

   - If present, it overrides the `DefaultTimeToLive` value of the parent container.

## Time to Live configurations

- If TTL is set to *"n"* on a container, then the items in that container will expire after *n* seconds. If there are items in the same container that have their own time to live, set to -1 (indicating they don't expire). If some items override the time to live setting with a different number, these items expire based on their own configured TTL value.

- If TTL isn't set on a container, then the time to live on an item in this container has no effect.

- If TTL on a container is set to -1, an item in this container that has the time to live set to n, will expire after n seconds, and remaining items won't expire.

## Examples

This section shows some examples with different time to live values assigned to container and items:

> [!NOTE]
> Setting TTL to null on an item isn't supported. The item TTL value must be a nonzero positive integer less than or equal to 2147483647, or -1 which means the item will never expire. To use the default TTL on an item, ensure the TTL property isn't present.

### Example 1

TTL on container is set to null (DefaultTimeToLive = null)

|TTL on item| Result|
|---|---|
|ttl property missing |TTL is disabled. The item never expires (default).|
|ttl = -1|TTL is disabled. The item never expires.|
|ttl = 2000|TTL is disabled. The item never expires.|

### Example 2

TTL on container is set to -1 (DefaultTimeToLive = -1)

|TTL on item| Result|
|---|---|
|ttl property missing |TTL is enabled. The item never expires (default).|
|ttl = -1|TTL is enabled. The item never expires.|
|ttl = 2000|TTL is enabled. The item expires after 2,000 seconds.|

### Example 3

TTL on container is set to 1000 (DefaultTimeToLive = 1000)

|TTL on item| Result|
|---|---|
|ttl property missing |TTL is enabled. The item will expire after 1,000 seconds (default).|
|ttl = -1|TTL is enabled. The item will never expire.|
|ttl = 2000|TTL is enabled. The item will expire after 2,000 seconds.|

## Next steps

Learn how to configure Time to Live in the following articles:

- [How to configure Time to Live](how-to-time-to-live.md)
