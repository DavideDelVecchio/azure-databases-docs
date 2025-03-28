---
title: "Migration of extensions in Migration Service"
description: Learn about migration of extensions by using the migration service in Azure Database for PostgreSQL.
author: apduvuri
ms.author: adityaduvuri
ms.reviewer: maghan
ms.date: 03/28/2025
ms.service: azure-database-postgresql
ms.topic: concept-article
---

# Extension migration using the Migration service

[!INCLUDE [applies-to-postgresql-flexible-server](~/reusable-content/ce-skilling/azure/includes/postgresql/includes/applies-to-postgresql-flexible-server.md)]

PostgreSQL extensions enhance the database functionality by adding new features such as advanced indexing, time-series management, spatial data processing, and foreign data wrappers. Extensions provide more capabilities beyond these, such as performance optimization, data transformation, and integration with external systems.

The Migration service for Azure Database for PostgreSQL supports the migration of all extensions that are available in Azure Database for PostgreSQL flexible Server. To ensure a successful migration using the migration service, it's recommended to verify the extensions used in your source PostgreSQL instance. Extensions provide essential functionality that may be required for your application, and verifying them before migration helps in ensuring a smooth transition.

## Steps for migrating Extensions

To ensure a successful migration by using the migration service in Azure Database for PostgreSQL, you might need to verify extensions to your source PostgreSQL instance. Extensions provide functionality and features that might be required for your application. Make sure that you verify the extensions on the source PostgreSQL instance before you initiate the migration process.

In the target instance of Azure Database for PostgreSQL flexible server, enable supported extensions that are identified in the source PostgreSQL instance.

For more information, see [Extensions in Azure Database for PostgreSQL](../../../../flexible-server/concepts-extensions.md#how-to-use-postgresql-extensions).

> [!NOTE]  
> A restart is required when you make any changes to the `shared_preload_libraries` parameter.

## Configuration tables in Extensions

Some PostgreSQL extensions define configuration tables, which store metadata or settings needed for the extension’s functionality. These tables allow users to modify configurations based on their specific data requirements. The migration service supports migrating configuration tables for the following extensions:

- TimescaleDB
- postgis_topology
- postgis_tiger_geocode
- pg_partman
- postgres_fdw

The migration service considers configuration tables for migration if they're listed in the `extconfig` column of the `pg_extension` system catalog table. These tables contain critical settings or metadata required for the extension’s functionality. This behavior aligns with how `pg_dump` manages extension configuration tables, ensuring consistency in migration processes. For more information, see the PostgreSQL documentation: [pg_dump extend-extensions](https://www.postgresql.org/docs/current/extend-extensions.html#EXTEND-EXTENSIONS-CONFIG-TABLES)

> [!NOTE]  
  > Some configuration tables migration might fail due to the superuser privileges required in Azure Database for PostgreSQL flexible Server


## Considerations for postgres_fdw migration

The migration service doesn't support migrating the postgres_fdw extension in the following scenarios:
- When user/roles migration is disabled using the migration server parameter: `azure.migration_skip_role_user = on`.
- When the source PostgreSQL version is greater than 15.
- When the source database is from AWS, on-premises, GCP, or an Azure VM (excluding Azure Database for PostgreSQL - Single Server).

## Considerations for pg_partman migration
`pg_partman` (PG Partition Manager) is an extension that automates partition management for time-based and serial-based table partitioning. It simplifies partition handling by managing the creation, retention, and maintenance of partitions.

- **Breaking Change**: Migration from version 4.* to 5.* fails due to significant structural changes in the extension configuration tables.
- **Trigger-Based Partitioning**: pg_partman 5.x doesn't support trigger-based partitioning. Users must transition from trigger-based partitioning to native PostgreSQL partitions before migration.
- **Pre-Migration Consideration**: Users must ensure that no background job (for example, cron, BGP) is running that recreates partitions during the migration process.


## TimescaleDB Migration
The `TimescaleDB` extension is a time-series database packaged as an extension for PostgreSQL. It provides time-oriented analytical functions and optimizations and scales PostgreSQL for time-series workloads. [Learn more about TimescaleDB](https://docs.timescale.com/timescaledb/latest/), a registered trademark of Timescale, Inc. Azure Database for PostgreSQL flexible server provides the TimescaleDB [Apache-2 edition](https://www.timescale.com/legal/licenses), and the migration service supports the same.

> [!NOTE]  
  > Only **Offline** migration is possible for time series databases.


By ensuring proper verification and configuration of extensions before migration, you can facilitate a seamless transition to Azure Database for PostgreSQL flexible Server using Migration service for Azure Database for PostgreSQL.


## Related content

- [Migration service](concepts-migration-service-postgresql.md)
- [Known issues and limitations](concepts-known-issues-migration-service.md)
- [Network setup](how-to-network-setup-migration-service.md)
