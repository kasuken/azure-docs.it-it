---
title: Annotazioni sulla versione per Application Insights | Documentazione Microsoft
description: Aggiungere indicatori della distribuzione o della build ai grafici di Esplora metriche in Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: mbullwin
ms.openlocfilehash: 4b7b663b95bee12848f4afe2d2f48504a4408266
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515183"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Annotazioni sui grafici delle metriche in Application Insights

Le annotazioni nei grafici di [Esplora metriche](app-insights-metrics-explorer.md) indicano dove è stata distribuita una nuova build o un altro evento significativo e consentono di verificare facilmente se le modifiche hanno avuto effetto sulle prestazioni dell'applicazione. Possono essere create automaticamente dal [sistema di compilazione di Azure DevOps Services](https://docs.microsoft.com/azure/devops/pipelines/tasks/). È anche possibile [creare annotazioni da PowerShell](#create-annotations-from-powershell) per contrassegnare qualsiasi evento.

> [!NOTE]
> Questo articolo descrive l'**esperienza classica delle metriche** che è stata deprecata. Le annotazioni sono attualmente disponibili solo nell'esperienza classica e nelle **[cartelle di lavoro](app-insights-usage-workbooks.md)**. Per altre informazioni sull'esperienza corrente delle metriche, consultare [questo articolo](../monitoring-and-diagnostics/monitoring-metric-charts.md).

![Esempi di annotazioni con correlazione visibile con il tempo di risposta del server](./media/app-insights-annotations/00.png)

## <a name="release-annotations-with-azure-devops-services-build"></a>Annotazioni sulla versione con build di Azure DevOps Services

Le annotazioni sulla versione sono una funzionalità del servizio basato sul cloud Azure Pipelines di Azure DevOps Services. 

### <a name="install-the-annotations-extension-one-time"></a>Installare l'estensione Annotazioni (una volta)
Per creare le annotazioni sulla versione, sarà necessario installare una delle numerose estensioni di Azure DevOps Services disponibili in Visual Studio Marketplace.

1. Accedere al progetto [Azure DevOps Services](https://visualstudio.microsoft.com/vso/).
2. In Visual Studio Marketplace [individuare l'estensione delle annotazioni sulla versione](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) e aggiungerla all'organizzazione di Azure DevOps Services.

![Nell'angolo in alto a destra della pagina Web di Azure DevOps Services aprire il marketplace. Selezionare Azure DevOps Services e quindi sotto Azure Pipelines, scegliere altre informazioni.](./media/app-insights-annotations/10.png)

È sufficiente eseguire questa operazione una sola volta per l'organizzazione di Azure DevOps Services. Le annotazioni sulla versione ora possono essere configurate per qualsiasi progetto nell'organizzazione. 

### <a name="configure-release-annotations"></a>Configurare annotazioni sulla versione

È necessario ottenere una chiave API separata per ogni modello di versione di Azure DevOps Services.

1. Accedere al [portale di Microsoft Azure](https://portal.azure.com) e aprire la risorsa di Application Insights che monitora l'applicazione o [crearne una nuova](app-insights-overview.md), se necessario.
2. Aprire **Accesso all'API**, **ID di Application Insights**.
   
    ![In portal.azure.com, aprire la risorsa di Application Insights e scegliere Settings. Aprire API Access. Copiare l'ID applicazione](./media/app-insights-annotations/20.png)

4. In una finestra del browser separata aprire (o creare) il modello di versione che gestisce le distribuzioni da Azure DevOps Services. 
   
    Aggiungere un'attività e scegliere l'attività di annotazione sulla versione di Application Insights dal menu.
   
    Incollare l' **ID di Application Insights** copiato dal pannello di accesso all'API.
   
    ![In Azure DevOps Services aprire Versione, selezionare una pipeline di versione e scegliere Modifica. Fare clic su Add Task e scegliere l'annotazione sulla versione di Application Insights. Incollare l'Id di Application Insights.](./media/app-insights-annotations/30.png)
4. Impostare il campo **Chiave API** su una variabile `$(ApiKey)`.

5. Nella finestra di Azure creare una nuova chiave API e richiedere una copia.
   
    ![Nel Pannello di accesso all'API nella finestra di Azure, fare clic su Crea chiave API. Inserire un commento, controllare le annotazioni di scrittura e fare clic su Genera chiave. Copiare la nuova chiave.](./media/app-insights-annotations/40.png)

6. Aprire la scheda Configurazione del modello di versione.
   
    Creare una definizione di variabile per `ApiKey`.
   
    Incollare la chiave API per la definizione della variabile della chiave API.
   
    ![Nella finestra Azure DevOps Services selezionare la scheda Configurazione e fare clic su Aggiungi variabile. Impostare il nome su ApiKey, incollare in Valore la chiave appena generata e fare clic sull'icona di blocco.](./media/app-insights-annotations/50.png)
7. Infine fare clic su **Salva** per salvare la pipeline di versione.


## <a name="view-annotations"></a>Visualizzare le annotazioni
Ora, quando si usa il modello di versione per distribuire una nuova versione, verrà inviata un'annotazione ad Application Insights. Le annotazioni verranno visualizzate nei grafici di Esplora metriche.

Fare clic su un marcatore di annotazione per visualizzare i dettagli sulla versione, tra cui richiedente, ramo di controllo del codice sorgente, pipeline di versione, ambiente e così via.

![Fare clic su un marcatore di annotazione della versione.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Creare annotazioni personalizzate da PowerShell
È anche possibile creare le annotazioni da qualsiasi processo desiderato (senza usare Azure DevOps Services). 


1. Creare una copia locale dello [script di Powershell da GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Ottenere l'ID applicazione e creare una chiave API dal pannello Accesso all'API.

3. Chiamare lo script seguente:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

È facile modificare lo script, ad esempio per creare annotazioni per quello precedente.

## <a name="next-steps"></a>Passaggi successivi

* [Creare elementi di lavoro](app-insights-diagnostic-search.md#create-work-item)
* [Automazione con PowerShell](app-insights-powershell.md)