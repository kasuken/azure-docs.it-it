---
title: Portale di che si sposta in Azure | Microsoft Docs
description: Il portale di OMS sta terminando il servizio con tutte le funzionalità che si trasferiscono nel portale di Azure. Questo articolo fornisce informazioni dettagliate su questa transizione.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 2e14112220eeea1df094ff0c5844429fa3891a9b
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612806"
---
# <a name="oms-portal-moving-to-azure"></a>Portale di che si sposta in Azure

> [!NOTE]
> Questo articolo si applica sia al cloud pubblico che al cloud per enti pubblici di Azure, salvo altrimenti indicato.

Il portale di Azure è l'hub per tutti i servizi di Azure e offre un'avanzata esperienza di gestione con funzionalità come ad esempio i dashboard per il blocco delle risorse, la ricerca intelligente per le risorse di individuazione e assegnazione di tag per la gestione delle risorse. Per consolidare e semplificare il flusso di lavoro di monitoraggio e gestione, l'azienda ha iniziato ad aggiungere le funzionalità del portale di OMS al portale di Azure. Tutte le funzionalità del portale di OMS fanno ora parte del portale di Azure. In realtà, alcune delle nuove funzionalità, ad esempio Analisi del traffico, sono disponibili solo nel portale di Azure. L'utente sarà in grado di eseguire tutto ciò che era nel portale di OMS con il portale di Azure e molto altro. È consigliabile iniziare a usare il portale di Azure oggi stesso!

**Il portale di OMS verrà ritirato ufficialmente il 15 gennaio 2019.** L'azienda è entusiasta di passare al portale di Azure e prevede che la transizione sarà semplice. Ma è comprensibile che le modifiche siano complesse e che possano rivelarsi dannose. Inviare eventuali domande, feedback o preoccupazioni a **LAUpgradeFeedback@microsoft.com**. Il resto di questo articolo tratterà degli scenari chiave e della Guida di orientamento per questa transizione.

## <a name="what-is-changing"></a>Cosa cambierà 
Vengono annunciate le seguenti modifiche con la deprecazione del portale di OMS. Ognuna di queste modifiche viene descritta più nei dettagli nelle sezioni seguenti.

- Sarà possibile creare [nuove aree di lavoro solo](#new-workspaces) nel portale di Azure.
- La nuova esperienza di gestione degli avvisi [sostituisce la Soluzione Gestione avvisi](#changes-to-alerts).
- [La gestione degli accessi utente](#user-access-and-role-migration) viene effettuata nel portale di Azure tramite il controllo degli accessi in base al ruolo di Azure.
- Il [Connettore di Application Insights non è più necessario](#application-insights-connector-and-solution) perché la stessa funzionalità è abilitata tramite le query tra aree di lavoro.
- L'[App per dispositivi mobili OMS](#oms-mobile-app) è deprecata. 
- La [soluzione di sicurezza NSG viene sostituita](#azure-network-security-group-analytics) con la funzionalità avanzata disponibile tramite una soluzione di Analisi del traffico.
- Le nuove connessioni da System Center Operations Manager a Log Analytics richiedono [Management Pack aggiornati](#system-center-operations-manager).
- Vedere [Eseguire la migrazione delle distribuzioni di aggiornamenti di OMS in Azure](../automation/migrate-oms-update-deployments.md) per informazioni dettagliate sulle modifiche a [Gestione aggiornamenti](../automation/automation-update-management.md).


## <a name="what-should-i-do-now"></a>Qual è il passo da fare ora?
Mentre la maggior parte delle funzionalità continuerà a funzionare senza eseguire la migrazione, è necessario eseguire le seguenti attività:

- È necessario [eseguire la migrazione delle autorizzazioni utente](#user-access-and-role-migration) al portale di Azure.
- Vedere [Eseguire la migrazione delle distribuzioni di aggiornamenti di OMS in Azure](../automation/migrate-oms-update-deployments.md) per informazioni dettagliate sulla transizione della soluzione Gestione aggiornamenti.

Consultare [Domande frequenti per la transizione dal portale di OMS al portale di Azure per gli utenti di Log Analytics](../log-analytics/log-analytics-oms-portal-faq.md) per informazioni su come effettuare la transizione al portale di Azure. Inviare eventuali commenti e suggerimenti, domande o problemi a **LAUpgradeFeedback@microsoft.com**.

## <a name="user-access-and-role-migration"></a>Migrazione del ruolo e dell'accesso utente
La gestione di accesso al portale Azure è più completa e più potente rispetto alla gestione di accesso nel portale di OMS. Vedere [Gestire le aree di lavoro](log-analytics-manage-access.md#manage-accounts-and-users) per informazioni dettagliate sulla gestione dell'accesso in Log Analytics.

> [!NOTE]
> Le versioni precedenti di questo articolo dichiaravano che le autorizzazioni che venivano convertite automaticamente dal portale di OMS al portale di Azure. La conversione automatica non è più prevista. È quindi necessario eseguire la conversione manualmente.

Si può già disporre di un accesso appropriato nel portale di Azure e, in questo caso, non è necessario apportare modifiche. Esistono un paio di casi in cui non si potrebbe disporre dell'accesso appropriato e, in questo caso, l'amministratore deve assegnare le autorizzazioni.

- Nel portale di OMS si dispone delle autorizzazioni utente di sola lettura, ma non delle autorizzazioni nel portale di Azure. 
- Nel portale di OMS si dispone delle autorizzazioni da collaboratore, ma solamente di quelle di utente di sola lettura nel portale di Azure.
 
In entrambi i casi, l'amministratore deve assegnare manualmente il ruolo appropriato nella tabella seguente. È consigliabile assegnare questo ruolo al gruppo di risorse o al livello di sottoscrizione.  Saranno fornite indicazioni prescrittive a breve per entrambi i casi.

| Autorizzazione del portale di OMS | Ruolo di Azure |
|:---|:---|
| ReadOnly | Lettore di Log Analytics |
| Collaboratore | Collaboratore di Log Analytics |
| Amministratore | Proprietario | 
 

## <a name="new-workspaces"></a>Nuove aree di lavoro
Non è più possibile creare nuove aree di lavoro usando il portale di OMS. Per creare una nuova area di lavoro nel portale di Azure, seguire le indicazioni riportate nell'articolo [Creare un'area di lavoro di Log Analytics nel portale di Azure](log-analytics-quick-create-workspace.md).

## <a name="changes-to-alerts"></a>Modifiche apportate agli avvisi

### <a name="alert-extension"></a>Estensione dell'avviso  

> [!NOTE]
> Gli avvisi sono stati interamente estesi al portale di Azure per il cloud pubblico. Le regole di avviso esistenti possono essere visualizzate anche nel portale di OMS, ma possono essere gestite solo nel portale di Azure. L'estensione degli avvisi nel portale di Azure verrà avviata per il cloud di Azure per enti pubblici nel mese di ottobre 2018.

Gli avvisi sono stati [estesi al portale di Azure](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Dopo questa procedura guidata, le azioni di gestione degli avvisi saranno disponibili solo nel portale di Azure. Gli avvisi esistenti continueranno a essere elencati nel portale di OMS, Se si accede agli avvisi a livello di codice tramite l'API REST per gli avvisi di Log Analytics o il modello risorse degli avvisi di Log Analytics sarà necessario usare i gruppi di azioni invece delle azioni nelle chiamate API, dei modelli di Azure Resource Manager e dei comandi di PowerShell.

### <a name="alert-management-solution"></a>soluzione Alert Management
Anziché la [soluzione di gestione di avviso](log-analytics-solution-alert-management.md), è possibile usare l'[interfaccia di avviso unificata del Monitoraggio di Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md) per visualizzare e gestire gli avvisi. Questa nuova esperienza aggrega avvisi provenienti da più origini tra cui gli avvisi di registro di Azure da Log Analytics. È possibile vedere le distribuzioni degli avvisi, sfruttare i vantaggi del raggruppamento automatizzato degli avvisi correlati tramite gruppi smart e visualizzare gli avvisi tra più sottoscrizioni durante l'applicazione di filtri avanzati. Tutte queste funzionalità sono disponibili in anteprima di avvio il 4 giugno 2018. La soluzione di gestione degli avvisi non sarà disponibile nel portale di Azure. 

I dati raccolti dalla soluzione di gestione degli avvisi (record con un tipo di avviso) continuano a essere in Log Analytics fino a quando la soluzione è installata per l'area di lavoro. All'inizio di agosto 2018, il flusso di avvisi dagli avvisi unificati nelle aree di lavoro verrà abilitato, sostituendo questa funzionalità. Alcune modifiche dello schema sono previste e verranno annunciate in un secondo momento.

## <a name="oms-mobile-app"></a>OMS Mobile App
L'app per dispositivi mobili OMS subirà la terminazione del servizio insieme al portale di OMS. Anziché l'app per dispositivi mobili OMS, per accedere alle informazioni sull'infrastruttura IT, sui dashboard e sulle query salvate, è possibile accedere al portale di Azure direttamente dal browser nel dispositivo mobile. Per ottenere avvisi, è necessario configurare i [Gruppi di azioni di Azure](../monitoring-and-diagnostics/monitoring-action-groups.md) per ricevere le notifiche sotto forma di SMS o di chiamata vocale

## <a name="application-insights-connector-and-solution"></a>Connettore di Application Insights e soluzione
[Connettore di Application Insights](log-analytics-app-insights-connector.md) fornisce un modo per importare i dati di Application Insights in un'area di lavoro di Log Analytics. Questa duplicazione dei dati è stata necessaria per abilitare la visibilità tra i dati dell'infrastruttura e dell'applicazione.

Con il supporto delle [query tra risorse](log-analytics-cross-workspace-search.md), non sarà più presente questa necessità di duplicare i dati. Di conseguenza, la soluzione Application Insights esistente verrà deprecata. A partire da ottobre, non sarà possibile collegare le nuove risorse di Application Insights alle Aree di lavoro di Log Analytics. I dashboard e i collegamenti esistenti continueranno a funzionare fino a gennaio 15, 2019.


## <a name="azure-network-security-group-analytics"></a>Analisi del gruppo di sicurezza di rete di Azure
La [soluzione Analisi gruppo di sicurezza di rete di Azure](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) verrà sostituita con l'ultimo lancio [Analisi del traffico](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) che offre visibilità nelle attività e nell'applicazione dell'utente su reti cloud. Analisi del traffico consente di controllare l'attività di rete dell'organizzazione, proteggere applicazioni e dati, ottimizzare le prestazioni dei carichi di lavoro e garantire la conformità. 

Questa soluzione consente di analizzare i Log del flusso del gruppo di sicurezza di rete e offre informazioni approfondite su quanto segue.

- Il flusso di traffico nelle reti tra Azure e Internet, nelle aree di cloud pubblico, nelle reti virtuali e nelle subnet
- Applicazioni e protocolli di rete, senza la necessità di sniffer o appliance di agenti di raccolta di flussi dedicati
- Talker principali, applicazioni con un livello di comunicazioni elevato, conversazioni tra macchine virtuali nel cloud e aree sensibili del traffico
- Origini e destinazioni del traffico su reti virtuali, interrelazioni tra applicazioni e servizi aziendali critici
- La sicurezza include il traffico dannoso, le porte aperte a Internet, le applicazioni o macchine virtuali che tentano di accedere a Internet.
- Utilizzo della capacità, che consente di eliminare i problemi di provisioning o sottoutilizzo.

È possibile continuare a fare affidamento sulle impostazioni di diagnostica per inviare i log di NSG a Log Analytics Log in modo che le ricerche avanzate, gli avvisi, i dashboard esistenti continueranno a funzionare correttamente. I clienti che hanno già installato la soluzione possono continuare a usarla fino al prossimo avviso. Dal 5 settembre la soluzione Network Security Group Analytics verrà rimossa dal marketplace e resa disponibile tramite la community come un [modello di avvio rapido di Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Se il [gruppo di gestione di Operations Manager è stato connesso a Log Analytics](log-analytics-om-agents.md), continuerà a funzionare senza alcuna variazione. Per le nuove connessioni, tuttavia, è necessario seguire le indicazioni fornite in [Microsoft System Center Operations Manager Management Pack to configure Operations Management Suite](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/) (Pacchetto di gestione di Microsoft System Center Operations Manager per configurare Operations Management Suite).

## <a name="next-steps"></a>Passaggi successivi
- Consultare [Domande frequenti per la transizione dal portale di OMS al portale di Azure per gli utenti di Log Analytics](log-analytics-oms-portal-faq.md) per il materiale sussidiario sullo spostamento dal portale di OMS al portale di Azure.
