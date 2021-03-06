---
title: Aprovisionamiento del rendimiento de base de datos en Azure Cosmos DB
description: Aprenda a aprovisionar el rendimiento en el nivel de base de datos en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 759adf95604e66209cf3ec5083246d16e952114a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884195"
---
# <a name="provision-throughput-for-a-database-in-azure-cosmos-db"></a>Aprovisionamiento del rendimiento de una base de datos en Azure Cosmos DB

En este artículo se explica cómo aprovisionar el rendimiento de una base de datos en Azure Cosmos DB. Puede aprovisionar el rendimiento de un único [contenedor](how-to-provision-container-throughput.md), o bien de una base de datos y compartir el rendimiento entre los contenedores que se incluyen en ella. Para aprovisionar el rendimiento en el nivel de base de datos, se puede usar Azure Portal o los SDK de Azure Cosmos DB.

## <a name="provision-throughput-by-using-azure-portal"></a>Aprovisionamiento del rendimiento mediante Azure Portal

### <a id="portal-sql"></a>SQL API (Core)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. [Cree una nueva cuenta de Azure Cosmos DB](create-sql-api-dotnet.md#create-a-database-account) o seleccione una existente.

1. Abra el panel de **Data Explorer** y seleccione **Nueva base de datos**. Especifique los detalles siguientes:

   * Escriba un identificador de base de datos. 
   * Seleccione **Aprovisionar rendimiento**.
   * Escriba un rendimiento, por ejemplo 1000 RU.
   * Seleccione **Aceptar**.

![Captura de pantalla del cuadro de diálogo Nueva base de datos](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-net-sdk"></a>Aprovisionamiento del rendimiento mediante el SDK para .NET

> [!Note]
> Use SQL API para aprovisionar el rendimiento de todas las API. También puede usar el ejemplo siguiente para Cassandra API.

### <a id="dotnet-all"></a>Todas las API

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para aprender sobre el rendimiento de3 aprovisionamiento en Azure Cosmos DB:

* [How to provision throughput for a container](how-to-provision-container-throughput.md) (Aprovisionamiento del rendimiento de un contenedor)
* [Rendimiento y unidades de solicitud en Azure Cosmos DB](request-units.md)
