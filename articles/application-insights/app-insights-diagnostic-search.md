---
title: Utilizzo della funzionalità Ricerca in Azure Application Insights | Microsoft Docs
description: Ricercare e filtrare elementi di telemetria non elaborata inviata da App Web.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mbullwin
ms.openlocfilehash: bf722756ed04349a06cbcb7b9686e6340e8d4b6c
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958407"
---
# <a name="using-search-in-application-insights"></a>Utilizzo della funzionalità Ricerca in Application Insights
Ricerca è una funzionalità di [Application Insights](app-insights-overview.md) che consente di trovare ed esplorare elementi singoli di telemetria, ad esempio visualizzazioni pagine, eccezioni o richieste Web. È possibile visualizzare le tracce del log e gli eventi codificati.

Per le query più complesse sui dati, utilizzare [Analytics](../log-analytics/query-language/get-started-analytics-portal.md).

## <a name="where-do-you-see-search"></a>Dove si trova Ricerca?

### <a name="in-the-azure-portal"></a>Nel portale di Azure

È possibile aprire la ricerca diagnostica in modo esplicito dal pannello Panoramica di Application Insights dell'applicazione:

![Aprire la ricerca diagnostica](./media/app-insights-diagnostic-search/001.png)

![Screenshot dei grafici di ricerca diagnostica](./media/app-insights-diagnostic-search/002.png)

Il corpo principale della Ricerca diagnostica è un elenco di elementi di telemetria: richieste del server, visualizzazioni pagina, eventi personalizzati che sono stati codificati e così via. Nella parte superiore dell'elenco è disponibile un grafico di riepilogo che mostra il numero di eventi nel tempo.

Fare clic su Aggiorna per ottenere nuovi eventi.

### <a name="in-visual-studio"></a>In Visual Studio

In Visual Studio è inoltre disponibile una finestra Ricerca di Application Insights. È più utile per visualizzare gli eventi di telemetria generati dall'applicazione di cui si esegue il debug. È inoltre possibile visualizzare gli eventi raccolti dall'app pubblicata nel portale di Azure.

Aprire la finestra Cerca in Visual Studio:

![Funzionalità di ricerca di Application Insights aperta in Visual Studio](./media/app-insights-diagnostic-search/32.png)

La finestra di ricerca ha funzionalità simili al portale Web:

![Finestra di ricerca di Application Insights in Visual Studio](./media/app-insights-diagnostic-search/34.png)

La scheda Track Operation (Traccia operazione) è disponibile quando si apre una richiesta o una visualizzazione pagina. Con "operazione" si intende una sequenza di eventi associata a una singola richiesta o visualizzazione pagina. Ad esempio, chiamate di dipendenza, eccezioni, log di traccia ed eventi personalizzati potrebbero far parte di una singola operazione. La scheda Track Operation (Traccia operazione) mostra graficamente l'intervallo e la durata di questi eventi in relazione alla richiesta o alla visualizzazione pagina. 

## <a name="inspect-individual-items"></a>Controllare i singoli elementi

Selezionare qualsiasi elemento di dati di telemetria per visualizzare i campi chiave e gli elementi correlati.

![Screenshot di una richiesta di dipendenza singola](./media/app-insights-diagnostic-search/003.png)

Verrà avviata la visualizzazione dei dettagli delle transazioni end-to-end:

![Screenshot della visualizzazione dei dettagli delle transazioni end-to-end.](./media/app-insights-diagnostic-search/004.png)

## <a name="filter-event-types"></a>I tipi di eventi sono i seguenti:
Aprire il pannello Filtro e scegliere i tipi di evento che si vuole visualizzare. Se, in seguito, si vogliono ripristinare i filtri con cui è stato aperto il pannello, fare clic su Reimposta.

![Scegliere il filtro e selezionare i tipi di telemetria](./media/app-insights-diagnostic-search/02-filter-req.png)

I tipi di eventi sono i seguenti:

* **Traccia**:  - [log di diagnostica](app-insights-asp-net-trace-logs.md) con chiamate TrackTrace, log4Net, NLog e System.Diagnostic.Trace.
* **Richiesta**: richieste HTTP ricevute dall'applicazione server, tra cui pagine, script, immagini, file di stile e dati. Questi eventi vengono usati per creare grafici di panoramica di richieste e risposte.
* **Visualizzazione pagina**: - [i dati di telemetria inviati al client Web](app-insights-javascript.md), usati per creare report di visualizzazioni pagine. 
* **Evento personalizzato**: se sono state inserite chiamate in TrackEvent() per [tenere traccia dell'utilizzo](app-insights-api-custom-events-metrics.md), è possibile cercarle qui.
* **Eccezione**: [eccezioni non rilevate nel server](app-insights-asp-net-exceptions.md) e quelle che si registrano con TrackException().
* **Dipendenza**:  - [chiamate dall'applicazione server](app-insights-asp-net-dependencies.md) ad altri servizi, ad esempio le API REST o i database, e chiamate AJAX dal [codice client](app-insights-javascript.md).
* **Disponibilità**: risultati dei [test di disponibilità](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Filtrare in base ai valori delle proprietà
È possibile filtrare gli eventi in base ai valori delle relative proprietà. Le proprietà disponibili dipendono dai tipi di eventi selezionati. 

Ad esempio, selezionare le richieste con un codice di risposta specifico. 

![Espandere una proprietà e scegliere un valore](./media/app-insights-diagnostic-search/03-response500.png)

La mancata scelta dei valori di una determinata proprietà ha lo stesso effetto della scelta di tutti i valori. Viene disattivata l'applicazione dei filtri per quella proprietà.

### <a name="narrow-your-search"></a>Limitare la ricerca
Si noti che il numero a destra dei valori di filtro mostra quante occorrenze sono incluse nel set filtrato corrente. 

In questo esempio è evidente che la richiesta "Rpt/Employees" produce la maggior parte dei "500" errori:

![Espandere una proprietà e scegliere un valore](./media/app-insights-diagnostic-search/04-failingReq.png)

## <a name="find-events-with-the-same-property"></a>Trovare gli eventi con la stessa proprietà
Trovare tutti gli elementi con lo stesso valore della proprietà:

![Fare clic con il pulsante destro del mouse su una proprietà](./media/app-insights-diagnostic-search/12-samevalue.png)

## <a name="search-the-data"></a>Eseguire ricerche nei dati

> [!NOTE]
> Per scrivere query più complesse, aprire [**Analytics**](../log-analytics/query-language/get-started-analytics-portal.md) nella parte superiore del pannello Ricerca.
> 

È possibile cercare i termini in uno dei valori delle proprietà. Ciò è particolarmente utile se sono stati scritti [eventi personalizzati](app-insights-api-custom-events-metrics.md) con i valori della proprietà. 

È possibile che si voglia impostare un intervallo di tempo, poiché le ricerche di un intervallo più breve sono più veloci. 

![Aprire la ricerca diagnostica](./media/app-insights-diagnostic-search/appinsights-311search.png)

Cercare parole complete, non sottostringhe. Utilizzare le virgolette per racchiudere i caratteri speciali.

| stringa | *non* si trova con | ma si trova con |
| --- | --- | --- |
| ControllerHome.Info |home<br/>controller<br/>fo | controllerhome<br/>about<br/>"homecontroller.info"|
|Stati Uniti|Uni<br/>ti|uniti<br/>stati<br/>uniti AND stati<br/>"stati uniti"

È possibile usare espressioni di ricerca quali le seguenti:

| Query di esempio | Effetto |
| --- | --- |
| `apple` |Individuazione di tutti gli eventi nell'intervallo di tempo i cui campi includono la parola "mela" |
| `apple AND banana` <br/>`apple banana` |Individuazione di eventi che contengono entrambe le parole. Usare "AND" in lettere maiuscole, non "and". <br/>Forma breve. |
| `apple OR banana` |Individuazione degli eventi che contengono una delle parole. Usare "OR", non "or". |
| `apple NOT banana` |Individua eventi che contengono una parola ma non l'altra. |

## <a name="sampling"></a>campionamento
Se l'app genera molti dati di telemetria (e si usa ASP.NET SDK versione 2.0.0-beta3 o successiva), il modulo di campionamento adattivo riduce automaticamente il volume che viene inviato al portale inviando solo una frazione rappresentativa di eventi. Tuttavia, gli eventi che fanno parte della stessa richiesta vengono selezionati o deselezionati come gruppo, per rendere possibile lo spostamento tra eventi correlati. 

[Informazioni sul campionamento](app-insights-sampling.md).

## <a name="create-work-item"></a>Creare un elemento di lavoro
È possibile creare un bug in GitHub o Azure DevOps con i dettagli provenienti da qualsiasi elemento di dati di telemetria. 

![Fare clic su Nuovo elemento di lavoro, modificare i campi e quindi fare clic su OK.](./media/app-insights-diagnostic-search/42.png)

La prima volta che si esegue questa operazione viene chiesto di configurare un collegamento all'organizzazione e al progetto di Azure DevOps.

![Immettere l'URL di Azure DevOps Services e il nome del progetto, quindi fare clic su Autorizza](./media/app-insights-diagnostic-search/41.png)

(È anche possibile configurare il collegamento al pannello Elementi di lavoro).

## <a name="send-more-telemetry-to-application-insights"></a>Inviare altri dati di telemetria ad Application Insights
Oltre la telemetria predefinita inviata da Application Insights SDK, è possibile:

* Acquisire le tracce del log dal framework di registrazione preferito in [.NET](app-insights-asp-net-trace-logs.md) o [Java](app-insights-java-trace-logs.md). Ciò significa che è possibile cercare le tracce del log e metterle in correlazione con le visualizzazioni pagina, le eccezioni e altri eventi. 
* [Scrivere codice](app-insights-api-custom-events-metrics.md) per inviare eventi personalizzati, visualizzazioni pagina ed eccezioni. 

[Informazioni su come inviare log e telemetria personalizzata ad Application Insights](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>Domande e risposte
### <a name="limits"></a>Quanti dati vengono conservati?

Vedere il [Riepilogo dei limiti](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Come è possibile visualizzare dati POST nelle richieste server?
I dati POST non vengono registrati automaticamente, ma è possibile usare [TrackTrace o chiamate di log](app-insights-asp-net-trace-logs.md). Inserire i dati POST nel parametro del messaggio. Non è possibile filtrare in base al messaggio nello stesso modo delle proprietà, ma il limite delle dimensioni è maggiore.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Passaggi successivi
* [Scrivere query complesse in Analytics](../log-analytics/query-language/get-started-analytics-portal.md)
* [Inviare log e dati di telemetria personalizzati ad Application Insights](app-insights-asp-net-trace-logs.md)
* [Configurare i test di disponibilità e velocità di risposta](app-insights-monitor-web-app-availability.md)
* [Risoluzione dei problemi](app-insights-troubleshoot-faq.md)
