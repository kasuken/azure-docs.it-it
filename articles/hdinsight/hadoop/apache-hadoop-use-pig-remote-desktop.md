---
title: Usare Apache Pig con Desktop remoto in HDInsight - Azure
description: Informazioni su come usare il comando Apache Pig per l'esecuzione di istruzioni Pig Latin da una connessione Desktop remoto a un cluster Apache Hadoop basato su Windows in HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 9ab3c5399263e301760b7822488fe00145149d2a
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633343"
---
# <a name="run-apache-pig-jobs-from-a-remote-desktop-connection"></a>Eseguire processi Apache Pig da una connessione Desktop remoto
[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Questo documento fornisce una procedura dettagliata dell'uso del comando Apache Pig per l'esecuzione di istruzioni Pig Latin da una connessione Desktop remoto a un cluster HDInsight basato su Windows. Pig Latin consente di creare applicazioni MapReduce descrivendo le trasformazioni di dati, anziché eseguendo il mapping e la riduzione delle funzioni.

> [!IMPORTANT]
> Desktop remoto è disponibile solo nei cluster HDInsight che usano Windows come sistema operativo. Linux è l'unico sistema operativo usato in HDInsight versione 3.4 o successiva. Per altre informazioni, vedere la sezione relativa al [ritiro di HDInsight in Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Per HDInsight 3.4 o versione successiva, vedere [Usare Pig con HDInsight e SSH](apache-hadoop-use-pig-ssh.md) per informazioni sull'esecuzione interattiva di processi Pig sul cluster dalla riga di comando.

## <a id="prereq"></a>Prerequisiti
Per seguire la procedura descritta in questo articolo, è necessario quanto segue:

* Un cluster HDInsight (Hadoop in HDInsight) basato su Windows
* Un computer client che esegue Windows 10, Windows 8 o Windows 7

## <a id="connect"></a>Connettersi con Desktop remoto
Abilitare Desktop remoto per il cluster HDInsight e quindi connettersi seguendo le istruzioni disponibili in [Connettersi a cluster HDInsight tramite RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Usare il comando Pig
1. Dopo avere stabilito una connessione Desktop remoto, avviare la **riga di comando di Hadoop** usando l'icona sul desktop.
2. Usare il codice seguente per avviare il comando Pig:

        %pig_home%\bin\pig

    Verrà visualizzato un prompt dei comandi `grunt>` .
3. Immettere la seguente istruzione:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Questo comando carica i contenuti del file sample.log nel file LOGS. È possibile visualizzare i contenuti del file usando il seguente comando:

        DUMP LOGS;
4. Trasformare i dati applicando un'espressione regolare per estrarre solo il livello di registrazione da ciascun record:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    È possibile usare **DUMP** per visualizzare i dati dopo la trasformazione. In questo caso, `DUMP LEVELS;`.
5. Continuare ad applicare le trasformazioni usando le seguenti istruzioni. Usare `DUMP` per visualizzare il risultato della trasformazione dopo ogni passaggio.

    <table>
    <tr>
    <th>Istruzione</th><th>Risultato</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;</td><td>Rimuove le righe che contengono un valore null per il livello di registrazione e memorizza i risultati in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;</td><td>Raggruppa le righe in base al livello di registrazione e memorizza i risultati in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;</td><td>Crea un nuovo set di dati che contiene ciascun valore univoco del livello di registrazione e il numero di occorrenze. I risultati vengono memorizzati in FREQUENCIES</td>
    </tr>
    <tr>
    <td>RESULT = order FREQUENCIES by COUNT desc;</td><td>Ordina i livelli di registrazione in base al numero (decrescente) e memorizza i risultati in RESULT</td>
    </tr>
</table>

6. È anche possibile salvare i risultati di una trasformazione usando l'istruzione `STORE` . Ad esempio, il seguente comando salva il valore `RESULT` nella directory **/example/data/pigout** nel contenitore di archiviazione predefinito per il cluster:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > I dati vengono memorizzati nella directory specificata nei file denominati **part-nnnnn**. Se la directory esiste già, si riceverà un messaggio di errore.
   >
   >
   
7. Per uscire dal prompt grunt, immettere la seguente istruzione.

        QUIT;

### <a name="pig-latin-batch-files"></a>File batch di Pig Latin
È inoltre possibile usare il comando Pig per eseguire processi Pig Latin contenuti in un file.

1. Dopo aver chiuso il prompt grunt, aprire il **Blocco note** e creare un nuovo file denominato **pigbatch.pig** nella directory **%PIG_HOME%**.
2. Digitare o incollare le seguenti righe nel file **pigbatch.pig** e quindi salvarlo:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Usare quanto segue per eseguire il file **pigbatch.pig** usando il comando pig.

        pig %PIG_HOME%\pigbatch.pig

    Al termine del processo batch, si otterrà il seguente output, corrispondente a quello ottenuto quando è stato usato `DUMP RESULT;` nei passaggi precedenti.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Riepilogo
Come si può notare, il comando Pig consente di eseguire in modo interattivo operazioni di MapReduce o di eseguire processi Pig Latin archiviati in un file batch.

## <a id="nextsteps"></a>Passaggi successivi
Per informazioni generali su Pig in HDInsight:

* [Usare Pig con Hadoop in HDInsight](hdinsight-use-pig.md)

Per informazioni su altre modalità d'uso di Hadoop in HDInsight:

* [Usare Hive con Hadoop in HDInsight](hdinsight-use-hive.md)
* [Usare MapReduce con Hadoop in HDInsight](hdinsight-use-mapreduce.md)
