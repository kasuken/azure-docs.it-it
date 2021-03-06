---
title: Copiare dati da o verso Azure Cosmos DB usando Data Factory | Microsoft Docs
description: Informazioni su come copiare dati da archivi dati di origine supportati da o verso Azure Cosmos DB in archivi sink supportati usando Data Factory.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: jingwang
ms.openlocfilehash: 9a75ae8645503366a490dbc0ea65d2fdc73d7c61
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167291"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-by-using-azure-data-factory"></a>Copiare dati da o verso Azure Cosmos DB usando Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versione 1](v1/data-factory-azure-documentdb-connector.md)
> * [Versione corrente](connector-azure-cosmos-db.md)

Questo articolo illustra come usare l'attività di copia in Azure Data Factory per copiare dati da e verso Azure Cosmos DB (API SQL). È basato sull'articolo [Attività di copia in Azure Data Factory](copy-activity-overview.md), che presenta una panoramica generale dell'attività di copia.

## <a name="supported-capabilities"></a>Funzionalità supportate

È possibile copiare dati da Azure Cosmos DB in qualsiasi archivio dati di sink supportato o da qualsiasi archivio dati di origine supportato in Azure Cosmos DB. Per un elenco degli archivi dati supportati dall'attività di copia come origini e sink, vedere [Archivi dati e formati supportati](copy-activity-overview.md#supported-data-stores-and-formats).

È possibile usare il connettore Azure Cosmos DB per:

- Copiare dati da e verso l'[API SQL](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction) di Azure Cosmos DB.
- Scrivere in Azure Cosmos DB tramite **insert** o **upsert**.
- Importare ed esportare documenti JSON così come sono o copiare dati da o in un set di dati tabulari, ad esempio un database SQL e un file CSV. Per copiare documenti così come sono da o verso file JSON o da o verso un'altra raccolta di Azure Cosmos DB, vedere [Importare o esportare documenti JSON](#importexport-json-documents).

Data Factory si integra con la [libreria dell'executor bulk di Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) per offrire le migliori prestazioni durante la scrittura in Azure Cosmos DB.

> [!TIP]
> Il [video sulla migrazione dei dati](https://youtu.be/5-SRNiC_qOU) illustra la procedura di copia dei dati da Archiviazione BLOB di Azure ad Azure Cosmos DB. Il video presenta inoltre alcune considerazioni generali sull'ottimizzazione delle prestazioni per l'inserimento di dati in Azure Cosmos DB.

## <a name="get-started"></a>Attività iniziali

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Le sezioni seguenti presentano informazioni dettagliate sulle proprietà che è possibile usare per definire entità di Data Factory specifiche per Azure Cosmos DB.

## <a name="linked-service-properties"></a>Proprietà del servizio collegato

Per il servizio collegato di Azure Cosmos DB sono supportate le proprietà seguenti:

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** deve essere impostata su **CosmosDb**. | Yes |
| connectionString |Specificare le informazioni richieste per connettersi al database di Azure Cosmos DB.<br /><br />**Nota**: è necessario specificare le informazioni sul database nella stringa di connessione come illustrato negli esempi seguenti. Contrassegnare questo campo come tipo **SecureString** per archiviare la password in modo sicuro in Data Factory. È anche possibile [fare riferimento a un segreto archiviato in Azure Key Vault](store-credentials-in-key-vault.md). |Yes |
| connectVia | [Runtime di integrazione](concepts-integration-runtime.md) da usare per la connessione all'archivio dati. È possibile usare Azure Integration Runtime o un runtime di integrazione self-hosted (se l'archivio dati si trova in una rete privata). Se questa proprietà non è specificata, viene usato il tipo Azure Integration Runtime predefinito. |No  |

**Esempio**

```json
{
    "name": "CosmosDbLinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Proprietà dei set di dati

Questa sezione presenta un elenco delle proprietà supportate dal set di dati Azure Cosmos DB. 

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione dei set di dati, vedere [Set di dati e servizi collegati](concepts-datasets-linked-services.md). 

Per copiare dati da o verso Azure Cosmos DB, impostare la proprietà **type** del set di dati su **DocumentDbCollection**. Sono supportate le proprietà seguenti:

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** del set di dati deve essere impostata su **DocumentDbCollection**. |Yes |
| collectionName |Nome della raccolta documenti di Azure Cosmos DB. |Yes |

**Esempio**

```json
{
    "name": "CosmosDbDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schema da Data Factory

Per gli archivi dati privi di schema come Azure Cosmos DB, l'attività di copia deduce lo schema in uno dei modi descritti nell'elenco seguente. A meno che non si voglia [importare o esportare documenti JSON come così sono](#import-or-export-json-documents), la procedura consigliata è quella di specificare la struttura di dati nella sezione **structure**.

* Se si specifica la struttura dei dati usando la proprietà **structure** nella definizione del set di dati, Data Factory considera la struttura come schema. 

    Se una riga non contiene un valore per una colonna, viene inserito un valore null per il valore di colonna.
* Se non si specifica la struttura dei dati usando la proprietà **structure** nella definizione del set di dati, il servizio Data Factory deduce lo schema usando la prima riga di dati. 

    Se la prima riga non contiene lo schema completo, alcune colonne non saranno presenti nel risultato dell'operazione di copia.

## <a name="copy-activity-properties"></a>Proprietà dell'attività di copia

Questa sezione presenta un elenco delle proprietà supportate dall'origine e dal sink Azure Cosmos DB.

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, vedere [Pipelines](concepts-pipelines-activities.md) (Pipeline).

### <a name="azure-cosmos-db-as-source"></a>Azure Cosmos DB come origine

Per copiare dati da Azure Cosmos DB, impostare il tipo **source** nell'attività di copia su **DocumentDbCollectionSource**. 

Nella sezione **source** dell'attività di copia sono supportate le proprietà seguenti:

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** dell'origine dell'attività di copia deve essere impostata su **DocumentDbCollectionSource**. |Yes |
| query |Specificare la query Azure Cosmos DB per leggere i dati.<br/><br/>Esempio:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |No  <br/><br/>Se non specificato, viene eseguita questa istruzione SQL: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Carattere speciale che indica che il documento è nidificato e come rendere flat il set di risultati.<br/><br/>Se ad esempio una query Azure Cosmos DB restituisce il risultato nidificato `"Name": {"First": "John"}`, l'attività di copia identifica il nome di colonna come `Name.First`, con il valore "John", se il valore **nestedSeparator** è un **.** (punto). |No <br />(il valore predefinito è il **.** (punto)) |

**Esempio**

```json
"activities":[
    {
        "name": "CopyFromCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Document DB input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-as-sink"></a>Azure Cosmos DB come sink

Per copiare dati in Azure Cosmos DB, impostare il tipo **sink** nell'attività di copia su **DocumentDbCollectionSink**. 

Nella sezione **source** dell'attività di copia sono supportate le proprietà seguenti:

| Proprietà | DESCRIZIONE | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** del sink dell'attività di copia deve essere impostata su **DocumentDbCollectionSink**. |Yes |
| writebehavior |Descrive come scrivere i dati in Azure Cosmos DB. Valori consentiti: **insert** e **upsert**.<br/><br/>Il comportamento di **upsert** consiste nella sostituzione del documento se esiste già un documento con lo stesso ID; in caso contrario, il documento viene inserito.<br /><br />**Nota**: Data Factory genera automaticamente un ID per un documento se non è specificato nel documento originale o tramite il mapping di colonna. Questo vuol dire che è necessario assicurarsi che il documento abbia un ID in modo che **upsert** funzioni come previsto. |No <br />(il valore predefinito è **insert**) |
| writeBatchSize | Data Factory usa la [libreria dell'executor bulk di Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) per scrivere dati in Azure Cosmos DB. La proprietà **writeBatchSize** controlla le dimensioni dei documenti che vengono forniti nella libreria. È possibile provare ad aumentare il valore di **writeBatchSize** per migliorare le prestazioni. |No <br />(il valore predefinito è **10.000**) |
| nestingSeparator |Carattere speciale nel nome della colonna **source** che indica che è necessario un documento nidificato. <br/><br/>Ad esempio, `Name.First` nella struttura del set di dati di output genera la struttura JSON seguente nel documento Azure Cosmos DB quando **nestedSeparator** è un **.** (punto): `"Name": {"First": "[value maps to this column from source]"}`  |No <br />(il valore predefinito è il **.** (punto)) |

**Esempio**

```json
"activities":[
    {
        "name": "CopyToCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="import-or-export-json-documents"></a>Importare o esportare documenti JSON

È possibile usare questo connettore Azure Cosmos DB per eseguire facilmente le operazioni seguenti:

* Importare in Azure Cosmos DB documenti JSON da varie origini, tra cui Archiviazione BLOB di Azure, Azure Data Lake Store e altri archivi basati su file supportati da Azure Data Factory.
* Esportare documenti JSON da una raccolta Azure Cosmos DB in diversi archivi basati su file.
* Copiare documenti tra due raccolte Azure Cosmos DB così come sono.

Per ottenere la copia senza schema:

* Quando si usa lo strumento Copia dati, selezionare l'opzione **Export as-is to JSON files or Cosmos DB collection** (Esportare così come sono nel file JSON o in una raccolta di Cosmos DB).
* Quando si usa la creazione di attività, non specificare la sezione **structure** (chiamata anche *schema*) nel set di dati di Azure Cosmos DB. Non specificare neanche la proprietà **nestingSeparator** nell'origine o nel sink di Azure Cosmos DB nell'attività di copia. Quando si importano o esportano dati da o verso file JSON, nel set di dati dell'archivio file corrispondente specificare il tipo **format** come **JsonFormat** e configurare **filePattern** come descritto nella sezione [Formato JSON](supported-file-formats-and-compression-codecs.md#json-format). Quindi, non specificare la sezione **structure** e ignorare le altre impostazioni del formato.

## <a name="next-steps"></a>Passaggi successivi

Per un elenco degli archivi dati supportati come origini e sink dall'attività di copia in Azure Data Factory, vedere gli [archivi dati supportati](copy-activity-overview.md##supported-data-stores-and-formats).
