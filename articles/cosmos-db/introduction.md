---
title: Introduzione ad Azure Cosmos DB | Microsoft Docs
description: Informazioni su Azure Cosmos DB. Questo database multimodello distribuito a livello globale è pensato per garantire bassa latenza, scalabilità elastica, disponibilità elevata e offre il supporto nativo per i dati NoSQL.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: overview
ms.date: 04/08/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 717a0234f0155229597c6fc4fe70a7868a536975
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160734"
---
# <a name="welcome-to-azure-cosmos-db"></a>Introduzione ad Azure Cosmos DB

Azure Cosmos DB è il database multimodello distribuito a livello globale di Microsoft. Con un semplice clic su un pulsante, Azure Cosmos DB garantisce la scalabilità elastica e indipendente della velocità effettiva e dello spazio di archiviazione tra un numero qualsiasi di aree geografiche di Azure. Assicura inoltre velocità effettiva, latenza, disponibilità e coerenza grazie a [contratti di servizio](https://aka.ms/acdbsla) (SLA, Service Level Agreement) completi, una garanzia che nessun altro servizio di database è in grado di offrire. È possibile [provare Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) senza una sottoscrizione di Azure, gratuitamente e senza impegno.

> [!div class="nextstepaction"]
> [Prova gratuitamente Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB è il servizio di database Microsoft distribuito a livello globale con scalabilità orizzontale elastica, bassa latenza garantita, cinque modelli di coerenza e contratti di servizio completi garantiti.](./media/introduction/azure-cosmos-db.png)

## <a name="key-capabilities"></a>Funzionalità principali
Come servizio di database multimodello distribuito a livello globale, Azure Cosmos DB consente di creare facilmente applicazioni scalabili e altamente reattive su scala globale:

* **Distribuzione globale chiavi in mano**
    * È possibile [distribuire i dati](distribute-data-globally.md) a un numero qualsiasi di [aree di Azure](https://azure.microsoft.com/regions/) semplicemente [facendo clic su un pulsante](tutorial-global-distribution-sql-api.md). Ciò consente di portare i dati nelle località in cui si trovano gli utenti, garantendo loro la latenza più bassa possibile. 
    * Con l'API multihoming di Azure Cosmos DB, l'app sa sempre dove si trova l'area più vicina e invia le richieste al data center più vicino. Tutto questo è possibile senza modificare la configurazione. Si imposta l'area di scrittura e il numero di aree di lettura desiderato. Le altre operazioni vengono gestite automaticamente.
    * Quando si aggiungono e rimuovono aree nel database Azure Cosmos DB, non è necessario ridistribuire l'applicazione che continua a essere a disponibilità elevata grazie alla funzionalità delle API multihosting.

* **Più modelli di dati e API comuni per l'accesso e le query sui dati**
    * Il modello di dati basato sulla sequenza di record ATOM (ARS) su cui si fonda Azure Cosmos DB supporta in modo nativo più modelli di dati, tra cui modelli relativi a documenti, grafi, coppie chiave-valore, tabelle e colonne.
    * Le API per i modelli di dati seguenti sono supportate con SDK disponibili in più lingue:
        * [API SQL](sql-api-introduction.md): un motore di database JSON senza schema con funzionalità di esecuzione di query SQL avanzate.
        * [API MongoDB](mongodb-introduction.md): *MongoDB come servizio* a scalabilità elevata con la tecnologia della piattaforma Azure Cosmos DB. Compatibile con librerie, driver, strumenti e applicazioni MongoDB esistenti.
        * [API Cassandra](cassandra-introduction.md): Cassandra come servizio con distribuzione a livello globale con la tecnologia della piattaforma Azure Cosmos DB. Compatibile con librerie, driver, strumenti e applicazioni [Apache Cassandra](https://cassandra.apache.org/) esistenti.
        * [API Gremlin](graph-introduction.md): servizio di database a grafo completamente gestito e scalabile orizzontalmente che semplifica la compilazione e l'esecuzione delle applicazioni che usano set di dati con connessione elevata che supportano le API Open Gremlin (in base alla [specifica Apache TinkerPop](http://tinkerpop.apache.org/), Apache Gremlin).
        * [API Tabelle](table-introduction.md): servizio di database di coppie chiave-valore progettato per offrire funzionalità Premium (ad esempio, indicizzazione automatica, bassa latenza garantita, distribuzione globale) alle applicazioni di archiviazione tabelle di Azure senza apportare modifiche alle app.
        * Altri modelli di dati e API saranno presto disponibili.

* **Ridimensionamento elastico e indipendente della velocità effettiva e dell'archiviazione su richiesta e in tutto il mondo**
    * Possibilità di ridimensionare facilmente la velocità effettiva del database con una granularità [al secondo](request-units.md), modificandola in qualsiasi momento. 
    * Scalabilità dell'archiviazione [in modo trasparente e automatico](partition-data.md) per soddisfare i requisiti di dimensione presenti e futuri.

* **Compilazione di applicazioni cruciali altamente reattive**
    * Azure Cosmos DB garantisce ai clienti una bassa latenza end-to-end al 99° percentile. 
    * Per un elemento tipico da 1 kB, Cosmos DB assicura una latenza end-to-end inferiore a 10 ms per le letture e a 10 ms per le scritture al 99° percentile, all'interno della stessa area di Azure. Le latenze medie sono significativamente inferiori (meno di 5 ms).

* **Disponibilità Always On**
    * Contratto di servizio con disponibilità del 99,99% per tutti gli account di database in una singola area e disponibilità in lettura del 99,999% per tutti gli account di database in più aree.
    * Distribuzione in un numero qualsiasi di [aree di Azure](https://azure.microsoft.com/regions) per una maggiore disponibilità e migliori prestazioni.
    * Impostazione dinamica delle priorità per le aree e [simulazione di un errore](high-availability.md) di una o più aree con garanzie di perdita dei dati pari a zero per testare la disponibilità end-to-end per l'intera app (oltre che per il solo database). 

* **Scrittura di applicazioni distribuite a livello globale in modo immediato**
    * Cinque [modelli di coerenza](consistency-levels.md) ben definiti, pratici e intuitivi offrono un'ampia gamma di livelli di coerenza, da quella assoluta di tipo SQL alla coerenza minima finale di tipo NoSQL, compresi i livelli intermedi. 
  
* **Recupero dell'investimento**
    * [Contratti di servizio](https://aka.ms/acdbsla) completi leader del settore con copertura finanziaria per disponibilità, latenza, velocità effettiva e coerenza per i dati di importanza critica. 

* **Nessuna gestione di schemi/indici di database**
    * Iterazione rapida dello schema dell'applicazione senza preoccuparsi dello schema del database e/o della gestione degli indici.
    * Il motore di database di Azure Cosmos DB è completamente indipendente dallo schema: questo significa che indicizza automaticamente tutti i dati inseriti senza richiedere schemi o indici e consente l'esecuzione di query ultrarapide. 

* **Costo di proprietà ridotto**
    * Da cinque a dieci volte più conveniente rispetto a una soluzione non gestita o a una soluzione NoSQL locale.
    * Tre volte più economico di AWS DynamoDB o di Google Spanner.

## <a name="capability-comparison"></a>Confronto delle funzionalità

Azure Cosmos DB fornisce le migliori funzionalità dei database tradizionali relazionali e non relazionali.

| Capabilities | Database relazionali   | Database non relazionali (NoSQL) |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Distribuzione globale | No  | No  | Sì, distribuzione chiavi in mano in oltre 30 aree con le API multihosting|
| Scalabilità orizzontale | No  | Yes | Sì, archiviazione e velocità effettiva sono scalabili in modo indipendente | 
| Garanzie di latenza | No  | Yes | Sì, 99% delle letture in <10 ms e delle scritture in <10 ms | 
| Disponibilità elevata | No  | Yes | Sì, Azure Cosmos DB è sempre online, ha compromessi ben definiti secondo il teorema PACELC e offre opzioni di failover automatico e manuale|
| Modello di dati + API | Relazionale + SQL | Multimodello + API OSS | Multimodello + SQL + API OSS |
| Contratti di servizio | Yes | No  | Sì, contratti di servizio completi per latenza, velocità effettiva, coerenza, disponibilità |

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Soluzioni che traggono vantaggio da Azure Cosmos DB

Qualsiasi [applicazione Web, per dispositivi mobili, di gioco e IoT](use-cases.md) che debba gestire un numero molto elevato di dati, letture e scritture su scala [globale](distribute-data-globally.md) con tempi di risposta quasi reali per una varietà di dati può trarre beneficio da disponibilità elevata, velocità effettiva elevata, bassa latenza e coerenza ottimizzabile [garantite](https://azure.microsoft.com/support/legal/sla/cosmos-db/) di Azure Cosmos DB. Informazioni su come applicare Azure Cosmos DB a [IoT e dati telematici](use-cases.md#iot-and-telematics), [vendite e marketing](use-cases.md#retail-and-marketing), [giochi](use-cases.md#gaming) e [applicazioni Web e per dispositivi mobili](use-cases.md#web-and-mobile-applications).

## <a name="next-steps"></a>Passaggi successivi
Per un'introduzione ad Azure Cosmos DB, fare riferimento alle guide introduttive seguenti:

* [Introduzione all'API SQL di Azure Cosmos DB](create-sql-api-dotnet.md)
* [Introduzione all'API MongoDB di Azure Cosmos DB](create-mongodb-nodejs.md)
* [Introduzione all'API Cassandra di Azure Cosmos DB](create-cassandra-dotnet.md)
* [Introduzione all'API Gremlin di Azure Cosmos DB](create-graph-dotnet.md)
* [Introduzione all'API Tabelle di Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Prova gratuitamente Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/)
