---
title: Procedura dettagliata per la soluzione Connected Factory - Azure | Microsoft Docs
description: Descrizione dell'acceleratore di soluzioni di connected factory di Azure IoT e della relativa architettura.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 23b36fb647c2949dca1c5efe7f8194ec5a397965
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140401"
---
# <a name="connected-factory-solution-accelerator-walkthrough"></a>Procedura dettagliata dell'acceleratore di soluzioni di connected factory

L'[acceleratore di soluzioni ][lnk-preconfigured-solutions] di connected factory rappresenta l'implementazione di una soluzione industriale end-to-end che:

* Si connette sia a dispositivi industriali simulati che eseguono server OPC UA in linee di produzione simulate degli stabilimenti, sia a dispositivi server OPC UA reali. Per altre informazioni su OPC UA, vedere le [domande frequenti su Connected Factory](iot-accelerators-faq-cf.md).
* Visualizza KPI e OEE di tali dispositivi e delle linee di produzione.
* Dimostra come usare un'applicazione basata sul cloud per interagire con i sistemi server OPC UA.
* Consente di connettere i propri dispositivi server OPC UA.
* Consente di esplorare e modificare i dati dei server OPC UA.
* Si integra con il servizio Azure Time Series Insights (TSI) per fornire visualizzazioni personalizzate dei dati dei server OPC UA.

È possibile usare la soluzione come punto di partenza per l'implementazione e [personalizzarla][lnk-customize] in base ai requisiti aziendali specifici.

Questo articolo illustra alcuni degli elementi principali della soluzione Connected Factory per comprenderne il funzionamento. L'articolo descrive anche come i dati passano attraverso la soluzione. Queste informazioni consentono di:

* Risolvere i problemi nella soluzione.
* Pianificare come personalizzare la soluzione per soddisfare requisiti specifici.
* Progettare la propria soluzione IoT che usa i servizi di Azure.

Per altre informazioni, vedere le [Domande frequenti su Connected Factory](iot-accelerators-faq-cf.md).

## <a name="logical-architecture"></a>Architettura logica

Il diagramma seguente illustra i componenti logici dell'acceleratore di soluzioni:

![Architettura logica di Connected Factory][connected-factory-logical]

## <a name="communication-patterns"></a>Modelli di comunicazione

La soluzione usa la [specifica OPC UA Pub/Sub](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) per inviare i dati di telemetria OPC UA all'hub IoT in formato JSON. La soluzione usa il [modulo di pubblicazione OPC](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge per questo scopo.

La soluzione ha anche un client OPC UA integrato in un'applicazione Web che può stabilire connessioni con server OPC UA locali. Il client usa un [proxy inverso](https://wikipedia.org/wiki/Reverse_proxy) e riceve assistenza dall'hub IoT per stabilire la connessione senza che sia necessario aprire le porte nel firewall locale. Questo modello di comunicazione è denominato [modello di comunicazione assistita con servizi](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/). La soluzione usa il modulo [proxy OPC](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge per questo scopo.


## <a name="simulation"></a>Simulazione

Le postazioni simulate e i sistemi MES (Manufacturing Execution Systems) simulati costituiscono una linea di produzione dello stabilimento. I dispositivi simulati e il modulo di pubblicazione OPC si basano sullo [standard OPC UA .NET][lnk-OPC-UA-NET-Standard] pubblicato dalla OPC Foundation.

Il proxy OPC e il modulo di pubblicazione OPC vengono implementati come moduli basati su [Azure IoT Edge][lnk-Azure-IoT-Gateway]. Ogni linea di produzione simulata ha un gateway collegato.

Tutti i componenti di simulazione vengono eseguiti in contenitori Docker ospitati in una macchina virtuale Linux di Azure. Per impostazione predefinita, la simulazione è configurata per l'esecuzione di otto linee di produzione simulate.

## <a name="simulated-production-line"></a>Linea di produzione simulata

Una linea di produzione produce parti. È composta da postazioni diverse: una di montaggio, una di collaudo e una di imballaggio.

La simulazione viene eseguita e aggiorna i dati resi disponibili tramite i nodi OPC UA. Tutte le postazioni della linea di produzione simulata sono coordinate dal sistema MES tramite OPC UA.

## <a name="simulated-manufacturing-execution-system"></a>MES (Manufacturing Execution System) simulato

Il MES monitora ogni postazione nella linea di produzione tramite OPC UA per rilevare le modifiche di stato della postazione stessa. Chiama metodi OPC UA per controllare le postazioni e trasferisce un prodotto da una postazione all'altra finché non è completo.

## <a name="gateway-opc-publisher-module"></a>Modulo di pubblicazione OPC del gateway

Il modulo di pubblicazione OPC si connette ai server OPC UA della postazione ed effettua la sottoscrizione ai nodi OPC da pubblicare. Il modulo:

1. Converte i dati del nodo in formato JSON.
1. Consente di codificare il JSON.
1. Invia il JSON all'hub IoT sotto forma di messaggi OPC UA Pub/Sub.

Il modulo di pubblicazione OPC richiede solo una porta HTTPS in uscita (443) e può funzionare con l'infrastruttura aziendale esistente.

## <a name="gateway-opc-proxy-module"></a>Modulo proxy OPC del gateway

Il modulo proxy OPC UA del gateway effettua il tunnel dei messaggi di comando e controllo OPC UA binari e richiede solo una porta HTTPS in uscita (443). Può funzionare con l'infrastruttura aziendale esistente, inclusi i proxy Web.

Usa metodi di dispositivo hub IoT per trasferire pacchetti di dati TCP/IP a livello di applicazione per garantire l'attendibilità degli endpoint, la crittografia dei dati e l'integrità tramite SSL/TLS.

Il protocollo binario OPC UA inoltrato attraverso il proxy stesso usa la crittografia e l'autenticazione UA.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Il modulo di pubblicazione OPC del gateway effettua la sottoscrizione ai nodi del server OPC UA per rilevare le modifiche ai valori dei dati. Se viene rilevata una modifica dei dati in uno dei nodi, questo modulo invia messaggi all'hub IoT di Azure.

L'hub IoT fornisce un'origine evento ad Azure Time Series Insights. Time Series Insights archivia i dati per 30 giorni in base ai timestamp associati ai messaggi. Questi dati includono:

* ApplicationUri OPC UA
* NodeId OPC UA
* Valore del nodo
* Timestamp di origine
* DisplayName OPC UA

Time Series Insights non consente attualmente ai clienti di definire il periodo di conservazione dei dati.

Time Series Insights esegue query sui dati del nodo usando una funzione **SearchSpan** basata sul tempo ed esegue l'aggregazione in base ad **ApplicationUri OPC UA**, **NodeId OPC UA** o **DisplayName OPC UA**.

Per recuperare i dati per i misuratori OEE e KPI e i grafici di serie temporali, la soluzione aggrega i dati per numero di eventi, **Sum**, **Avg**, **Min** e **Max**.

Le serie temporali vengono compilate usando un processo differente. La soluzione calcola i valori OEE e KPI a partire dai dati di base della postazione e propaga i valori per le linee di produzione, le industrie e l'impresa.

La serie temporale per la topologia OEE e KPI viene calcolata nell'app ogni volta che un intervallo di tempo visualizzato è pronto, ad esempio la visualizzazione giornaliera viene aggiornata ogni ora.

La visualizzazione della serie temporale dei dati del nodo proviene direttamente da Time Series Insights tramite un'aggregazione per timespan.

## <a name="iot-hub"></a>Hub IoT
L'[hub IoT][lnk-IoT Hub] riceve i dati inviati dal modulo di pubblicazione OPC nel cloud e li mette a disposizione del servizio di Azure Time Series Insights. 

L'hub IoT nella soluzione esegue anche le operazioni seguenti:
- Gestisce un registro delle identità che archivia gli ID per tutti i moduli di pubblicazione OPC e tutti i moduli proxy OPC.
- Viene usato come canale di trasporto per la comunicazione bidirezionale del modulo proxy OPC.

## <a name="azure-storage"></a>Archiviazione di Azure
La soluzione usa Archiviazione BLOB di Azure come spazio di archiviazione su disco per la macchina virtuale e per i dati di distribuzione.

## <a name="web-app"></a>app Web
L'app Web distribuita nell'ambito dell'acceleratore di soluzioni include un client OPC UA integrato, elaborazione degli avvisi e visualizzazione dei dati di telemetria.

## <a name="telemetry-data-flow"></a>Flusso di dati di telemetria

![Flusso di dati di telemetria](./media/iot-accelerators-connected-factory-sample-walkthrough/telemetry_dataflow.png)

### <a name="flow-steps"></a>Passaggi del flusso

1. Il modulo di pubblicazione OPC legge i certificati OPC UA X509 necessari e le credenziali di sicurezza dell'hub IoT dall'archivio certificati locale.
    - Se necessario, il modulo di pubblicazione OPC crea e archivia i certificati o le credenziali mancanti nell'archivio certificati.

2. Il modulo di pubblicazione OPC si registra con l'hub IoT.
    - Usa il protocollo configurato. Può usare qualsiasi protocollo supportato dall'SDK client per l'hub IoT. Il protocollo predefinito è MQTT.
    - La comunicazione del protocollo è protetta da TLS.

3. Il modulo di pubblicazione OPC legge il file di configurazione.

4. Il modulo di pubblicazione OPC crea una sessione OPC con ogni server OPC UA configurato.
    - Usa la connessione TCP.
    - Il modulo di pubblicazione e il server OPC UA si autenticano reciprocamente usando i certificati X509.
    - Tutto il traffico OPC UA successivo viene crittografato dal meccanismo di crittografia OPC UA configurato.
    - Il modulo di pubblicazione OPC crea una sottoscrizione OPC nella sessione OPC per ogni intervallo di pubblicazione configurato.
    - Crea elementi OPC monitorati per i nodi OPC da pubblicare nella sottoscrizione OPC.

5. Se il valore di un nodo OPC monitorato cambia, il server OPC UA invia gli aggiornamenti al modulo di pubblicazione OPC.

6. Il modulo di pubblicazione OPC transcodifica il nuovo valore.
    - Invia più modifiche in batch se l'invio in batch è abilitato.
    - Crea un messaggio dell'hub IoT.

7. Il modulo di pubblicazione OPC invia un messaggio all'hub IoT.
    - Usare il protocollo configurato.
    - La comunicazione è protetta da TLS.

8. Time Series Insights (TSI) legge i messaggi dall'hub IoT.
    - Usa AMQP su TCP/TLS.
    - Questo passaggio è interno al data center.

9. Dati inattivi in TSI.

10. L'app Web di Connected Factory nel Servizio app di Azure richiede i dati necessari da TSI.
    - Usa la comunicazione protetta TCP/TLS.
    - Questo passaggio è interno al data center.

11. Il Web browser si connette all'App Web di Connected Factory.
    - Visualizza il dashboard di Connected Factory.
    - Si connette tramite HTTPS.
    - Per accedere all'app di Connected Factory è necessaria l'autenticazione dell'utente tramite Azure Active Directory.
    - Le chiamate API Web all'app di Connected Factory sono protette da token antifalsificazione.

12. Quando i dati vengono aggiornati, l'App Web di Connected Factory li invia al Web browser.
    - Usa il protocollo SignalR.
    - Applica la protezione TCP/TLS.

## <a name="browsing-data-flow"></a>Esplorazione del flusso di dati

![Esplorazione del flusso di dati](./media/iot-accelerators-connected-factory-sample-walkthrough/browsing_dataflow.png)

### <a name="flow-steps"></a>Passaggi del flusso

1. Il proxy OPC (componente server) viene avviato.
    - Legge le chiavi di accesso condiviso da un archivio locale.
    - Se necessario, salva le chiavi di accesso mancanti nell'archivio.

2. Il proxy OPC (componente server)si registra con l'hub IoT.
    - Legge tutti i dispositivi noti dall'hub IoT.
    - Usa MQTT su TLS su socket o WebSocket sicuro.

3. Il Web browser si connette all'app Web di Connected Factory e visualizza il dashboard di Connected Factory.
    - Usa HTTPS.
    - Un utente seleziona un server OPC UA a cui connettersi.

4. L'App Web di Connected Factory stabilisce una sessione OPC UA con il server OPC UA selezionato.
    - Usa lo stack OPC UA.

5. Il trasporto proxy OPC riceve una richiesta dallo stack OPC UA per stabilire una connessione socket TCP al server OPC UA.
    - Recupera solo il payload TCP e lo usa senza modificarlo.
    - Questo passaggio è interno all'App Web di Connected Factory.

6. Il proxy OPC (componente client) cerca il dispositivo proxy OPC (componente server) nel Registro di sistema del dispositivo hub IoT. Chiama quindi un metodo del dispositivo proxy OPC (componente server) nell'hub IoT.
    - Usa HTTPS su TCP/TLS per cercare il proxy OPC.
    - Usa HTTPS su TCP/TLS per stabilire una connessione socket TCP al server OPC UA.
    - Questo passaggio è interno al data center.

7. L'hub IoT chiama quindi un metodo nel dispositivo proxy OPC (componente server).
    - Usa una connessione MQTT su TLS su socket o Websocket sicuro stabilita per stabilire una connessione socket TCP al server OPC UA.

8. Il proxy OPC (componente server) invia il payload TCP alla rete di produzione.

9. Il server OPC UA elabora il payload e invia la risposta.

10. La risposta viene ricevuta dal socket del proxy OPC (componente server).
    - Il proxy OPC invia i dati come valore restituito del metodo del dispositivo all'hub IoT e al proxy OPC (componente client).
    - Questi dati vengono inviati allo stack OPC UA nell'app di Connected Factory.

11. L'App Web di Connected Factory restituisce l'esperienza utente del browser OPC arricchita con le informazioni specifiche di OPC UA ricevute dal server OPC UA al Web browser per eseguirne il rendering.
    - Mentre un utente esplora lo spazio indirizzi OPC e applica le funzioni ai nodi nello spazio indirizzi OPC, il client dell'esperienza utente del browser OPC usa le chiamate AJAX su HTTPS protette con i token antifalsificazione per ottenere i dati dall'app Web di Connected Factory.
    - Se necessario, il client usa la comunicazione illustrata nei passaggi da 4 a 10 per scambiare informazioni con il server OPC UA.

> [!NOTE]
> Il proxy OPC (componente server) e il componente proxy OPC (client) completano i passaggi da 4 a 10 per tutto il traffico TCP correlato alla comunicazione OPC UA.

> [!NOTE]
> Per il server OPC UA e lo stack OPC UA nell'App Web di Connected Factory, la comunicazione del proxy OPC è trasparente e vengono applicate tutte le funzionalità di sicurezza OPC UA per l'autenticazione e la crittografia.

## <a name="next-steps"></a>Passaggi successivi

È possibile proseguire con l'introduzione agli acceleratori di soluzioni IoT leggendo gli articoli seguenti:

* [Autorizzazioni per il sito azureiotsuite.com][lnk-permissions]
* [Distribuire un gateway in Windows o Linux per gli acceleratori di soluzioni di connected factory](iot-accelerators-connected-factory-gateway-deployment.md)
* [OPC Publisher reference implementation (Implementazione di riferimento del modulo di pubblicazione OPC)](https://github.com/Azure/iot-edge-opc-publisher/blob/master/README.md).

[connected-factory-logical]:media/iot-accelerators-connected-factory-sample-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]:about-iot-accelerators.md
[lnk-customize]: iot-accelerators-connected-factory-customize.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-accelerators-faq.md
