---
title: Unità richiesta e velocità effettiva in Azure Cosmos DB
description: Informazioni su come specificare e stimare i requisiti relativi alle unità richiesta in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: rimman
ms.openlocfilehash: 709cd16c7eee30dd2d88ea87b5f704ad20530ffb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621344"
---
# <a name="request-units-in-azure-cosmos-db"></a>Unità richiesta in Azure Cosmos DB

Con Azure Cosmos DB, viene addebitato un costo per la velocità effettiva sottoposta a provisioning e per le risorse di archiviazione utilizzate su base oraria. Il provisioning della velocità effettiva è necessario per assicurarsi che siano costantemente disponibili risorse di sistema sufficienti per il database Cosmos in modo da soddisfare o superare le condizioni del contratto di servizio per Cosmos DB.

Cosmos DB supporta un'ampia gamma di API (SQL, MongoDB, Cassandra, Gremlin e Tabella). Ogni API ha uno specifico set di operazioni di database, da semplici operazioni di lettura e scrittura puntuali a query complesse. Ogni operazione di database utilizza le risorse di sistema a seconda della complessità.  Il costo di tutte le operazioni di database viene normalizzato da Cosmos DB ed è espresso in termini di unità richiesta (UR). Il costo per leggere un elemento da 1 KB è pari a 1 unità richiesta (1 UR). In modo analogo, a tutte le altre operazioni di database viene assegnato un costo in termini di UR. Indipendentemente dall'API usata per interagire con il contenitore Cosmos e dall'operazione di database (scrittura, lettura, query), i costi sono sempre misurati in termini di UR.

L'unità di misura UR/sec può essere considerata come la valuta per la velocità effettiva. UR/sec è una valuta basata sulla frequenza, che è determinata in base all'astrazione delle risorse di sistema, ad esempio CPU, operazioni di I/O al secondo e memoria, necessarie per eseguire le operazioni di database supportate da Cosmos DB. L'immagine seguente mostra le unità richiesta utilizzate dalle diverse operazioni di database:

![Utilizzo delle unità di richiesta da parte delle operazioni di database](./media/request-units/request-units.png)

Per gestire e pianificare la capacità, Cosmos DB garantisce che il numero di UR per una specifica operazione di database su un determinato set di dati sia deterministico. È possibile tenere traccia del numero di UR utilizzate da qualsiasi operazione di database esaminando l'intestazione della risposta. Una volta identificati i fattori che influiscono sugli addebiti delle unità richiesta e i requisiti di velocità effettiva dell'applicazione, è possibile eseguire l'applicazione in modo economicamente conveniente.

Anche se la fatturazione avviene su base oraria, il provisioning del numero di UR per l'applicazione viene effettuato in base a incrementi di 100 UR/sec. Per ridimensionare la velocità effettiva di cui viene effettuato il provisioning per l'applicazione, è possibile aumentare o diminuire il numero di UR (in base a incrementi o decrementi di 100 UR) in qualsiasi momento, a livello di codice o tramite il portale di Azure.

È possibile effettuare il provisioning della velocità effettiva a due diversi livelli di granularità: 

* **Contenitori**. Per altre informazioni, vedere l'articolo su [come effettuare il provisioning della velocità effettiva in un contenitore Cosmos](how-to-provision-container-throughput.md).
* **Database**. Per altre informazioni, vedere l'articolo su [come effettuare il provisioning della velocità effettiva in un database Cosmos](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>Considerazioni sulle unità richiesta

Durante la stima del numero di UR/sec di cui effettuare il provisioning, è importante prendere in considerazione i fattori seguenti:

* **Dimensioni degli elementi**: con l'aumentare della dimensione di un elemento, aumenta anche il numero di UR utilizzate per leggere o scrivere l'elemento.

* **Indicizzazione dei documenti**: per impostazione predefinita, ogni elemento viene indicizzato automaticamente. Se si sceglie di non indicizzare alcuni elementi in un contenitore, viene utilizzato un numero inferiore di unità richiesta.

* **Numero di proprietà degli elementi**: supponendo che sia applicata l'indicizzazione predefinita a tutte le proprietà, il numero di UR utilizzate per scrivere un elemento aumenta proporzionalmente al numero delle proprietà dell'elemento.

* **Proprietà indicizzate**: i criteri di indicizzazione in ogni contenitore determinano le proprietà che vengono indicizzate per impostazione predefinita. È possibile ridurre l'utilizzo di unità richiesta per operazioni di scrittura limitando il numero di proprietà indicizzate.

* **Coerenza dei dati**: durante l'esecuzione di operazioni di lettura, i livelli di coerenza assoluta e con decadimento ristretto utilizzano un numero di UR di due volte maggiore rispetto ad altri livelli di coerenza meno rigidi.

* **Modelli di query**: la complessità di una query influisce sulla quantità di unità richiesta utilizzate per un'operazione. Il numero di risultati di query, il numero di predicati, la natura dei predicati, il numero di funzioni definite dall'utente, la dimensione dei dati di origine, la dimensione del set di risultati e le proiezioni sono tutti fattori che incidono sul costo delle operazioni di query. Cosmos DB assicura che la stessa query sugli stessi dati costi sempre lo stesso numero di UR su esecuzioni ripetute.

* **Utilizzo di script**: come le query, le stored procedure e i trigger utilizzano le UR in base alla complessità delle operazioni da eseguire. Quando si sviluppa l'applicazione, controllare l'intestazione per l'addebito delle richieste per comprendere meglio quanta capacità di unità richiesta viene utilizzata da ogni operazione.

## <a name="next-steps"></a>Passaggi successivi

* Altre informazioni su come [effettuare il provisioning della velocità effettiva per contenitori e database Cosmos DB](set-throughput.md)
* Altre informazioni sulle [partizioni logiche](partition-data.md)
* Altre informazioni sul [ridimensionamento a livello globale della velocità effettiva sottoposta a provisioning](scaling-throughput.md)
* Informazioni su [come effettuare il provisioning della velocità effettiva in un contenitore Cosmos](how-to-provision-container-throughput.md)
* Informazioni su [come effettuare il provisioning della velocità effettiva in un database Cosmos](how-to-provision-database-throughput.md)
