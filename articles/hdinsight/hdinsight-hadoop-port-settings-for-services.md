---
title: Porte usate dai servizi Hadoop su HDInsight - Azure
description: Un elenco di porte usate dai servizi Hadoop in esecuzione su HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: e6204933d6b9a4a6b296a141520fc8887c9181f1
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51279713"
---
# <a name="ports-used-by-hadoop-services-on-hdinsight"></a>Porte usate dai servizi Hadoop su HDInsight

Questo documento fornisce un elenco delle porte usate dai servizi Hadoop in esecuzione nei cluster HDInsight basati su Linux. Fornisce anche informazioni sulle porte usate per connettersi al cluster tramite SSH.

## <a name="public-ports-vs-non-public-ports"></a>Porte pubbliche e porte non pubbliche

I cluster HDInsight basati su Linux espongono pubblicamente solo tre porte su Internet: 22, 23 e 443. Queste porte vengono usate per accedere in modo sicuro al cluster tramite SSH e ai servizi esposti tramite il protocollo HTTPS protetto.

Internamente, HDInsight viene implementato da più macchine virtuali di Azure (i nodi all'interno del cluster) in esecuzione su una rete virtuale di Azure. Dall'interno della rete virtuale è possibile accedere alle porte non esposte a Internet. Ad esempio, se ci si connette a uno dei nodi head tramite SSH, dal nodo head è possibile accedere direttamente ai servizi in esecuzione sui nodi del cluster.

> [!IMPORTANT]
> Se non si specifica una rete virtuale di Azure come opzione di configurazione per HDInsight, se ne crea automaticamente una. Tuttavia, non è possibile aggiungere altri computer (ad esempio altre macchine virtuali di Azure o nel computer di sviluppo client) a questa rete virtuale.


Per aggiungere altre macchine alla rete virtuale, creare innanzitutto la rete virtuale e specificarla durante la creazione del cluster HDInsight. Per altre informazioni, vedere [Estendere le funzionalità di HDInsight usando Rete virtuale di Azure](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Porte pubbliche

Tutti i nodi di un cluster HDInsight si trovano all'interno di una rete virtuale Azure e non sono accessibile direttamente da Internet. Un gateway pubblico fornisce accesso a Internet per le porte seguenti, comuni a tutti i tipi di cluster HDInsight.

| Service | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- |
| sshd |22 |SSH |Connette i client a SSHD sul nodo head primario. Per altre informazioni, vedere [Usare SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |22 |SSH |Connette i client a sshd sul nodo perimetrale. Per altre informazioni, vedere [Usare SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |23 |SSH |Connette i client a SSHD sul nodo head secondario. Per altre informazioni, vedere [Usare SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Ambari |443 |HTTPS |Interfaccia utente Web Ambari Vedere [Gestire i cluster HDInsight mediante l'utilizzo dell'interfaccia utente Web Ambari](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |API REST Ambari Vedere [Gestire i cluster HDInsight mediante l'API REST Ambari](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |API REST HCatalog Vedere gli articoli sull'[uso di Hive con Curl](hadoop/apache-hadoop-use-pig-curl.md), l'[uso di Pig con Curl](hadoop/apache-hadoop-use-pig-curl.md) e l'[uso di MapReduce con Curl](hadoop/apache-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |Esegue la connessione ad Hive tramite ODBC. Vedere [Connettere Excel a HDInsight mediante Microsoft Hive ODBC Driver](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |Esegue la connessione ad Hive tramite JDBC. Vedere [Connettersi a Hive in Azure HDInsight con il driver Hive JDBC](hadoop/apache-hadoop-connect-hive-jdbc-driver.md) |

Gli elementi seguenti sono disponibili per tipi di cluster specifici:

| Service | Porta | Protocollo | Tipo di cluster | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |hbase |API REST HBase Vedere [Introduzione all'uso di HBase](hbase/apache-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |API REST Spark Vedere [Inviare processi Spark in modalità remota con Livy](spark/apache-spark-livy-rest-interface.md) |
| Server Spark Thrift |443 |HTTPS |Spark |Server Spark Thrift usato per inviare query Hive. Vedere [Usare Beeline con Hive in HDInsight](hadoop/apache-hadoop-use-hive-beeline.md) |
| Storm |443 |HTTPS |Storm |Interfaccia utente Web di Storm Vedere [Distribuire e gestire topologie Apache Storm in HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Authentication

Tutti i servizi esposti pubblicamente su Internet devono essere autenticati:

| Porta | Credenziali |
| --- | --- |
| 22 o 23 |Le credenziali utente SSH specificate durante la creazione del cluster |
| 443 |Il nome di accesso (impostazione predefinita: admin) e la password impostati durante la creazione del cluster |

## <a name="non-public-ports"></a>Porte non pubbliche

> [!NOTE]
> Alcuni servizi sono disponibili solo su tipi di cluster specifici. Ad esempio, HBase è disponibile solo su tipi di cluster HBase.

> [!IMPORTANT]
> Alcuni servizi vengono eseguiti in un solo nodo head alla volta. Se quando si tenta di connettersi al servizio nel nodo head primario viene visualizzato un errore, riprovare usando il nodo head secondario.

### <a name="ambari"></a>Ambari

| Service | Nodi | Porta | Percorso URL | Protocollo | 
| --- | --- | --- | --- | --- |
| Interfaccia utente Web Ambari | Nodi head | 8080 | / | HTTP |
| API REST Ambari | Nodi head | 8080 | /api/v1 | HTTP |

Esempi:

* API REST Ambari: `curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>Porte HDFS

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| Interfaccia utente Web NameNode |Nodi head |30070 |HTTPS |Interfaccia utente Web per visualizzare lo stato |
| Servizio metadati NameNode |Nodi head |8020 |IPC |Metadati del file system |
| DataNode |Tutti i nodi di lavoro |30075 |HTTPS |Interfaccia utente Web per visualizzare lo stato, i log e così via. |
| DataNode |Tutti i nodi di lavoro |30010 |&nbsp; |Trasferimento dati |
| DataNode |Tutti i nodi di lavoro |30020 |IPC |Operazioni sui metadati |
| NameNode secondario |Nodi head |50090 |HTTP |Checkpoint per i metadati NameNode |

### <a name="yarn-ports"></a>Porte YARN

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| Interfaccia utente Web di Resource Manager |Nodi head |8088 |HTTP |Interfaccia utente Web per Resource Manager |
| Interfaccia utente Web di Resource Manager |Nodi head |8090 |HTTPS |Interfaccia utente Web per Resource Manager |
| Interfaccia di amministrazione di Resource Manager |Nodi head |8141 |IPC |Per gli invii delle applicazioni (Hive, server Hive, Pig e così via) |
| Utilità di pianificazione di Resource Manager |Nodi head |8030 |HTTP |Interfaccia di amministrazione |
| Interfaccia dell'applicazione Resource Manager |Nodi head |8050 |HTTP |Indirizzo dell'interfaccia di gestione delle applicazioni |
| NodeManager |Tutti i nodi di lavoro |30050 |&nbsp; |L'indirizzo del gestore di contenitore |
| Interfaccia utente Web di NodeManager |Tutti i nodi di lavoro |30060 |HTTP |Interfaccia di Resource Manager |
| Indirizzo di Timeline |Nodi head |10200 |RPC |Il servizio RPC del servizio Timeline. |
| Interfaccia utente Web di Timeline |Nodi head |8181 |HTTP |L'interfaccia utente Web del servizio Timeline |

### <a name="hive-ports"></a>Porte Hive

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| HiveServer2 |Nodi head |10001 |Thrift |Servizio per la connessione ad Hive (Thrift/JDBC) |
| Metastore Hive |Nodi head |9083 |Thrift |Servizio per la connessione ai metadati Hive (Thrift/JDBC) |

### <a name="webhcat-ports"></a>Porte WebHCat

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| Server WebHCat |Nodi head |30111 |HTTP |API Web su HCatalog e su altri servizi Hadoop |

### <a name="mapreduce-ports"></a>Porte MapReduce

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| JobHistory |Nodi head |19888 |HTTP |Interfaccia utente Web di MapReduce JobHistory |
| JobHistory |Nodi head |10020 |&nbsp; |Server di MapReduce JobHistory |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Trasferisce output intermedi di Map ai reducer che eseguono la richiesta |

### <a name="oozie"></a>Oozie

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| Server di Oozie |Nodi head |11000 |HTTP |URL per il servizio Oozie |
| Server di Oozie |Nodi head |11001 |HTTP |Porta per l'amministrazione di Oozie |

### <a name="ambari-metrics"></a>Metriche di Ambari

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| TimeLine (cronologia delle applicazioni) |Nodi head |6188 |HTTP |L'interfaccia utente Web del servizio Timeline |
| TimeLine (cronologia delle applicazioni) |Nodi head |30200 |RPC |L'interfaccia utente Web del servizio Timeline |

### <a name="hbase-ports"></a>Porte HBase

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| HMaster |Nodi head |16000 |&nbsp; |&nbsp; |
| Interfaccia utente Web informativa di HMaster |Nodi head |16010 |HTTP |La porta per l'interfaccia utente Web Master HBase |
| Server dell'area |Tutti i nodi di lavoro |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |La porta usata dai client per connettersi a ZooKeeper |

### <a name="kafka-ports"></a>Porte Kafka

| Service | Nodi | Porta | Protocollo | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| Gestore |Nodi di lavoro |9092 |[Protocollo di trasmissione Kafka](http://kafka.apache.org/protocol.html) |Usato per la comunicazione di client |
| &nbsp; |Nodi Zookeeper |2181 |&nbsp; |La porta usata dai client per connettersi a ZooKeeper |

### <a name="spark-ports"></a>Porte Spark

| Service | Nodi | Porta | Protocollo | Percorso URL | DESCRIZIONE |
| --- | --- | --- | --- | --- | --- |
| Server Spark Thrift |Nodi head |10002 |Thrift | &nbsp; | Servizio per la connessione a Spark SQL (Thrift/JDBC) |
| Server Livy | Nodi head | 8998 | HTTP | &nbsp; | Servizio per l'esecuzione di istruzioni, processi e applicazioni |
| Notebook di Jupyter | Nodi head | 8001 | HTTP | &nbsp; | Sito Web Notebook di Jupyter |

Esempi:

* Livy: `curl -u admin -G "http://10.0.0.11:8998/"`. In questo esempio, `10.0.0.11` è l'indirizzo IP del nodo head che ospita il servizio Livy.
