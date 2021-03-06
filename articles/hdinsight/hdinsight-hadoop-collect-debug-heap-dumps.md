---
title: Eseguire il debug e analizzare i servizi Apache Hadoop con i dump dell'heap - Azure
description: Raccogliere automaticamente i dump dell'heap per i servizi Apache Hadoop e inserirli nell'account di archiviazione BLOB di Azure per il debug e l'analisi.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 2a399899c93addf966d3f2ec0e36d4b1c76b686f
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51038308"
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-apache-hadoop-services"></a>Raccogliere i dump dell'heap nell'archiviazione BLOB per eseguire il debug e analizzare i servizi Apache Hadoop
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

I dump dell'heap includono uno snapshot della memoria dell'applicazione, ad esempio i valori delle variabili al momento della creazione del dump. Si rivelano quindi utili per diagnosticare i problemi che si verificano in fase di esecuzione. È possibile raccogliere automaticamente i dump dell'heap per i servizi Apache Hadoop e posizionarli nell'account di archiviazione BLOB di Azure di un utente in HDInsightHeapDumps/.

La raccolta di dump dell'heap relativi ai diversi servizi deve essere abilitata sui singoli cluster. Per impostazione predefinita, questa funzionalità è disattivata per un cluster. Poiché i dump dell'heap possono avere dimensioni elevate, è consigliabile monitorare l'account di archiviazione BLOB in cui vengono salvati dopo l'abilitazione della raccolta.

> [!IMPORTANT]
> Linux è l'unico sistema operativo usato in HDInsight versione 3.4 o successiva. Per altre informazioni, vedere la sezione relativa al [ritiro di HDInsight in Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Le informazioni contenute in questo articolo si applicano solo a HDInsight basato su Windows. Per informazioni su HDInsight basato su Linux, vedere [Abilitare i dump dell'heap per i servizi Hadoop in HDInsight basato su Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>Servizi idonei per i dump dell'heap
È possibile abilitare dump dell'heap per i servizi seguenti:

* **Apache hcatalog** - tempelton
* **Apache hive** - hiveserver2, metastore, derbyserver
* **mapreduce** - jobhistoryserver
* **Apache yarn** - resourcemanager, nodemanager, timelineserver
* **Apache hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Elementi di configurazione per l'abilitazione dei dump dell'heap
Per attivare i dump di heap per un servizio, è necessario impostare gli elementi di configurazione appropriati nella sezione relativa al servizio in questione, specificata da **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Il valore di **service_name** può essere uno qualsiasi dei servizi qui elencati: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode o namenode.

## <a name="enable-using-azure-powershell"></a>Abilitare con Azure PowerShell
Ad esempio, per attivare i dump dell'heap usando Azure PowerShell per jobhistoryserver, è possibile usare lo script seguente:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Abilitare con .NET SDK
Ad esempio, per attivare i dump dell'heap usando Azure HDInsight .NET SDK per jobhistoryserver, è possibile usare il codice seguente:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
