---
title: Dati di Application Insights da LUIS tramite Node.js
titleSuffix: Azure Cognitive Services
description: Creare un bot integrato con un'applicazione LUIS e Application Insights usando Node.js.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 6199e4a681f7f58ea0cf57b575afb2a63d160eee
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321955"
---
# <a name="add-luis-results-to-application-insights"></a>Aggiungere risultati LUIS ad Application Insights
Questa esercitazione aggiunge le informazioni relative a richieste e risposte LUIS all'archivio dei dati di telemetria di [Application Insights](https://azure.microsoft.com/services/application-insights/). Quando si dispone dei dati, è possibile eseguire query su di essi con il linguaggio Kusto o Power BI per analizzare, aggregare e registrare le finalità e le entità delle espressioni in tempo reale. Questa analisi consente di determinare se è necessario aggiungere o modificare le finalità e le entità dell'app LUIS.

Il bot viene compilato con Bot Framework 3.x e il bot per app Web di Azure.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
* Aggiungere la libreria di Application Insights a un bot app Web
* Acquisire i risultati di query LUIS e inviarli ad Application Insights
* Eseguire query su Application Insights per finalità principale, punteggio ed espressione

## <a name="prerequisites"></a>Prerequisiti

* Il bot app Web LUIS dell'**[esercitazione precedente](luis-nodejs-tutorial-build-bot-framework-sample.md)** con Application Insights è attivato. 

> [!Tip]
> Se non si ha già una sottoscrizione, è possibile registrarsi per ottenere un [account gratuito](https://azure.microsoft.com/free/).

Tutto il codice di questa esercitazione è disponibile nel [repository GitHub degli esempi LUIS](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/nodejs) e ogni riga associata a questa esercitazione ha il commento `//APPINSIGHT:`. 

## <a name="web-app-bot-with-luis"></a>Bot app Web con LUIS
Questa esercitazione presuppone che si disponga di codice simile a quello riportato di seguito o che sia stata completata l'[altra esercitazione](luis-nodejs-tutorial-build-bot-framework-sample.md): 

   [!code-javascript[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/nodejs/app.js "Web app bot with LUIS")]

## <a name="add-application-insights-library-to-web-app-bot"></a>Aggiungere la libreria di Application Insights al bot app Web
Attualmente, il servizio Application Insights usato in questo bot app Web raccoglie dati generali di telemetria relativa allo stato per il bot. Non raccoglie informazioni relative a richieste e risposte LUIS necessarie per controllare e correggere le finalità e le entità. 

Per acquisire le richieste e le risposte LUIS, il bot app Web richiede che il pacchetto NPM di **[Application Insights](https://www.npmjs.com/package/applicationinsights)** sia installato e configurato nel file **app.js**. I gestori dei dialoghi delle finalità devono inviare le informazioni relative a richieste e risposte LUIS ad Application Insights. 

1. Nel portale di Azure, nel servizio bot app Web, selezionare **Compila** nella sezione **Bot Management** (Gestione bot). 

    ![Cercare Application Insights](./media/luis-tutorial-appinsights/build.png)

2. Viene visualizzata una nuova scheda del browser con l'Editor del servizio app. Selezionare il nome dell'app nella barra superiore e quindi scegliere **Open Kudu Console** (Apri console Kudu). 

    ![Cercare Application Insights](./media/luis-tutorial-appinsights/kudu-console.png)

3. Nella console immettere il comando seguente per installare Application Insights e i pacchetti Underscore:

    ```
    cd site\wwwroot && npm install applicationinsights && npm install underscore
    ```

    ![Cercare Application Insights](./media/luis-tutorial-appinsights/npm-install.png)

    Attendere l'installazione dei pacchetti:

    ```
    luisbot@1.0.0 D:\home\site\wwwroot
    `-- applicationinsights@1.0.1 
      +-- diagnostic-channel@0.2.0 
      +-- diagnostic-channel-publishers@0.2.1 
      `-- zone.js@0.7.6 
    
    npm WARN luisbot@1.0.0 No repository field.
    luisbot@1.0.0 D:\home\site\wwwroot
    +-- botbuilder-azure@3.0.4
    | `-- azure-storage@1.4.0
    |   `-- underscore@1.4.4 
    `-- underscore@1.8.3 
    ```

    Le operazioni nella scheda del browser relative alla console Kudu sono state completate.

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Acquisire i risultati di query LUIS e inviarli ad Application Insights
1. Nella scheda del browser dell'Editor del servizio app aprire il file **app.js**.

2. Aggiungere le librerie NPM seguenti sotto le righe `requires`:

   [!code-javascript[Add NPM packages to app.js](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=12-16 "Add NPM packages to app.js")]

3. Creare l'oggetto Application Insights e usare l'impostazione dell'applicazione del bot app Web **BotDevInsightsKey**: 

   [!code-javascript[Create the Application Insights object](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=68-80 "Create the Application Insights object")]

4. Aggiungere la funzione **appInsightsLog**:

   [!code-javascript[Add the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=82-109 "Add the appInsightsLog function")]

    L'ultima riga della funzione rappresenta la posizione in cui i dati vengono aggiunti ad Application Insights. Il nome dell'evento è **LUIS-results**, un nome univoco in aggiunta a tutti gli altri dati di telemetria raccolti da questo bot app Web. 

5. Usare la funzione **appInsightsLog**. La funzione viene aggiunta a ogni dialogo delle finalità:

   [!code-javascript[Use the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=117-118 "Use the appInsightsLog function")]

6. Per testare il bot app Web, usare la funzionalità **Test in Web Chat** (Testa nella chat). Non dovrebbe esserci alcuna differenza visibile, perché tutto il lavoro avviene in Application Insights e non nelle risposte del bot.

## <a name="view-luis-entries-in-application-insights"></a>Visualizzare le voci LUIS in Application Insights
Aprire Application Insights per visualizzare le voci LUIS. 

1. Nel portale selezionare **Tutte le risorse** e quindi filtrare il base al nome del bot app Web. Fare clic sulla risorsa con il tipo **Application Insights**. L'icona di Application Insights è una lampadina. 

    ![Cercare Application Insights](./media/luis-tutorial-appinsights/search-for-app-insights.png)



2. Quando la risorsa viene aperta, fare clic sull'icona **Cerca** a forma di lente di ingrandimento nel pannello di destra. Verrà visualizzato un altro pannello a destra. A seconda della quantità di dati di telemetria trovati, la visualizzazione del pannello potrebbe richiedere un attimo. Cercare `LUIS-results` e premere INVIO. L'elenco viene ristretto ai soli risultati delle query LUIS aggiunti in questa esercitazione.

    ![Filtro in base alle dipendenze](./media/luis-tutorial-appinsights/app-insights-filter.png)

3. Selezionare la voce principale. Verrà visualizzata a nuova finestra con dati più dettagliati, tra cui i dati personalizzati per la query LUIS all'estrema destra. I dati includono la finalità principale e il relativo punteggio.

    ![Dettagli delle dipendenze](./media/luis-tutorial-appinsights/app-insights-detail.png)

    Al termine, fare clic su **X** in alto a destra per tornare all'elenco di elementi di dipendenza. 


> [!Tip]
> Se si desidera salvare l'elenco di dipendenze e tornarvi in un secondo momento, fare clic su **...Altro** e quindi su **Salva preferito**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Eseguire query su Application Insights per finalità, punteggio ed espressione
Application Insights consente di eseguire query sui dati con il linguaggio [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics), oltre che esportare i dati in [PowerBI](https://powerbi.microsoft.com). 

1. Fare clic su **Analisi** nella parte superiore dell'elenco di dipendenze, sopra la casella di filtro. 

    ![Pulsante Analytics](./media/luis-tutorial-appinsights/analytics-button.png)

2. Verrà aperta una nuova finestra con una finestra di query nella parte superiore e una finestra di tabella di dati al di sotto. Per chi ha già usato database in precedenza, questa disposizione risulta familiare. La query include tutti gli elementi delle ultime 24 ore che iniziano con il nome `LUIS-results`. La colonna **CustomDimensions** contiene i risultati delle query LUIS sotto forma di coppie nome/valore.

    ![Finestra di query di analisi](./media/luis-tutorial-appinsights/analytics-query-window.png)

3. Per estrarre finalità principale, punteggio ed espressione, aggiungere quanto segue sopra l'ultima riga nella finestra di query:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_intent_intent)
    | extend score = todouble(customDimensions.LUIS_intent_score)
    | extend utterance = tostring(customDimensions.LUIS_text)
    ```

4. Eseguire la query. Scorrere verso destra nella tabella di dati. Saranno disponibili le nuove colonne relative a finalità principale, punteggio ed espressione. Fare clic sulla colonna relativa alla finalità principale per applicare l'ordinamento.

    ![Analisi della finalità principale](./media/luis-tutorial-appinsights/app-insights-top-intent.png)


Leggere altre informazioni sul [linguaggio di query Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) o su come [esportare i dati in PowerBi](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Passaggi successivi

Tra le altre informazioni che è possibile aggiungere ai dati di Application Insights ci sono ID app, ID versione, data dell'ultima modifica del modello, data dell'ultimo training, data dell'ultima pubblicazione. Questi valori possono essere recuperati dall'URL endpoint (ID app e ID versione) o da una chiamata a un'[API di creazione](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) e quindi impostati nelle impostazioni del bot app Web e ricavati da lì.  

Se si sta usando la stessa sottoscrizione di endpoint per più app LUIS, è anche necessario includere l'ID sottoscrizione e una proprietà che indica che si tratta di una chiave condivisa. 

> [!div class="nextstepaction"]
> [Altre informazioni sulle espressioni di esempio](luis-how-to-add-example-utterances.md)
