---
title: Archiviazione con ridondanza geografica per la replica tra più aree in Archiviazione di Azure | Microsoft Docs
description: Con l'archiviazione con ridondanza geografica i dati vengono replicati tra due aree a centinaia di chilometri di distanza. L'archiviazione con ridondanza geografica protegge dagli errori hardware nel data center e dalle situazioni di emergenza a livello di area.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 85d69db2f94e4bddf1258233c34c64dcf78a3eeb
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219223"
---
# <a name="geo-redundant-storage-grs-cross-regional-replication-for-azure-storage"></a>Archiviazione con ridondanza geografica: replica tra più aree per Archiviazione di Azure
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-grs.md)]

## <a name="read-access-geo-redundant-storage"></a>Archiviazione con ridondanza geografica e accesso in lettura
L'archiviazione con ridondanza geografica e accesso in lettura (RA-GRS) ottimizza la disponibilità per l'account di archiviazione. L'archiviazione con ridondanza geografica e accesso in lettura fornisce l'accesso in sola lettura ai dati nella località secondaria, oltre alla replica geografica tra due aree.

Se si abilita l'accesso in sola lettura ai dati nell'area secondaria, i dati saranno disponibili in un endpoint secondario, oltre che nell'endpoint primario dell'account di archiviazione. L'endpoint secondario è simile a quello primario, ma al nome dell'account viene aggiunto il suffisso `–secondary` . Se ad esempio l'endpoint primario per il servizio BLOB è `myaccount.blob.core.windows.net`, l'endpoint secondario sarà `myaccount-secondary.blob.core.windows.net`. Le chiavi di accesso per l'account di archiviazione sono identiche per gli endpoint primario e secondario.

Alcune considerazioni da ricordare quando si usa l'archiviazione con ridondanza geografica e accesso in lettura:

* L'applicazione deve gestire l'endpoint con cui interagire quando usa l'archiviazione con ridondanza geografica e accesso in lettura.
* Poiché la replica asincrona implica un ritardo, è possibile che le modifiche non ancora replicate nell'area secondaria vadano perse se non è possibile recuperare i dati dall'area primaria.
* È possibile controllare l'ora dell'ultima sincronizzazione dell'account di archiviazione. L'ora dell'ultima sincronizzazione è un valore di data/ora GMT. Tutte le scritture nell'area primaria precedenti all'ora dell'ultima sincronizzazione sono state scritte correttamente nella località secondaria, vale a dire che sono disponibili per la lettura dalla località secondaria. Le scritture nell'area primaria successive all'ora dell'ultima sincronizzazione possono essere o meno già disponibili per la lettura. È possibile eseguire query su questo valore usando il [portale di Azure](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md) o una delle librerie client di Archiviazione di Azure.
* Se Microsoft avvia un failover nell'area secondaria, al termine del failover si avrà accesso in lettura e scrittura a tali dati. Per altre informazioni, vedere [Indicazioni sul ripristino di emergenza](storage-disaster-recovery-guidance.md).
* Per informazioni su come passare all'area secondaria, vedere [Cosa fare se si verifica un'interruzione di Archiviazione di Azure](storage-disaster-recovery-guidance.md).
* L'archiviazione con ridondanza geografica e accesso in lettura è pensata per rispondere a requisiti di disponibilità elevata. Per indicazioni sulla scalabilità, vedere l'[elenco di controllo delle prestazioni](storage-performance-checklist.md).
* Per suggerimenti su come progettare la disponibilità elevata con l'archiviazione con ridondanza geografica e accesso in lettura, vedere [Progettazione di applicazioni a disponibilità elevata con RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="what-is-the-rpo-and-rto-with-grs"></a>Quali sono gli obiettivi RPO e RTO con l'archiviazione con ridondanza geografica?
**Obiettivo del punto di ripristino (RPO)**: nel caso dell'archiviazione con ridondanza geografica e dell'archiviazione con ridondanza geografica e accesso in lettura, il servizio di archiviazione esegue la replica geografica asincrona dalla località primaria a quella secondaria. Nell'eventualità di una grave emergenza a livello locale nell'area primaria, Microsoft esegue un failover all'area secondaria. Se viene eseguito un failover, le modifiche recenti non ancora replicate geograficamente potrebbero andare perse. Il numero di minuti di potenziale perdita di dati è detto obiettivo del punto di ripristino e indica il punto nel tempo in cui è possibile recuperare i dati. L'obiettivo del punto di ripristino di Archiviazione di Azure è in genere inferiore a 15 minuti, anche se attualmente non è previsto alcun contratto di servizio sulla durata della replica geografica.

**Obiettivo del tempo di ripristino:** l'obiettivo del tempo di ripristino è una misura del tempo necessario per eseguire il failover e riportare online l'account di archiviazione. Il tempo necessario per eseguire il failover include le azioni seguenti:

   * Tempo necessario a Microsoft per determinare se i dati possono essere recuperati nella località primaria o se è necessario un failover.
   * Tempo per eseguire il failover dell'account di archiviazione mediante modifica delle voci relative al DNS primario in modo che puntino alla località secondaria.

Microsoft prende seriamente la responsabilità di salvaguardare i dati degli utenti. Se esiste la possibilità di recuperare i dati nell'area primaria, Microsoft ritarderà il failover e si concentrerà sul recupero dei dati. 

## <a name="paired-regions"></a>Aree abbinate 
L'area primaria viene selezionata durante la creazione di un account di archiviazione. L'area secondaria associata viene determinata in base all'area primaria e non è possibile modificarla. Per informazioni aggiornate sulle aree supportate da Azure, vedere [Continuità aziendale e ripristino di emergenza nelle aree geografiche abbinate di Azure](../../best-practices-availability-paired-regions.md).

## <a name="see-also"></a>Vedere anche 
- [Replica di Archiviazione di Azure](storage-redundancy.md)
- [Archiviazione con ridondanza locale: ridondanza dei dati a basso costo per Archiviazione di Azure](storage-redundancy-lrs.md)
- [Archiviazione con ridondanza della zona (ZRS): applicazioni di Archiviazione di Azure a disponibilità elevata](storage-redundancy-zrs.md)