---
title: Partizionamento e scalabilità orizzontale in Azure Cosmos DB
description: Informazioni sul funzionamento del partizionamento in Azure Cosmos DB, sulla configurazione del partizionamento e delle chiavi di partizione e su come scegliere la chiave di partizione corretta per l'applicazione.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: 5dd1926496351f5bbfe8e5b3e4d1e0b68e82d272
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283394"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Partizionamento e scalabilità orizzontale in Azure Cosmos DB

Questo articolo illustra le partizioni fisiche e logiche in Azure Cosmos DB e le procedure consigliate per la scalabilità e il partizionamento. 

## <a name="logical-partitions"></a>Partizioni logiche

Una partizione logica è costituita da un set di elementi con la stessa chiave di partizione. In un contenitore in cui in tutti gli elementi è presente una proprietà `City`, è possibile ad esempio usare `City` come chiave di partizione per il contenitore. Gruppi di elementi con valori specifici per `City`, ad esempio "Londra", "Parigi", "New York" e così via, formano una partizione logica distinta.

In Azure Cosmos DB un contenitore è l'unità fondamentale di scalabilità. I dati aggiunti al contenitore e la velocità effettiva di cui si effettua il provisioning nel contenitore vengono automaticamente partizionati (orizzontalmente) in un set di partizioni logiche. L'operazione viene eseguita in base alla chiave di partizione specificata per il contenitore Cosmos. Per altre informazioni, vedere l'articolo [Creare un contenitore in Azure Cosmos DB](how-to-create-container.md).

Una partizione logica definisce l'ambito delle transazioni di database. È possibile aggiornare gli elementi all'interno di una partizione logica tramite una transazione di isolamento dello snapshot.

Quando al contenitore vengono aggiunti nuovi elementi o se la velocità effettiva di cui è stato effettuato il provisioning nel contenitore viene aumentata, nuove partizioni logiche vengono create in modo trasparente dal sistema.

## <a name="physical-partitions"></a>Partizioni fisiche

Un contenitore Cosmos viene ridimensionato distribuendo i dati e la velocità effettiva tra un numero elevato di partizioni logiche. Una o più partizioni logiche vengono mappate internamente a una **partizione di risorse** costituite da un set di repliche. Ogni set di repliche ospita un'istanza del motore di database Cosmos. Un set di repliche rende i dati archiviati nella partizione di risorse durevoli, altamente disponibile e coerenti. Una partizione di risorse supporta una quantità fissa di spazio di archiviazione e di unità riservate con un limite massimo. Ogni replica che include la partizione di risorse eredita la quota di archiviazione. Tutte repliche di una partizione di risorse supportano complessivamente la velocità effettiva allocata alla partizione stessa. La figura seguente mostra come le partizioni logiche vengono mappate alle partizioni fisiche distribuite a livello globale:

![Partizionamento di Azure Cosmos DB](./media/partition-data/logical-partitions.png)

La velocità effettiva di provisioning per un contenitore viene suddivisa equamente tra le partizioni fisiche. Una progettazione di chiave di partizione che non preveda la distribuzione uniforme delle richieste di velocità effettiva può creare partizioni ad accesso frequente. Tali partizioni possono causare un uso inefficiente e con limitazioni della velocità effettiva di cui viene effettuato il provisioning.

A differenza delle partizioni logiche, le partizioni fisiche sono un'implementazione interna del sistema. Non è possibile controllarne le dimensioni, la posizione o il numero, né controllare il mapping tra le partizioni logiche e quelle fisiche. È possibile tuttavia controllare il numero di partizioni logiche, la distribuzione dei dati e la velocità effettiva scegliendo la chiave di partizione corretta.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo sono state presentate una panoramica del partizionamento dei dati e procedure consigliate per la scalabilità e il partizionamento in Azure Cosmos DB. 

* Informazioni sulla [velocità effettiva con provisioning in Azure Cosmos DB](request-units.md)
* Informazioni sulla [distribuzione globale in Azure Cosmos DB](distribute-data-globally.md)
* Informazioni sulla [scelta di una chiave di partizione](partitioning-overview.md#choose-partitionkey)
* Informazioni su [come effettuare il provisioning della velocità effettiva in un contenitore Cosmos](how-to-provision-container-throughput.md)
* Informazioni su [come effettuare il provisioning della velocità effettiva in un database Cosmos](how-to-provision-database-throughput.md)
