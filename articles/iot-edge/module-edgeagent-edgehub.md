---
title: Informazioni di riferimento su EdgeAgent ed EdgeHub per Azure IoT | Microsoft Docs
description: Esaminare le proprietà specifiche e i rispettivi valori per i moduli gemelli edgeAgent ed edgeHub
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 01e1942b12de126aa34130f5a4b77dd0fb958aa6
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568921"
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Proprietà dei moduli gemelli "agente di Edge" e "hub di Edge"

L'agente di Edge e l'hub di Edge sono due moduli che costituiscono il runtime di IoT Edge. Per altre informazioni sulle operazioni eseguite da ciascun modulo, vedere [Informazioni sul runtime di Azure IoT Edge e la relativa architettura](iot-edge-runtime.md). 

Questo articolo descrive le proprietà desiderate e quelle segnalate dei moduli gemelli del runtime. Per altre informazioni su come distribuire i moduli nei dispositivi IoT Edge, vedere [Distribuzione e monitoraggio](module-deployment-monitoring.md).

## <a name="edgeagent-desired-properties"></a>Proprietà desiderate di EdgeAgent

Il dispositivo gemello del modulo per l'agente Edge è denominato `$edgeAgent` e coordina le comunicazioni tra l'agente Edge in esecuzione su un dispositivo e l'hub IoT. Le proprietà desiderate vengono impostate durante l'applicazione di un manifesto della distribuzione in un dispositivo specifico nell'ambito di una distribuzione di un singolo dispositivo o su larga scala. 

| Proprietà | DESCRIZIONE | Obbligatoria |
| -------- | ----------- | -------- |
| schemaVersion | Deve essere "1.0" | Yes |
| runtime.type | Deve essere "docker" | Yes |
| runtime.settings.minDockerVersion | Impostata sulla versione minima di Docker richiesta da questo manifesto della distribuzione | Yes |
| runtime.settings.loggingOptions | Un file JSON in formato stringa contenente le opzioni di registrazione per il contenitore dell'agente Edge. [Opzioni di registrazione di Docker](https://docs.docker.com/engine/admin/logging/overview/) | No  |
| runtime.settings.registryCredentials<br>.{registryId}.username | Nome utente del registro contenitori. Per Registro contenitori di Azure, il nome utente corrisponde in genere al nome del registro.<br><br> Le credenziali del registro sono necessarie per tutte le immagini di modulo che non sono pubbliche. | No  |
| runtime.settings.registryCredentials<br>.{registryId}.password | Password del registro contenitori. | No  |
| runtime.settings.registryCredentials<br>.{registryId}.address | Indirizzo del registro contenitori. Per Registro contenitori di Azure, l'indirizzo è in genere *{registryname}.azurecr.io*. | No  |  
| systemModules.edgeAgent.type | Deve essere "docker" | Yes |
| systemModules.edgeAgent.settings.image | URI dell'immagine dell'agente Edge. Attualmente, l'agente Edge non è in grado di aggiornarsi automaticamente. | Yes |
| systemModules.edgeAgent.settings<br>.createOptions | Un file JSON in formato stringa contenente le opzioni per la creazione del contenitore dell'agente Edge. [Opzioni di creazione di Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | No  |
| systemModules.edgeAgent.configuration.id | ID della distribuzione che ha distribuito questo modulo. | Questa proprietà è impostata dall'hub IoT quando questo manifesto viene applicato tramite una distribuzione. Non fa parte di un manifesto della distribuzione. |
| systemModules.edgeHub.type | Deve essere "docker" | Yes |
| systemModules.edgeHub.status | Deve essere "running" | Yes |
| systemModules.edgeHub.restartPolicy | Deve essere "always" | Yes |
| systemModules.edgeHub.settings.image | URI dell'immagine dell'hub Edge. | Yes |
| systemModules.edgeHub.settings<br>.createOptions | Un file JSON in formato stringa contenente le opzioni per la creazione del contenitore dell'hub Edge. [Opzioni di creazione di Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | No  |
| systemModules.edgeHub.configuration.id | ID della distribuzione che ha distribuito questo modulo. | Questa proprietà è impostata dall'hub IoT quando questo manifesto viene applicato tramite una distribuzione. Non fa parte di un manifesto della distribuzione. |
| modules.{moduleId}.version | Una stringa definita dall'utente che rappresenta la versione di questo modulo. | Yes |
| modules.{moduleId}.type | Deve essere "docker" | Yes |
| modules.{moduleId}.status | {"running" \| "stopped"} | Yes |
| modules.{moduleId}.restartPolicy | {"never" \| "on-failed" \| "on-unhealthy" \| "always"} | Yes |
| modules.{moduleId}.settings.image | URI dell'immagine del modulo. | Yes |
| modules.{moduleId}.settings.createOptions | Un file JSON in formato stringa contenente le opzioni per la creazione del contenitore del modulo. [Opzioni di creazione di Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | No  |
| modules.{moduleId}.configuration.id | ID della distribuzione che ha distribuito questo modulo. | Questa proprietà è impostata dall'hub IoT quando questo manifesto viene applicato tramite una distribuzione. Non fa parte di un manifesto della distribuzione. |

## <a name="edgeagent-reported-properties"></a>Proprietà segnalate di EdgeAgent

Le proprietà segnalate dell'agente Edge includono tre tipi di informazioni principali:

1. Lo stato dell'applicazione delle proprietà desiderate visualizzate per ultime;
2. Lo stato dei moduli attualmente in esecuzione nel dispositivo, come segnalato dall'agente Edge;
3. Una copia delle proprietà desiderate attualmente in esecuzione nel dispositivo.

Quest'ultima informazione è utile nel caso in cui le proprietà desiderate più recenti non vengano applicate correttamente dal runtime e il dispositivo esegua ancora un manifesto della distribuzione precedente.

> [!NOTE]
> Le proprietà segnalate dell'agente Edge sono utili in quanto possono essere sottoposte a query con il [linguaggio di query dell'hub IoT](../iot-hub/iot-hub-devguide-query-language.md) per esaminare lo stato delle distribuzioni su larga scala. Per altre informazioni su come usare le proprietà dell'agente Edge relative allo stato, vedere [Informazioni sulle distribuzioni IoT Edge per singoli dispositivi o su vasta scala](module-deployment-monitoring.md).

La tabella seguente non include le informazioni copiate dalle proprietà desiderate.

| Proprietà | DESCRIZIONE |
| -------- | ----------- |
| lastDesiredVersion | Questo integer si riferisce all'ultima versione delle proprietà desiderate elaborate dall'agente di Edge. |
| lastDesiredStatus.code | Questo è il codice di stato che fa riferimento alle ultime proprietà desiderate visualizzate dall'agente Edge. Valori consentiti: `200` Success, `400` Invalid configuration, `412` Invalid schema version, `417` the desired properties are empty, `500` Failed |
| lastDesiredStatus.description | Descrizione di testo dello stato |
| deviceHealth | `healthy` se lo stato di runtime di tutti i moduli è `running` o `stopped`, altrimenti `unhealthy` |
| configurationHealth.{deploymentId}.health | `healthy` se lo stato di runtime di tutti i moduli impostati da {deploymentId} della distribuzione è `running` o `stopped`, altrimenti `unhealthy` |
| runtime.platform.OS | Segnalazione del sistema operativo in esecuzione nel dispositivo |
| runtime.platform.architecture | Segnalazione dell'architettura della CPU nel dispositivo |
| systemModules.edgeAgent.runtimeStatus | Stato segnalato dell'agente Edge: {"running" \| "unhealthy"} |
| systemModules.edgeAgent.statusDescription | Descrizione di testo dello stato segnalato dell'agente Edge. |
| systemModules.edgeHub.runtimeStatus | Stato corrente dell'hub Edge: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| systemModules.edgeHub.statusDescription | Descrizione di testo dello stato corrente dell'hub Edge se unhealthy. |
| systemModules.edgeHub.exitCode | Se è stata chiusa, il codice di uscita segnalato dal contenitore dell'hub Edge |
| systemModules.edgeHub.startTimeUtc | Ora dell'ultimo avvio dell'hub Edge |
| systemModules.edgeHub.lastExitTimeUtc | Ora dell'ultima chiusura dell'hub Edge |
| systemModules.edgeHub.lastRestartTimeUtc | Ora dell'ultimo riavvio dell'hub Edge |
| systemModules.edgeHub.restartCount | Numero di tentativi di riavvio del modulo nell'ambito dei criteri di riavvio. |
| modules.{moduleId}.runtimeStatus | Stato corrente del modulo: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| modules.{moduleId}.statusDescription | Descrizione di testo dello stato corrente del modulo se unhealthy. |
| modules.{moduleId}.exitCode | Se è stata chiusa, il codice di uscita segnalato dal contenitore del modulo |
| modules.{moduleId}.startTimeUtc | Ora dell'ultimo avvio del modulo |
| modules.{moduleId}.lastExitTimeUtc | Ora dell'ultima chiusura del modulo |
| modules.{moduleId}.lastRestartTimeUtc | Ora dell'ultimo riavvio del modulo |
| modules.{moduleId}.restartCount | Numero di tentativi di riavvio del modulo nell'ambito dei criteri di riavvio. |

## <a name="edgehub-desired-properties"></a>Proprietà desiderate di EdgeHub

Il dispositivo gemello del modulo per l'hub Edge è denominato `$edgeHub` e coordina le comunicazioni tra l'hub Edge in esecuzione su un dispositivo e l'hub IoT. Le proprietà desiderate vengono impostate durante l'applicazione di un manifesto della distribuzione in un dispositivo specifico nell'ambito di una distribuzione di un singolo dispositivo o su larga scala. 

| Proprietà | DESCRIZIONE | Obbligatoria nel manifesto della distribuzione |
| -------- | ----------- | -------- |
| schemaVersion | Deve essere "1.0" | Yes |
| routes.{routeName} | Stringa che rappresenta una route dell'hub Edge. | L'elemento `routes` può essere presente, ma vuoto. |
| storeAndForwardConfiguration.timeToLiveSecs | Durata di conservazione dei messaggi in secondi da parte dell'hub di Edge in caso di endpoint di routing disconnessi, ad esempio disconnessi dall'hub IoT o dal modulo locale | Yes |

## <a name="edgehub-reported-properties"></a>Proprietà segnalate di EdgeHub

| Proprietà | DESCRIZIONE |
| -------- | ----------- |
| lastDesiredVersion | Questo integer si riferisce all'ultima versione delle proprietà desiderate elaborate dall'hub di Edge. |
| lastDesiredStatus.code | Questo è il codice di stato che fa riferimento alle ultime proprietà desiderate visualizzate dall'hub Edge. Valori consentiti: `200` Success, `400` Invalid configuration, `500` Failed |
| lastDesiredStatus.description | Descrizione di testo dello stato |
| clients.{device or moduleId}.status | Stato di connettività del dispositivo o del modulo. Valori possibili: {"connected" \| "disconnected"}. Solo le identità del modulo possono essere in stato disconnected. I dispositivi downstream che si connettono all'hub Edge vengono visualizzati solo se lo stato è connected. |
| clients.{device or moduleId}.lastConnectTime | Ultima ora di connessione del dispositivo o del modulo |
| clients.{device or moduleId}.lastDisconnectTime | Ultima ora di disconnessione del dispositivo o del modulo |

## <a name="next-steps"></a>Passaggi successivi

Per informazioni su come usare queste proprietà per generare i manifesti di distribuzione, vedere [Informazioni su come usare, configurare e riusare i moduli IoT Edge](module-composition.md).
