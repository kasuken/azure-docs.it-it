---
title: Panoramica di Azure Data Lake Storage Gen1 | Microsoft Docs
description: Comprendere che cos'è Azure Data Lake Storage Gen1 (precedentemente conosciuto come Azure Data Lake Store) e il valore che fornisce per altri archivi di dati
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 4dff8f4ff9fc324d48391c0399677b64824493c6
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37034886"
---
# <a name="overview-of-azure-data-lake-storage-gen1"></a>Panoramica di Azure Data Lake Storage Gen1

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Store è un repository su vasta scala a livello aziendale per carichi di lavoro di analisi di Big Data. Azure Data Lake consente di acquisire dati di qualsiasi dimensione, tipo e velocità di inserimento in un'unica posizione per le analisi esplorative e operative.

> [!TIP]
> Utilizzare il [percorso di apprendimento di archivio Data Lake](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) per iniziare ad esplorare il servizio di Archivio Data Lake di Azure.
> 
> 

Si può accedere all’Archivio Data Lake di Azure da Hadoop (disponibile con i cluster HDInsight) mediante le API REST WebHDFS compatibili. È progettato specificamente per consentire l'analisi su dati archiviati ed è ottimizzato per eseguire prestazioni per scenari di analisi dei dati. Per impostazione predefinita, include tutte le funzionalità a livello aziendale, protezione, gestibilità, scalabilità, affidabilità e disponibilità, essenziali per i casi di utilizzo aziendale reali.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Di seguito sono riportate alcune delle principali funzionalità di Azure Data Lake.

### <a name="built-for-hadoop"></a>Creato per Hadoop
L'archivio Azure Data Lake è un file system Apache Hadoop compatibile con HDFS (Hadoop Distributed File System) e funziona con l'ecosistema Hadoop.  Le applicazioni HDInsight esistenti o i servizi che utilizzano l'API WebHDFS possono integrarsi facilmente con l'Archivio Data Lake. L’Archivio Data Lake presenta anche un'interfaccia REST compatibile con WebHDFS per le applicazioni

I dati archiviati nell'Archivio Data Lake possono essere analizzati facilmente mediante framework di analisi di Hadoop, come MapReduce o Hive. Si può eseguire il provisioning sui cluster di Microsoft Azure HDInsight e questi possono essere configurati per accedere direttamente ai dati archiviati nell'Archivio Data Lake.

### <a name="unlimited-storage-petabyte-files"></a>Archiviazione illimitata, file dei petabyte
L’Archivio Data Lake di Azure fornisce un'archiviazione illimitata ed è adatto per l'archiviazione di una serie di dati per l'analisi. Ciò non impone limiti per le dimensioni dell’account, le dimensioni dei file o la quantità di dati che possono essere archiviati in un Data Lake. I singoli file possono variare da kilobyte di petabyte rendendo la scelta ideale per memorizzare qualsiasi tipo di dati. I dati vengono archiviati in modo permanente creando più copie e non esiste alcun limite per la durata del tempo durante il quale i dati possono essere archiviati nel Data Lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Prestazioni ottimizzate per l'analisi di Big Data
L'archivio Data Lake di Azure è progettato per l'esecuzione di sistemi di analisi di scalabilità di grandi dimensioni che richiedono una velocità effettiva molto elevata per eseguire query e analisi su grandi quantità di dati. Il Data Lake propaga parti di un file su un numero di singoli server di archiviazione. Ciò migliora la velocità effettiva di lettura durante la lettura in parallelo del file per l'esecuzione dell’analisi dei dati.

### <a name="enterprise-ready-highly-available-and-secure"></a>A livello aziendale: con disponibilità elevata e sicuro
L’Archivio Data Lake di Azure offre affidabilità e disponibilità standard del settore. Gli asset di dati vengono archiviati in modo permanente creando copie ridondanti per salvaguardarsi da eventuali errori imprevisti. Le aziende possono utilizzare Azure Data Lake nelle loro soluzioni come una parte importante della piattaforma di dati esistente.

Archivio Data Lake fornisce anche la protezione a livello aziendale per i dati archiviati. Per altre informazioni, vedere [Protezione dei dati in Archivio Data Lake di Azure](#DataLakeStoreSecurity).

### <a name="all-data"></a>Tutti i dati
L’Archivio Data Lake di Azure può immagazzinare i dati nel loro formato originale, così come sono, senza alcuna trasformazione. Archivio Data Lake non richiede uno schema prima che i dati vengano caricati, lasciando al singolo framework di analisi l’interpretazione dei dati e la definizione di uno schema al momento dell'analisi. La possibilità di archiviare i file di dimensioni e formati arbitrari fa sì che Archivio Data Lake possa gestire dati strutturati, semi-strutturati e non strutturati.

I contenitori di Archivio Azure Data Lake per i dati sono essenzialmente cartelle e file. È possibile agire sui dati archiviati mediante SDK, portale di Azure e Azure PowerShell. Dopo aver inserito i dati nell'archivio usando queste interfacce e i contenitori appropriati, è possibile memorizzare qualsiasi tipo di dati. Archivio Data Lake non esegue una gestione particolare dei dati in base al tipo di dati archiviati.

## <a name="DataLakeStoreSecurity"></a>Protezione dei dati nell'archivio Data Lake di Azure
Azure Data Lake Store utilizza la Azure Active Directory per gli elenchi di autenticazione e di controllo di accesso (ACL) per gestire l'accesso ai dati.

| Funzionalità | DESCRIZIONE |
| --- | --- |
| Authentication |L’Archivio Data Lake di Azure si integra con la Azure Active Directory (AAD) per la gestione delle identità e degli accessi per tutti i dati memorizzati nell'archivio Data Lake di Azure. Come risultato dell'integrazione, ci sono i vantaggi di Azure Data Lake tratti da tutte le funzionalità AAD compresi l’autenticazione a più fattori, l'accesso condizionale, il controllo dell'accesso basato su ruoli, il monitoraggio dell'utilizzo dell'applicazione, sicurezza, il monitoraggio e l’avviso di sicurezza, e così via. L’Archivio Data Lake di Azure supporta il protocollo OAuth 2.0 per l'autenticazione con l'interfaccia REST. Vedere [Data Lake Store authentication](data-lakes-store-authentication-using-azure-active-directory.md) (Autenticazione con Data Lake Store)|
| Controllo di accesso |L'Archivio Data Lake di Azure fornisce il controllo di accesso mediante il supporto delle autorizzazioni di tipo POSIX esposte dal protocollo WebHDFS. Gli elenchi di controllo di accesso possono essere abilitati nella cartella radice, nelle sottocartelle e nei singoli file. Per altre informazioni sul funzionamento degli elenchi di controllo di accesso nel contesto di Data Lake Store, vedere [Controllo di accesso in Data Lake Store](data-lake-store-access-control.md). |
| Crittografia |Data Lake Store fornisce anche la crittografia per i dati archiviati nell'account. Le impostazioni della crittografia vengono specificate durante la creazione dell'account Data Lake Store. È possibile scegliere di crittografare i dati oppure di fare a meno della crittografia. Per altre informazioni, vedere [Crittografia in Data Lake Store](data-lake-store-encryption.md). Per istruzioni su come specificare la configurazione relativa alla crittografia, vedere [Introduzione ad Azure Data Lake Store con il portale di Azure](data-lake-store-get-started-portal.md). |

Per altre informazioni sulla protezione dei dati in Archivio Data Lake, fare clic sui collegamenti seguenti.

* Per istruzioni su come proteggere i dati in Archivio Data Lake, vedere [Protezione dei dati nell'archivio Data Lake di Azure](data-lake-store-secure-data.md).
* Se si preferiscono i video, [Guardare questo video](https://mix.office.com/watch/1q2mgzh9nn5lx) su come proteggere i dati archiviati in Archivio Data Lake.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Applicazioni compatibili con l'archivio Data Lake di Azure
Archivio Azure Data Lake è compatibile con la maggior parte dei componenti open source nell'ecosistema Hadoop. Si integra bene anche con altri servizi di Azure. Questo fa di Archivio Data Lake la soluzione ideale per le esigenze di archiviazione dei dati. Per altre informazioni su come usare Archivio Data Lake sia con componenti open source che con altri servizi di Azure, fare clic sui collegamenti seguenti.

* Vedere [Applicazioni e servizi compatibili con Archivio Azure Data Lake](data-lake-store-compatible-oss-other-applications.md) per un elenco di applicazioni open source interoperative con Archivio Data Lake.
* Vedere [Integrazione con altri servizi di Azure](data-lake-store-integrate-with-other-services.md) per comprendere come l’Archivio Data Lake possa essere utilizzato con altri servizi di Azure per abilitare una vasta gamma di scenari.
* Per informazioni su come usare Archivio Data Lake in scenari come l'inserimento, l'elaborazione, il download e la visualizzazione di dati, vedere [Scenari per l'uso di Archivio Data Lake](data-lake-store-data-scenarios.md) .

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Informazioni sul file system di Azure Data Lake Store (adl://)
È possibile accedere a Data Lake Store attraverso il nuovo file system, AzureDataLakeFilesystem (adl://), in ambienti Hadoop (disponibile con il cluster HDInsight). Le applicazioni e servizi che utilizzano adl:// sono in grado di sfruttare altre ottimizzazioni delle prestazioni che non sono attualmente disponibili in WebHDFS. Di conseguenza, l’Archivio Data Lake offre la flessibilità di ricorrere alle migliori prestazioni con l'opzione consigliata di utilizzo adl:// o di mantenere il codice esistente continuando a utilizzare l'API WebHDFS direttamente. Azure HDInsight utilizza AzureDataLakeFilesystem per fornire prestazioni ottimali su Archivio Data Lake.

È possibile accedere ai dati nell'Archivio Data Lake utilizzando `adl://<data_lake_store_name>.azuredatalakestore.net`. Per altre informazioni su come accedere ai dati nell'Archivio Data Lake vedere [Visualizzare le proprietà dei dati archiviati](data-lake-store-get-started-portal.md#properties)

## <a name="next-steps"></a>Passaggi successivi

* [Introduzione a Data Lake Store con il portale di Azure](data-lake-store-get-started-portal.md)
* [Introduzione a Azure Data Lake Store utilizzando .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Usare Azure HDInsight con Archivio Data Lake](data-lake-store-hdinsight-hadoop-use-portal.md)