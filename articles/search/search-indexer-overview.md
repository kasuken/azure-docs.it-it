---
title: Indicizzatori in Ricerca di Azure | Documentazione Microsoft
description: Effettuare una ricerca per indicizzazione in un database SQL, in Azure Cosmos DB o in Archiviazione di Azure per estrarre dati ricercabili e popolare un indice di Ricerca di Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: heidist
ms.openlocfilehash: 2164e0b7cc973969e39f5708bb6509c1ed5f636a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/02/2018
ms.locfileid: "34641136"
---
# <a name="indexers-in-azure-search"></a>Indicizzatori in Ricerca di Azure

Un *indicizzatore* in Ricerca di Azure è un crawler che estrae dati e metadati ricercabili da un'origine dati Azure esterna e popola un indice in base a mapping dei campi tra l'indice e l'origine dati dell'utente. Questo approccio viene talvolta definito modello pull perché il servizio esegue il pull dei dati senza la necessità di scrivere codice che esegua il push dei dati in un indice.

Gli indicizzatori sono basati su tipi di origini dati o piattaforme, con singoli indicizzatori per SQL Server in Azure, Cosmos DB, Archiviazione tabelle di Azure, Archiviazione BLOB e così via.

È possibile usare un indicizzatore come unico mezzo per l'inserimento di dati o una combinazione di tecniche, tra cui l'uso di un indicizzatore per caricare solo alcuni dei campi nell'indice.

È possibile eseguire gli indicizzatori su richiesta o in base a una pianificazione di aggiornamento dati ricorrente che viene eseguita ogni quindici minuti. Aggiornamenti più frequenti richiedono un modello push che aggiorna contemporaneamente i dati sia in Ricerca di Azure che nell'origine dati esterna.

## <a name="approaches-for-creating-and-managing-indexers"></a>Approcci per la creazione e la gestione degli indicizzatori

È possibile creare e gestire gli indicizzatori nei modi seguenti:

* [Portale > Importazione guidata dati](search-import-data-portal.md)
* [API REST del servizio](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Un nuovo indicizzatore viene inizialmente annunciato come funzionalità di anteprima. Le funzionalità di anteprima vengono introdotte nelle API (REST e .NET) e quindi integrate nel portale dopo il passaggio alla disponibilità generale. Se si sta valutando un nuovo indicizzatore, è consigliabile prevedere la scrittura di codice.


<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Origini dati supportate

Gli indicizzatori eseguono ricerche per indicizzazione negli archivi dati in Azure.

* [SQL di Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Archivio BLOB di Azure](search-howto-indexing-azure-blob-storage.md)
* [Archiviazione tabelle di Azure](search-howto-indexing-azure-tables.md)


## <a name="basic-configuration-steps"></a>Procedura di configurazione di base
Gli indicizzatori possono offrire funzionalità univoche per l'origine dati. In questo senso, alcuni aspetti della configurazione dell'indicizzatore o dell'origine dati possono variare a seconda del tipo di indicizzatore. Tutti gli indicizzatori, tuttavia, condividono la stessa composizione e gli stessi requisiti di base. Le procedure comuni a tutti gli indicizzatori sono descritte sotto.

### <a name="step-1-create-a-data-source"></a>Passaggio 1: Creare un'origine dati
Un indicizzatore effettua il pull dei dati da un'*origine dati* che contiene informazioni, ad esempio una stringa di connessione ed eventualmente credenziali. Chiamare l'API REST [Create Datasource](https://docs.microsoft.com/rest/api/searchservice/create-data-source) o la [classe DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource) per creare la risorsa.

Le origini dati vengono configurate e gestite indipendentemente dagli indicizzatori che le usano. Ciò significa che un'origine dati può essere usata da più indicizzatori per caricare più di un indice alla volta.

### <a name="step-2-create-an-index"></a>Passaggio 2: Creare un indice
Un indicizzatore consente di automatizzare alcune attività relative all'inserimento dei dati, ma la creazione di un indice in genere non fa parte di esse. Uno dei prerequisiti prevede che sia disponibile un indice predefinito con campi corrispondenti a quelli dell'origine dati esterna. Per altre informazioni su come strutturare un indice, vedere [Creare l'indice (API REST di Ricerca di Azure)](https://docs.microsoft.com/rest/api/searchservice/Create-Index) o [classe Index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index). Per una guida alle associazioni dei campi, vedere [Mapping dei campi negli indicizzatori di Ricerca di Azure](search-indexer-field-mappings.md).

> [!Tip]
> Anche se gli indicizzatori non possono generare automaticamente un indice, la procedura guidata **Importa dati** nel portale può risultare utile. Nella maggior parte dei casi, la procedura guidata può dedurre uno schema di indice dai metadati esistenti nell'origine, presentando uno schema dell'indice preliminare che è possibile modificare inline mentre la procedura guidata è attiva. Dopo la creazione dell'indice nel servizio, le ulteriori modifiche nel portale sono per lo più limitate all'aggiunta di nuovi campi. Prendere in considerazione la procedura guidata per la creazione, ma non per la revisione di un indice. Per un'esperienza di apprendimento pratico, seguire le indicazioni della [procedura dettagliata per il portale](search-get-started-portal.md).

### <a name="step-3-create-and-schedule-the-indexer"></a>Passaggio 3: Creare e pianificare l'indicizzatore
La definizione dell'indicizzatore è un costrutto che specifica l'indice, l'origine dati e una pianificazione. Un indicizzatore può fare riferimento a un'origine dati di un altro servizio, purché tale origine dati appartenga alla stessa sottoscrizione. Per altre informazioni su come strutturare un indicizzatore, vedere [Creare un indicizzatore (API REST di Ricerca di Azure)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

## <a name="next-steps"></a>Passaggi successivi
Dopo aver acquisito queste informazioni di base, il passaggio successivo prevede l'analisi dei requisiti e delle attività specifici per ogni tipo di origine dati.

* [Database SQL di Azure o SQL Server in una macchina virtuale di Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Archivio BLOB di Azure](search-howto-indexing-azure-blob-storage.md)
* [Archiviazione tabelle di Azure](search-howto-indexing-azure-tables.md)
* [Indicizzazione di BLOB CSV con l'indicizzatore di BLOB di Ricerca di Azure](search-howto-index-csv-blobs.md)
* [Indicizzazione di BLOB JSON con l'indicizzatore di BLOB di Ricerca di Azure](search-howto-index-json-blobs.md)
