---

copyright:
  years: 2018, 2021
lastupdated: "2021-02-02"

keywords: nodesync, repair, cassandra, datastax, dse
subcollection: databases-for-cassandra

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Getting Started
{: #getting-started}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-cassandra_full}} deployment. 

## Before you begin

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){:new_window}.
- And a {{site.data.keyword.databases-for-cassandra}} deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-cassandra). Give your deployment a memorable name that appears in your account's Resource List.
- [Set the Admin Password](/docs/databases-for-cassandra?topic=databases-for-cassandra-admin-password) for your deployment.

Review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-cassandra_full}} deployment.

## Connecting with DataStax drivers

Drivers are a key component to connecting external applications to your {{site.data.keyword.databases-for-cassandra}} deployment. 
 
Review the information on [Connecting an external application](/docs/databases-for-cassandra?topic=databases-for-cassandra-external-app) for details on compatible drivers. 

Deeper details on the specific drivers, including upgrade guides, can be found on the DataStax [Developing applications with Apache Cassandra and DataStax Enterprise](https://docs.datastax.com/en/devapp/doc/devapp/aboutDrivers.html){: external} page. 
 
Only DataStax drivers that are explicitly stated in the table at [Connecting an external application](/docs/databases-for-cassandra?topic=databases-for-cassandra-external-app) function correctly for connecting to {{site.data.keyword.databases-for-cassandra}}.
{: .tip} 

## Node repairs with NodeSync

[NodeSync](https://docs.datastax.com/en/dse/6.7/dse-admin/datastax_enterprise/config/aboutNodesync.html){: external} is a continuous repair service that runs in the background to validate data is in sync on all replicas. It is automatic and reduces the need for manual repairs. For operational simplicity and performance, {{site.data.keyword.databases-for-cassandra}} enables the NodeSync service on all keyspaces and tables to handle node repairs. Traditional repairs are unnecessary, as the automation ensures that NodeSync handles the repairs for you.  

While the automation enables NodeSync for you, you can also manually create tables with NodeSync enabled to ensure that the service can repair the tables' data when necessary: 
```
CREATE TABLE myTable (...) WITH nodesync = { 'enabled': 'true'};
```
{: .pre}

Review the detailed DataStax documentation on [enabling the NodeSync service](https://docs.datastax.com/en/dse/6.7/dse-admin/datastax_enterprise/config/enablingNodesync.html){: external}.  

Nodetool is unsupported. Manual repairs that are issued against any table that is enabled with the NodeSync service [are ignored](https://docs.datastax.com/en/opscenter/6.5/opsc/online_help/services/opscNodeSyncService.html#NodeSyncServiceversusRepairService){: external}:
{: .tip}
```
WARNING: A manual nodetool repair or a repair operation from the OpsCenter node administration menu fails to run if a NodeSync-enabled table is targeted.
```

## Recommendations
### Benchmark before production
- Neither `cqlsh` and `COPY` are not recommended for benchmarking, as `COPY` does not mimic typical client behavior. Instead, [`nosqlbench`](https://github.com/nosqlbench/nosqlbench) can be used for benchmarking 

### Data migrations
- `DSBULK` is recommended for data migration. See the [DSBULK documentation](https://docs.datastax.com/en/dsbulk/doc/dsbulk/reference/dsbulkCmd.html) from DataStax for more information. 

### Resource configurations
- The recommended configuration for a node is: 
  - 16 CPUs 
  - 32 GB to 64 GB RAM 
  - 16 K disk IOPS (16 k IOPS == 1.6 TB disk)

## Next steps

Detailed information on CQL, the Cassandra Query Language, can be found by consulting [CQL for DSE Documentation](https://docs.datastax.com/en/dse/6.0/cql/).

Looking to administer your deployment? Consult DataStax's documentation on using the the [standalone CQLSH client](https://docs.datastax.com/en/astra/docs/connecting-to-databases-using-standalone-cqlsh.html). 

You can manage your deployment with [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli), the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or by using the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you are planning to use {{site.data.keyword.databases-for-cassandra}} for your applications, check out some of our other documentation pages.
- [Connecting an external application](/docs/databases-for-cassandra?topic=databases-for-cassandra-external-app)
- [Connecting an IBM Cloud application](/docs/databases-for-cassandra?topic=databases-for-cassandra-ibmcloud-app)

Also, to ensure the stability of your applications and your database, check out the pages on 
- [High-Availability](/docs/databases-for-cassandra?topic=databases-for-cassandra-high-availability)
- [Performance](/docs/databases-for-cassandra?topic=databases-for-cassandra-performance)