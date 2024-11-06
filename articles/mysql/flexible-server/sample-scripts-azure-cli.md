---
title: Azure CLI samples
description: This article lists the Azure CLI code samples available for interacting with Azure Database for MySQL - Flexible Server.
author: shreyaaithal
ms.author: shaithal
ms.reviewer: maghan
ms.date: 06/18/2024
ms.service: azure-database-mysql
ms.subservice: flexible-server
ms.topic: sample
ms.custom:
  - mvc
  - devx-track-azurecli
ms.devlang: azurecli
---
# Azure CLI samples for Azure Database for MySQL - Flexible Server

[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]

The following table includes links to sample Azure CLI scripts for Azure Database for MySQL Flexible Server.

| Sample link | Description  |
|---|---|
|**Create and connect to a server**||
| [Create a server and enable public access connectivity](scripts/sample-cli-create-connect-public-access.md) | Creates an Azure Database for MySQL Flexible Server instance, configures a server-level firewall rule (public access connectivity method) and connects to the server. |
| [Create a server and enable private access connectivity (VNet Integration)](scripts/sample-cli-create-connect-private-access.md) | Creates an Azure Database for MySQL Flexible Server instance in a VNet (private access connectivity method) and connects to the server through a VM within the VNet. |
|**Monitor and scale**||
| [Monitor metrics and scale a server](scripts/sample-cli-monitor-and-scale.md) | Monitors and scales a single Azure Database for MySQL Flexible Server instance up or down to allow for changing performance needs. |
|**Backup and restore**||
| [Restore a server](scripts/sample-cli-restore-server.md) | Restores a single Azure Database for MySQL Flexible Server instance to a previous point in time. |
|**High Availability**||
| [Configure zone-redundant high availability](scripts/sample-cli-zone-redundant-ha.md) | Enables Zone-Redundant high availability while creating an Azure Database for MySQL Flexible Server instance.|
| [Configure same-zone high availability](scripts/sample-cli-same-zone-ha.md) | Enables Same-Zone high availability while creating an Azure Database for MySQL Flexible Server instance.|
|**Manage server**||
| [Restart, Stop, Start a Server](scripts/sample-cli-restart-stop-start.md)| Performs restart, stop and start operations on a single Azure Database for MySQL Flexible Server instance. |
| [Change server parameters](scripts/sample-cli-change-server-parameters.md) | Changes server parameters of a single Azure Database for MySQL Flexible Server instance. |
|**Replication**||
| [Create read replicas](scripts/sample-cli-read-replicas.md) | Creates and manages read replicas in a single Azure Database for MySQL Flexible Server instance. |
|**Configure logs**||
| [Configure audit logs](scripts/sample-cli-audit-logs.md) | Configures audit logs on a single Azure Database for MySQL Flexible Server instance. |
| [Configure slow-query logs](scripts/sample-cli-slow-query-logs.md) | Configures slow-query logs on a single Azure Database for MySQL Flexible Server instance. |
