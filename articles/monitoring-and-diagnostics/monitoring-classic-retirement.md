---
title: Funzionalità unificata di avviso e monitoraggio in Monitoraggio di Azure in sostituzione delle funzionalità classiche di avviso e monitoraggio
description: Panoramica del ritiro delle funzionalità e dei servizi di monitoraggio classici, presenti in passato nella sezione Avvisi (versione classica) del portale di Azure. Le funzionalità classiche di avviso e monitoraggio includono avvisi sulle metriche classici per le risorse di Azure, avvisi sulle metriche classici per Application Insights, avvisi di test Web classici per Application Insights, avvisi classici basati su metriche personalizzate per Application Insights e avvisi classici per Application Insights SmartDetection v1
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 589aae8321d2c081f09ed46d9def2229d3973ffd
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613208"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Funzionalità unificata di avviso e monitoraggio in Monitoraggio di Azure in sostituzione delle funzionalità classiche di avviso e monitoraggio

Monitoraggio di Azure è ora diventato un servizio di monitoraggio unificato dell'intero stack che supporta metriche unificate e avvisi unificati per tutte le risorse. Per altre informazioni, vedere il [post di blog sul nuovo servizio Monitoraggio di Azure](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/). Le nuove piattaforme di monitoraggio e avviso di Azure sono state create per essere più veloci, intelligenti ed estendibili, per tenere il passo con la continua crescita del cloud computing e in linea con la filosofia Microsoft di cloud intelligente. 

Con la nuova piattaforma di monitoraggio e avviso di Azure, verrà ritirata la versione classica della piattaforma di monitoraggio e avviso, ospitata nella sezione *Visualizza avvisi classici* degli avvisi di Azure, che sarà deprecata entro giugno 2019.

 ![Avviso classico nel portale di Azure](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Gli utenti sono invitati a iniziare a usare la nuova piattaforma e ricreare al suo interno gli avvisi. Per i clienti con un numero elevato di avvisi, Microsoft sta lavorando per fornire un metodo automatico per spostare gli avvisi classici esistenti nel nuovo sistema di avvisi senza interruzioni o costi aggiuntivi.

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Metriche e avvisi unificati in Application Insights

La nuova piattaforma per le metriche di Monitoraggio di Azure supporterà ora il monitoraggio da Application Insights. Ciò significa che Application Insights verrà collegato ai gruppi di azioni, per offrire un numero di opzioni molto più elevato rispetto alle precedenti funzionalità di chiamate di webhook e di posta elettronica. Gli avvisi possono ora attivare chiamate vocali, Funzioni di Azure, App per la logica, SMS e strumenti di Gestione dei servizi IT, ad esempio ServiceNow e i runbook di automazione. Grazie alle funzionalità di monitoraggio e avviso quasi in tempo reale, la nuova piattaforma consente agli utenti di Application Insights di sfruttare la stessa tecnologia alla base del monitoraggio in altre risorse di Azure e potenziare il monitoraggio dei prodotti Microsoft.

La nuova funzionalità unificata di monitoraggio e avviso per Application Insights comprenderà:

- **Metriche della piattaforma Application Insights**, per fornire popolari metriche predefinite del prodotto Application Insights. Per altre informazioni, vedere l'articolo sull'uso di [metriche della piattaforma per Application Insights nel nuovo servizio Monitoraggio di Azure](../application-insights/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics).
- **Test Web e della disponibilità di Application Insights**, che offrono la possibilità di valutare la velocità di risposta e la disponibilità del server o dell'app Web. Per altre informazioni, vedere l'articolo sull'uso di [test di disponibilità e avvisi per Application Insights nel nuovo servizio Monitoraggio di Azure](../application-insights/app-insights-monitor-web-app-availability.md).
- **Metriche personalizzate di Application Insights**, che permettono di definire e generare metriche personalizzate per il monitoraggio e gli avvisi. Per altre informazioni, vedere l'articolo sull'uso di [metriche personalizzate per Application Insights nel nuovo servizio Monitoraggio di Azure](../application-insights/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).
- **Anomalie errori di Application Insights (parte della funzionalità di rilevamento intelligente)**, per l'invio di notifiche automatiche quasi in tempo reale se si verifica un incremento anomalo della frequenza di chiamate di dipendenze o richieste HTTP non riuscite nell'app Web. La funzionalità Anomalie errori di Application Insights (parte della funzionalità di rilevamento intelligente) come parte del nuovo servizio Monitoraggio di Azure sarà disponibile a breve e questo documento verrà aggiornato con i collegamenti all'iterazione successiva, poiché il lancio è previsto nei prossimi mesi.

## <a name="unified-metrics--alerts-for-other-azure-resources"></a>Funzionalità unificata di metriche e avvisi per altre risorse di Azure

Da marzo 2018, è disponibile la nuova generazione di avvisi e monitoraggio multidimensionale per le risorse di Azure. Ora la nuova piattaforma per le metriche e gli avvisi è più veloce e offre funzionalità quasi in tempo reale. È anche importante notare che gli avvisi della nuova piattaforma per le metriche offrono maggiore granularità, in quanto la nuova piattaforma include l'opzione delle dimensioni, grazie a cui è possibile sezionare e filtrare in base a una specifica combinazione di valori, condizione o operazione. Come tutti gli avvisi nel nuovo servizio Monitoraggio di Azure, i nuovi avvisi delle metriche offrono maggiore estendibilità grazie all'uso dei gruppi di azioni, che consentono di estendere le notifiche oltre la posta elettronica o i webhook, con la possibilità di scegliere anche SMS, funzionalità vocali, funzioni di Azure, runbook di automazione e altro ancora.
Le nuove metriche per le risorse di Azure sono disponibili come:

- **Metriche della piattaforma di Monitoraggio di Azure Standard**, che forniscono popolari metriche precompilate di diversi servizi e prodotti di Azure. Per altre informazioni, vedere l'articolo sulle [metriche supportate in Monitoraggio di Azure](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported) e quello sugli [avvisi delle metriche supportati in Monitoraggio di Azure](alert-metric-overview.md#supported-resource-types-for-metric-alerts).
- **Metriche personalizzate di Monitoraggio di Azure**, che forniscono metriche da origini definite dall'utente, tra cui l'agente di Diagnostica di Azure. Per altre informazioni, vedere l'articolo sulle [metriche personalizzate in Monitoraggio di Azure](metrics-custom-overview.md). Usando le metriche personalizzate, è anche possibile pubblicare le metriche raccolte dall'[agente di Diagnostica di Microsoft Azure](metrics-store-custom-guestos-resource-manager-vm.md) e dall'[agente InfluxData Telegraf](metrics-store-custom-linux-telegraf.md).

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Ritiro della piattaforma classica di monitoraggio e avviso

Come indicato in precedenza, la piattaforma classica di monitoraggio e avviso attualmente disponibile nella [sezione Avvisi (versione classica)](monitoring-overview-alerts-classic.md) del portale di Azure verrà ritirata nei prossimi mesi e sostituita dal nuovo sistema.
La piattaforma precedente di monitoraggio e avviso verrà ritirata il 30 giugno 2019. Il ritiro comporta la chiusura delle API correlate, dell'interfaccia del portale di Azure e dei servizi inclusi. In particolare, queste funzionalità verranno deprecate:

- Metriche e avvisi precedenti (versione classica) per le risorse di Azure attualmente disponibili tramite la [sezione Avvisi (versione classica)](monitoring-overview-alerts-classic.md) del portale di Azure, accessibili come risorsa [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)
- Piattaforma e metriche personalizzate precedenti (versione classica) per Azure Application Insights, oltre agli avvisi attualmente disponibili tramite la [sezione Avvisi (versione classica)](monitoring-overview-alerts-classic.md) del portale di Azure e accessibili come risorsa [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)
- Avviso Anomalie errori precedente (versione classica) attualmente disponibile come funzionalità [Rilevamento intelligente in Application Insights](../application-insights/app-insights-proactive-diagnostics.md) nel portale di Azure, con avvisi configurati visualizzati nella [sezione Avvisi (versione classica)](monitoring-overview-alerts-classic.md) del portale di Azure

Tutti i sistemi di monitoraggio e avviso in versione classica, inclusi [API](https://msdn.microsoft.com/library/azure/dn931945.aspx), [PowerShell](insights-alerts-powershell.md), [interfaccia della riga di comando](insights-alerts-command-line-interface.md), pagina del portale di Azure e [modello di risorsa](monitoring-enable-alerts-using-template.md) corrispondenti potranno essere usati fino a giugno 2019. Dopo questa data, il servizio classico di monitoraggio e avvisi verrà ritirato e non sarà più disponibile per l'uso. Le regole di avviso che continueranno a essere presenti nella sezione Avvisi (versione classica) dopo giugno 2019 potranno ancora essere eseguite, ma non saranno disponibili per la modifica.

Per tutti gli avvisi rimanenti nella piattaforma classica di monitoraggio e avviso dopo giugno 2019 verrà eseguita automaticamente la migrazione da parte di Microsoft alla versione equivalente nella nuova piattaforma di Monitoraggio di Azure a luglio 2019. Il processo avverrà in modo fluido, senza tempo di inattività e senza alcuna perdita nella copertura del monitoraggio per i clienti.

A breve verranno resi disponibili strumenti per consentire di eseguire volontariamente la migrazione degli avvisi dalla [sezione Avvisi (versione classica)](monitoring-overview-alerts-classic.md) del portale di Azure ai nuovi avvisi di Azure. Tutte le regole configurate nella sezione Avvisi (versione classica) di cui viene eseguita la migrazione al nuovo servizio Monitoraggio di Azure rimarranno gratuite e non verranno addebitati costi. Le regole di avviso classiche di cui viene eseguita la migrazione non prevederanno inoltre costi per l'invio di notifiche tramite posta elettronica, webhook o app per la logica. L'uso dei nuovi tipi di azioni o notifiche (ad esempio SMS, chiamata vocale, integrazione di Gestione dei servizi IT e così via) potrà comportare un addebito in caso di aggiunta a un avviso nuovo o di cui è stata eseguita la migrazione. Per altre informazioni, vedere [Prezzi di Monitoraggio di Azure](https://azure.microsoft.com/pricing/details/monitor/).

Sono previsti inoltre addebiti per quanto indicato di seguito in base a quanto specificato in [Prezzi di Monitoraggio di Azure](https://azure.microsoft.com/pricing/details/monitor/):

- Tutte le regole di avviso nuove (non sottoposte a migrazione) create oltre le unità gratuite, nella nuova piattaforma di Monitoraggio di Azure
- Tutti i dati inseriti e conservati oltre le unità gratuite incluse in Monitoraggio di Azure
- Tutti i test Web multitest eseguiti da Application Insights
- Tutte le metriche personalizzate archiviate oltre le unità gratuite incluse in Monitoraggio di Azure

Questo articolo verrà continuamente aggiornato con collegamenti e informazioni sulle nuove funzionalità di monitoraggio e avviso di Azure e sulla disponibilità di strumenti di supporto per l'adozione della nuova piattaforma di Monitoraggio di Azure.


## <a name="next-steps"></a>Passaggi successivi

* Leggere le informazioni sul nuovo [servizio unificato di Monitoraggio di Azure](../azure-monitor/overview.md).
* Leggere le informazioni sui nuovi [avvisi di Azure](monitoring-overview-alerts.md).
