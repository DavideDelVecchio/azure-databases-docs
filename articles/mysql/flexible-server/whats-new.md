---
title: "What's New in Azure Database for MySQL"
description: Learn about recent updates to Azure Database for MySQL.
author: SudheeshGH
ms.author: sunaray
ms.reviewer: vamehta, maghan
ms.date: 05/02/2025
ms.service: azure-database-mysql
ms.subservice: flexible-server
ms.topic: overview
---

# What's new in Azure Database for MySQL?

[What is Azure Database for MySQL](overview.md) Is an Azure Database service that provides more granular control and flexibility over database management functions and configuration settings. The service currently supports the community versions of MySQL 5.7 and 8.0.

This article summarizes new releases and features in the Azure Database for MySQL service.

For previously released features, visit [Azure Database for MySQL: Previous released features](whats-new-archive.md).

> [!NOTE]  
> This article references the term slave, which Microsoft no longer uses. When the term is removed from the software, we remove it from this article.

## May 2025

### Enable Auto-Scale of IOPS for faster restore and replica provisions

Azure Database for MySQL now supports the ability to enable [auto-scaling of IOPS](https://techcommunity.microsoft.com/blog/adformysql/autoscale-iops-for-azure-database-for-mysql---flexible-server---general-availabi/3884602) for both the source and target servers during restore operations and replica provisioning workflows. This enhancement helps accelerate the restore and replica provisioning process by temporarily boosting IOPS to meet the performance demands of these operations. Once provisioning is complete, you can disable the auto-scale IOPS setting. To learn more, visit [How to restore a server](./how-to-restore-server-portal.md).

### Configure backup interval for the automated backups (Preview)

Azure Database for MySQL now supports ability to configure backup interval for the automatic backup’s taken by the system to improve restore speed. The feature is currently available in limited regions, namely – West Central US and East Asia. Learn more about the feature [here](./concepts-backup-restore.md#backup-frequency).

## March 2025

### New Built-in Stored Procedures for Plugin Management and Undo Log Cleanup

There are two new built-in stored procedures in Azure Database for MySQL, allowing customers to manage plugin settings and clean up undo logs without requiring support intervention:

- Validate Password Plugin Management:
  - Enable: `CALL az_install_validate_password_plugin();`
  - Disable: `CALL az_uninstall_validate_password_plugin();`
  - Once enabled, the plugin's configuration parameters are available in the **Server Parameters**  page on the Azure portal.

- Undo Log Cleanup:
  - A new stored procedure is available to manually clean up the **Undo Log**, preventing unnecessary storage consumption.

Refer to the [Built-in stored procedures in Azure Database for MySQL](./concepts-built-in-store-procedure.md) article to learn more about Azure Database for MySQL built-in store procedure

### Caching SHA-2 Password Plugin Now Exposed by Default

The `caching_sha2_password` plugin is now exposed to customers by default. Customers can enable and configure it by setting the relevant **Server Parameters** in the Azure portal.

## February 2025

### Known Issues

- Azure Advisor recommendations might continue to recommend enabling accelerated logs even after the feature is enabled on your Azure Database for MySQL server.

- For servers with [customer managed keys (CMK)](./concepts-customer-managed-key.md), enabling [accelerated logs](./concepts-accelerated-logs.md) might not work due to a current limitation. As a workaround, you can temporarily disable CMK, enable accelerated logs, and then re-enable CMK. We're actively working to resolve this issue at the earliest. For more information, see the [accelerated logs documentation](./concepts-accelerated-logs.md).

## January 2025

### Default zone-resiliency for Business-Critical service tier

You now benefit from the highest level of availability against infrastructure failures within an availability zone, at no extra cost for mission-critical workloads running on the Business-Critical service tier. Regardless of whether your flexible servers are enabled with High Availability (HA), your server data and log files are hosted in zone-redundant storage by default. While zone-redundant HA-enabled servers continue to benefit from a 99.99% uptime SLA from the built-in zonal redundancy and hot standby, non-HA servers are able to recover quickly from zonal outages using zone-redundant backups. This enhancement is applicable to all new server provisioned in Business-Critical service tier.

### Accelerated Logs enabled for all new business critical servers

Accelerated Logs, a feature that significantly enhances the performance of Azure Database for MySQL flexible server instances, is now enabled by default for all new Business-Critical servers. Accelerated Logs offers a dynamic solution designed for high throughput needs, reducing latency with no extra cost. Existing Business Critical servers can also enable Accelerated Logs through the Azure portal. [Accelerated logs feature in Azure Database for MySQL ](concepts-accelerated-logs.md).

## November 2024

### MySQL 8.4 LTS version support - Public Preview

Azure Database for MySQL now supports MySQL 8.4 LTS version, bringing the latest MySQL capabilities to Azure. MySQL 8.4 LTS version offers enhanced replication features, expanded monitoring, and long-term support, making it ideal for production environments requiring stability and advanced management. [Azure Database for MySQL version support policy](../concepts-version-policy.md)

### MySQL 9.1 innovation version support - Public Preview

Azure Database for MySQL now supports MySQL 9.1 Innovation version, introduces experimental features, including JavaScript support for stored procedures and the new vector data type, designed for modern applications in machine learning and analytics. [Azure Database for MySQL version support policy](../concepts-version-policy.md)

## October 2024

**New B1s Servers are unavailable**

To ensure performance and reliability in light of ongoing security improvements, the creation of new B1s servers will be limited starting November 1st 2024. We recommend exploring alternative SKUs designed to better support your needs while maintaining high service reliability. Thank you for your understanding as we work to improve your experience.

**What does this mean for you?**

- Limited Access: New B1s instances will be unavailable after 10/28/24.

- Existing Deployments: Any current B1s instances will remain unaffected.
- Alternative Options: Explore and consider using other SKUs, such as B1ms and B2s, which provide enhanced compute power and memory while supporting greater performance.

**Next Steps:**
- Review recommended alternative SKUs
- Access documentation for changing your SKU here:
  - [Azure portal](/azure/mysql/flexible-server/how-to-manage-server-portal)
  - [Azure CLI](/azure/mysql/flexible-server/how-to-manage-server-cli)
- Review the Usage Dashboard to monitor your current usage

## September 2024

- **Azure Database for MySQL Flexible Maintenance is now Generally Available**

  Azure Database for MySQL has officially released the Flexible Maintenance feature to General Availability (GA). With this feature, users can now reschedule maintenance activities via the Azure CLI, providing enhanced control and flexibility. The reschedule window has also been expanded to cover all available maintenance dates within the same region and same round of maintenance, instead of being limited to the 14-day window around the instance's originally scheduled date.

To learn more, visit [Scheduled maintenance](./concepts-maintenance.md#maintenance-reschedule).

## August 2024

- **Azure Database for MySQL flexibls server now supports up to 8 TB in a single data file!**

  Azure Database for MySQL now supports single InnoDB data files up to **8 TB** in size, enabling users to store larger datasets within a single file. This enhancement reduces the need for data partitioning and streamlines database management, making it easier to handle substantial volumes of data using the InnoDB storage engine. [Learn more.](./concepts-server-parameters.md#innodb_file_per_table)

- **Major version upgrade support for Burstable compute tier**

  Azure Database for MySQL now offers major version upgrades for Burstable SKU compute tiers. This support automatically upgrades the compute tier to General Purpose SKU before performing the upgrade, ensuring sufficient resources. Customers can choose to revert back to Burstable SKU after the upgrade. Additional costs might apply. [Learn more](how-to-upgrade.md#perform-a-planned-major-version-upgrade-from-mysql-57-to-mysql-80-using-the-azure-portal-for-burstable-sku-servers)

- **Deprecated Metrics Announcement**

  We would like to inform you about the deprecation of the following metrics in Azure Database for MySQL Flexible Server.
  - Storage Throttle Count (deprecated)
  - Available Memory Bytes (deprecated)
  - MySQL Lock Row Lock Waits (deprecated)

  These metrics are no longer supported and are removed from the monitoring by the end of August 2024. We recommend updating your monitoring and alerting configurations to exclude these metrics and use alternative metrics that provide similar insights. 

## July 2024

- **Move from private access (virtual network integrated) network to public access or private link**

  Azure Database for MySQL Flexible Server can be transitioned from private access (virtual network Integrated) to public access, with the option to use Private Link. This functionality enables servers to switch from virtual network integrated to Private Link/Public infrastructure seamlessly, without the need to alter the server name or migrate data, simplifying the process for customers. [Learn more](concepts-networking-vnet.md#move-from-private-access-virtual-network-integrated-network-to-public-access-or-private-link)

- **Managed HSM support for Azure Database for MySQL Flexible Server (Generally Available)**

  We're excited to announce the General Availability (GA) of Azure Key Vault Managed HSM support for Customer Managed Keys (CMK) in Azure Database for MySQL Flexible Server. With Managed HSM you can import your own HSM-backed encryption keys using the CMK bring your own key (BYOK) feature to protect data at rest in your Azure Database for MySQL Flexible Server instances while maintaining data residency and full control of your HSM keys. [Learn more](/azure/mysql/flexible-server/concepts-customer-managed-key)

## May 2024

- **Accelerated Logs in Azure Database for MySQL Flexible Server is now Generally Available**

  We're thrilled to announce the General Availability (GA) of the Accelerated Logs feature for Azure Database for MySQL Flexible Server. This feature is available within the Business-Critical service tier, which significantly enhances the performance of Azure Database for MySQL Flexible Server instances. It offers a dynamic solution designed for high throughput needs, reducing latency with no additional cost. [Accelerated logs feature in Azure Database for MySQL - Flexible Server](concepts-accelerated-logs.md).

- **Support for storage up to 32 TB in Azure Database for MySQL Flexible Server is now Generally Available**

  We're excited to announce the general availability (GA) of storage support up to 32 TB in Azure Database for MySQL Flexible Server. The feature is available only in the Business Critical service tier. To learn more, visit [service tiers](concepts-service-tiers-storage.md).

## April 2024

- **Enhanced Memory Allocation in Azure Database for MySQL Flexible Server**

  In the April deployments, we introduced optimized memory allocation for Azure Database for MySQL Flexible Server. This refinement ensures a more accurate and efficient memory calculation for the MySQL Server component, allowing it to utilize available resources effectively for query processing and data management. [Azure Database for MySQL - Flexible Server service tiers](concepts-service-tiers-storage.md).

- **Enhanced Monitoring for Azure Database for MySQL Flexible Server: Introducing New Metrics**

  The newly added metrics include MySQL Uptime, MySQL History list length, MySQL Deadlocks, Active Transactions, and MySQL Lock Timeouts. These metrics provide a more detailed view of your server's performance, enabling you to monitor and optimize your database operations more effectively. In addition to these new metrics, we've also improved the Memory percent metric. It now offers more precise calculations of memory usage for the MySQL server (mysqld) process. [Monitor Azure Database for MySQL - Flexible Server](concepts-monitoring.md)

- **Microsoft Defender for Cloud supports Azure Database for MySQL Flexible Server (General Availability)**

  We're excited to announce the general availability of the Microsoft Defender for Cloud feature for Azure Database for MySQL Flexible Server in all service tiers. The Microsoft Defender Advanced Threat Protection feature simplifies the security management of Azure Database for MySQL Flexible Server instances. It monitors the server for anomalous or suspicious database activities to detect potential threats and provides security alerts for you to investigate and take appropriate action, allowing you to actively improve the security posture of your database without being a security expert. [What is Microsoft Defender for open-source relational databases](/azure/defender-for-cloud/defender-for-databases-introduction)
- **On-demand backup and Export (Preview)**

  Azure Database for MySQL Flexible Server now allows triggering an on-demand server backup and exporting it to an Azure storage account (Azure blob storage). The feature is currently in public preview and available only in public cloud regions. [Learn more](./concepts-backup-restore.md#on-demand-backup-and-export-preview)
- **Known Issues**

  While attempting to enable the Microsoft Defender for Cloud feature for an Azure Database for MySQL Flexible Server, you might encounter the following error: 'The server <server_name> is incompatible with Advanced Threat Protection. Contact Microsoft support to update the server to a supported version.' This issue can occur on MySQL Flexible Servers still awaiting an internal update. It's automatically resolved in the following internal update of your server. Alternatively, you can open a support ticket to expedite an immediate update."

## March 2024

- **Accelerated Logs now supports major version upgrades.**

   Accelerated Logs has now introduced support for [major version upgrade](how-to-upgrade.md), allowing an upgrade from MySQL version 5.7 to MySQL version 8.0 with accelerated logs feature enabled.[Accelerated logs feature in Azure Database for MySQL - Flexible Server](concepts-accelerated-logs.md)

- **Support for Long-term retention of backups in Azure Database for MySQL Flexible Server (Preview)**

  This feature allows backups to be retained for up to 35 days and up to 10 years. [Backup and restore in Azure Database for MySQL - Flexible Server](concepts-backup-restore.md)

## February 2024

- **Accelerated Logs is now available for existing servers and three new regions.**

  Accelerated Logs, previously limited to servers created after November 14, is now accessible for all existing Business Critical tier's **standalone** servers in the preview phase. Accelerated logs also support [Microsoft Entra authentication for Azure Database for MySQL - Flexible Server](concepts-azure-ad-authentication.md). Additionally, this feature has been extended to include three new regions: Japan East, Korea Central, and Poland Central. [Accelerated logs feature in Azure Database for MySQL - Flexible Server](concepts-accelerated-logs.md)

- **Known Issues**

  Due to a technical issue in this month's deployment, primary servers with read-replica are temporarily restricted from enabling the [accelerated logs](concepts-accelerated-logs.md) feature. Users should turn off the accelerated logs feature before creating a replica server. If you require assistance with accelerated logs and replica creation, open a [support ticket](https://azure.microsoft.com/support/create-ticket) for assistance. [Learn more](./concepts-accelerated-logs.md#limitations).

- **Audit logs now support wild card entries**

  The server parameters now support wildcards in `audit_log_include_users` and `audit_log_exclude_users`, enhancing flexibility for specifying user inclusions and exclusions in audit logs. [Learn more](./concepts-audit-logs.md#configure-audit-logging)

- **Enhanced Audit Logging with CONNECTION_V2 for Comprehensive MySQL User Audits**

  Server parameter [audit_log_events](./concepts-audit-logs.md#configure-audit-logging) now supports event CONNECTION_V2 for detailed connection logs, providing insights into user audits, connection status, and [error codes in MySQL](https://dev.mysql.com/doc/mysql-errors/8.0/en/server-error-reference.html) interactions.[Learn more](./concepts-audit-logs.md#analyze-logs-in-azure-monitor-logs)

## Feedback and support

If you have questions about or suggestions for working with Azure Database for MySQL, consider the following points of contact as appropriate:

- To contact Azure Support, [file a ticket from the Azure portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- To fix an issue with your account, file a [support request](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) in the Azure portal.

## Related content

- [Azure Database for MySQL pricing](https://azure.microsoft.com/pricing/details/mysql/server/)
- [Azure Database for MySQL documentation](../index.yml)
- [Troubleshoot errors](how-to-troubleshoot-common-errors.md)
- [Azure Database for MySQL: Previous released features](whats-new-archive.md)
