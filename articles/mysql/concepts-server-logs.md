---
title: Log del server per Database di Azure per MySQL
description: Descrive i log disponibili nel Database di Azure per MySQL e i parametri disponibili per l'abilitazione di diversi livelli di registrazione.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/03/2018
ms.openlocfilehash: 73be0e4ecff4bc0d9b69249430bba69a93cc54ae
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093783"
---
# <a name="server-logs-in-azure-database-for-mysql"></a>Log del server nel Database di Azure per MySQL
Nel Database di Azure per MySQL, il log delle query lente è disponibile per gli utenti. L'accesso al log delle transazioni non è supportato. Il log delle query lente può essere usato per identificare eventuali colli di bottiglia delle prestazioni e procedere alla risoluzione dei problemi. 

Per altre informazioni sul log delle query lente MySQL, vedere la [sezione relativa ai log di query lente](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) del manuale di riferimento per MySQL.

## <a name="access-server-logs"></a>Accesso ai registri server
È possibile elencare e scaricare i log del server del Database di Azure per MySQL usando il portale di Azure e l'interfaccia della riga di comando di Azure.

Nel portale di Azure selezionare il server del Database di Azure per MySQL. Nell'intestazione **Monitoraggio** selezionare la pagina **Log del server**.

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere [Configurare e accedere ai log del server usando l'interfaccia della riga di comando di Azure](howto-configure-server-logs-in-cli.md).

## <a name="log-retention"></a>Conservazione dei log
I log sono disponibili per un massimo di sette giorni dalla data di creazione. Se le dimensioni totali dei log disponibili superano 7 GB, i file meno recenti vengono eliminati fino a quando non è disponibile dello spazio. 

I log vengono ruotati ogni 24 ore o 7 GB, a seconda del valore raggiunto per primo.


## <a name="configure-logging"></a>Configurare la registrazione 
Per impostazione predefinita il log delle query lente è disabilitato. Per abilitarlo, impostare slow_query_log su ON.

Altri parametri che è possibile modificare includono:

- **long_query_time**: se una query richiede più tempo del valore di long_query_time (in secondi), la query viene registrata. Il valore predefinito è 10 secondi.
- **log_slow_admin_statements**: se è ON include le istruzioni a livello amministrativo come ALTER_TABLE e ANALYZE_TABLE nelle istruzioni scritte in slow_query_log.
- **log_queries_not_using_indexes**: determina se le query che non usano gli indici vengono registrate in slow_query_log
- **log_throttle_queries_not_using_indexes**: questo parametro limita il numero di query non di indice che possono essere scritte nel log di query lente. Questo parametro ha effetto quando log_queries_not_using_indexes è impostato su ON.

Per una descrizione completa dei parametri per il log di query lente, vedere la [documentazione sul log di query lente](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) per MySQL.

## <a name="diagnostic-logs"></a>Log di diagnostica
Database di Azure per MySQL è integrato con i log di diagnostica di Monitoraggio di Azure. Dopo aver abilitato i log query lente nel server MySQL, è possibile scegliere se inviarli a Log Analytics, Hub eventi o Archiviazione di Azure. Per altre informazioni sull'abilitazione dei log di diagnostica, vedere la sezione sulle procedure della [documentazione sui log di diagnostica](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

La tabella seguente descrive il contenuto di ogni log. A seconda del metodo di output, è possibile che i campi inclusi e il relativo ordine di visualizzazione siano differenti.

| **Proprietà** | **Descrizione** |
|---|---|---|
| TenantId | ID del tenant. |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Timestamp in cui il log è stato registrato in formato UTC. |
| type | Tipo di log. Sempre `AzureDiagnostics` |
| SubscriptionId | GUID per la sottoscrizione a cui appartiene il server. |
| ResourceGroup | Nome del gruppo di risorse a cui appartiene il server. |
| ResourceProvider | Nome del provider di risorse. Sempre `MICROSOFT.DBFORMYSQL` |
| ResourceType | `Servers` |
| ResourceId | URI della risorsa |
| Risorsa | Nome del server |
| Categoria | `MySqlSlowLogs` |
| OperationName | `LogEvent` |
| Logical_server_name_s | Nome del server |
| start_time_t [UTC] | Ora di inizio della query |
| query_time_s | Tempo totale di esecuzione della query |
| lock_time_s | Tempo totale in cui la query è rimasta bloccata |
| user_host_s | Username |
| rows_sent_s | Numero di righe inviate |
| rows_examined_s | Numero di righe esaminate |
| last_insert_id_s | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| insert_id_s | ID inserimento |
| sql_text_s | Query completa |
| server_id_s | Id del server |
| thread_id_s | ID thread |
| \_ResourceId | URI della risorsa |

## <a name="next-steps"></a>Passaggi successivi
- [Come configurare e accedere ai log del server usando l'interfaccia della riga di comando di Azure](howto-configure-server-logs-in-cli.md).
