---
title: MongoDB per-document TTL feature in Azure Cosmos DB
description: Learn how to set time to live value for documents using Azure Cosmos DB for MongoDB, to automatically purge them from the system after a period of time.
author: gahl-levy
ms.author: gahllevy
ms.service: azure-cosmos-db
ms.subservice: mongodb
ms.topic: how-to
ms.devlang: csharp
ms.date: 08/21/2025
ms.custom:
  - devx-track-csharp
---
# Expire data with Azure Cosmos DB for MongoDB
[!INCLUDE[MongoDB](~/reusable-content/ce-skilling/azure/includes/cosmos-db/includes/appliesto-mongodb.md)]

Time-to-live (TTL) functionality allows the database to automatically expire data. Azure Cosmos DB for MongoDB utilizes Azure Cosmos DB's core TTL capabilities. Two modes are supported: setting a default TTL value on the whole collection, and setting individual TTL values for each document. The logic governing TTL indexes and per-document TTL values in Azure Cosmos DB for MongoDB is the [same as in Azure Cosmos DB](indexing.md).

## TTL indexes
To enable TTL universally on a collection, a ["TTL index" (time-to-live index)](indexing.md#index-properties) needs to be created. The TTL index is an index on the `_ts` field with an "expireAfterSeconds" value.

MongoShell example:

```
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```
The command in the previous example creates an index with TTL functionality. 

The output of the command includes various metadata:

```output
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : true,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

 Once the index is created, the database will automatically delete any documents in that collection that haven't been modified in the last 10 seconds. 

> [!NOTE]
> `_ts` is an Azure Cosmos DB-specific field and isn't accessible from MongoDB clients. It's a reserved (system) property that contains the timestamp of the document's last modification.

Java example:

```java
MongoCollection collection = mongoDB.getCollection("collectionName");
String index = collection.createIndex(Indexes.ascending("_ts"),
new IndexOptions().expireAfter(10L, TimeUnit.SECONDS));
```

C# example: 

```csharp
var options = new CreateIndexOptions {ExpireAfter = TimeSpan.FromSeconds(10)}; 
var field = new StringFieldDefinition<BsonDocument>("_ts"); 
var indexDefinition = new IndexKeysDefinitionBuilder<BsonDocument>().Ascending(field); 
await collection.Indexes.CreateOneAsync(indexDefinition, options); 
``` 

## Set time to live value for a document 
Per-document TTL values are also supported. One or more documents must contain a root-level property "ttl" (lower-case), and a TTL index as described previously must have been created for that collection. TTL values set on a document override the collection's TTL value.

The TTL value must be an int32. Alternatively, an int64 that fits in an int32, or a double with no decimal part that fits in an int32. Values for the TTL property that don't conform to these specifications are allowed but not treated as a meaningful document TTL value.

The TTL value for the document is optional; documents without a TTL value can be inserted into the collection. In this case, the collection's TTL value is honored. 

The following documents have valid TTL values. Once the documents are inserted, the document TTL values override the collection's TTL values. So, the documents will be removed after 20 seconds. 	

```JavaScript 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: 20.0}) 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: NumberInt(20)}) 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: NumberLong(20)}) 
```

The following documents have invalid TTL values. The documents are inserted, but the document TTL value won't be honored. So, the documents will be removed after 10 seconds because of the collection's TTL value. 

```JavaScript 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: 20.5}) //TTL value contains non-zero decimal part. 
globaldb:PRIMARY> db.coll.insert({id:1, location: "Paris", ttl: NumberLong(2147483649)}) //TTL value is greater than Int32.MaxValue (2,147,483,648). 
``` 

## Next step

> [!div class="nextstepaction"]
> [Indexing your Azure Cosmos DB database configured with Azure Cosmos DB for MongoDB](../mongodb/indexing.md)
