---
title: "Esercitazione 1: esaminare le espressioni endpoint con l'apprendimento attivo"
titleSuffix: Azure Cognitive Services
description: Migliorare le stime delle app tramite la verifica o la correzione delle espressioni ricevute tramite l'endpoint HTTP di cui LUIS non è sicuro. Potrebbe essere necessario verificare l'entità o la finalità di alcune espressioni. È consigliabile includere l'esaminazione delle espressioni endpoint come parte normale della manutenzione programmata di LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: d2400c09a7d18faccc94464158419c81962d4a86
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282153"
---
# <a name="tutorial-1-fix-unsure-predictions"></a>Esercitazione 1: correggere le stime dubbie
Questa esercitazione illustra come migliorare le stime delle app tramite la verifica o la correzione delle espressioni ricevute tramite l'endpoint HTTPS di cui LUIS non è sicuro. Potrebbe essere necessario verificare l'entità o la finalità di alcune espressioni. È consigliabile includere l'esaminazione delle espressioni endpoint come parte normale della manutenzione programmata di LUIS. 

Questo processo di revisione è un altro approccio usato da LUIS per conoscere il dominio dell'app. LUIS ha selezionato le espressioni che appaiono nell'elenco di revisione. L'elenco ha le caratteristiche seguenti:

* È specifico per l'app.
* È progettato per migliorare la precisione delle previsioni dell'app. 
* Deve essere esaminato periodicamente. 

La revisione delle espressioni endpoint consente di verificare o correggere la finalità prevista dell'espressione. Consente anche di etichettare entità personalizzate non previste o previste in modo errato. 

**In questa esercitazione si apprenderà come:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Usare l'app di esercitazione esistente
> * Esaminare le espressioni endpoint
> * Aggiornare un elenco di frasi
> * Eseguire il training dell'app
> * Pubblicare l'app
> * Eseguire query sull'endpoint dell'app per ottenere una risposta JSON di Language Understanding

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Usare l'app esistente

Continuare con l'app creata nell'ultima esercitazione denominata **HumanResources**. 

Se non si dispone dell'app HumanResources dell'esercitazione precedente, usare la procedura seguente:

1.  Scaricare e salvare il [file JSON dell'app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-sentiment-HumanResources.json).

2. Importare il file JSON in una nuova app.

3. Nella scheda **Versioni** della sezione **Gestisci**, clonare la versione e denominarla `review`. La clonazione è un ottimo modo per provare le diverse funzionalità di LUIS senza modificare la versione originale. Poiché viene usato come parte della route dell'URL, il nome della versione non può contenere caratteri non validi per un URL.

    Se si usa questa esercitazione come nuova app importata, è inoltre necessario eseguire il training, pubblicare e quindi aggiungere le espressioni all'endpoint tramite uno [script](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) o dall'endpoint in un browser. Le espressioni da aggiungere sono le seguenti:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

    Se sono disponibili tutte le versioni dell'app, tramite la serie di esercitazioni, potrebbe risultare sorprendente il fatto che l'elenco **Review endpoint utterances** (Esamina espressioni endpoint) non subisce alcuna modifica in base alla versione. È disponibile un singolo pool di espressioni da esaminare, indipendentemente dalla versione che si sta modificando attivamente o dalla versione dell'app pubblicata nell'endpoint. 

## <a name="review-endpoint-utterances"></a>Esaminare le espressioni endpoint

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selezionare **Review endpoint utterances** (Esamina espressioni endpoint) dal riquadro di spostamento a sinistra. All'elenco viene applicato un filtro relativo alla finalità **ApplyForJob**. 

    [ ![Screenshot dell'elenco di revisione delle espressioni endpoint nel riquadro di spostamento a sinistra](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png#lightbox)

3. Attivare/disattivare la **Entities view** (Visualizzazione entità) per visualizzare le entità con etichette. 
    
    [ ![Screenshot dell'elenco di revisione delle espressioni endpoint con attivazione/disattivazione della visualizzazione entità evidenziata](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png#lightbox)

    |Espressione|Finalità corretta|Entità mancanti|
    |:--|:--|:--|
    |I'm looking for a job with Natural Language Processing|GetJobInfo|Job - "Natural Language Process"|

    Questa espressione non ha la finalità corretta e ha un punteggio inferiore al 50%. La finalità **ApplyForJob** è associata a 21 espressioni rispetto alle sette espressioni in **GetJobInformation**. Oltre ad allineare correttamente l'espressione endpoint, è necessario aggiungere altre espressioni alla finalità **GetJobInformation**. Questo è un esercizio che deve essere completato autonomamente dall'utente. Ogni finalità, ad eccezione della finalità **None**, deve avere approssimativamente lo stesso numero di espressioni di esempio. La finalità **None** deve avere il 10% delle espressioni totali nell'app. 

4. Per la finalità `I'm looking for a job with Natual Language Processing` selezionare la finalità corretta, **GetJobInformation** nella colonna **Aligned intent** (Finalità allineata). 

    [ ![Screenshot della revisione delle espressioni endpoint per l'allineamento dell'espressione alla finalità](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png#lightbox)

5. Nella stessa espressione l'entità per `Natural Language Processing` è keyPhrase. Dovrebbe trattarsi invece ci un'entità **Job**. Selezionare `Natural Language Processing` e quindi selezionare l'entità **Job** dall'elenco.

    [ ![Screenshot della revisione delle espressioni endpoint per l'applicazione di etichette a un'entità nell'espressione](./media/luis-tutorial-review-endpoint-utterances/label-entity.png)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png#lightbox)

6. Nella stessa riga selezionare il segno di spunta racchiuso in un cerchio nella colonna **Add to aligned intent** (Aggiungi alla finalità allineata). 

    [ ![Screenshot della finalizzazione dell'allineamento delle espressioni nella finalità](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png)](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png#lightbox)

    Questa azione sposta l'espressione da **Review endpoint utterances** (Esamina espressioni endpoint) alla finalità **GetJobInformation**. L'espressione endpoint è ora un'espressione di esempio per tale finalità. 

7. Esaminare le espressioni rimanenti in questa finalità, assegnando etichette alle espressioni e correggendo il valore di **Aligned intent** (Finalità allineata), in caso di errori.

8. Quando tutte le espressioni sono corrette, selezionare la casella di controllo in ogni riga e quindi selezionare **Add selected** (Aggiungi elementi selezionati) per allineare correttamente le espressioni. 

    [ ![Screenshot della finalizzazione delle espressioni rimanenti per la finalità allineata](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png)](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png#lightbox)

9. Queste espressioni non dovrebbero essere più incluse nell'elenco. Se vengono visualizzate altre espressioni, continuare a esaminare l'elenco, correggendo le finalità e assegnando etichette a eventuali entità mancanti, fino allo svuotamento dell'elenco. 

10. Selezionare la finalità successiva nell'elenco di filtri, quindi continuare a correggere le espressioni e ad assegnare etichette alle entità. Occorre ricordare che l'ultimo passaggio per ogni finalità consiste nel selezionare **Add to aligned intent** (Aggiungi alla finalità allineata) nella riga dell'espressione o nel selezionare la casella corrispondente a ogni finalità e quindi selezionare **Add selected** (Aggiungi elementi selezionati) sopra la tabella.

    Continuare finché tutte le finalità e le entità nell'elenco di filtri dispongono di un elenco vuoto. Questa app è molto piccola. Il processo di revisione richiede solo qualche minuto. 

## <a name="update-phrase-list"></a>Aggiornare un elenco di frasi
Mantenere aggiornato l'elenco di frasi inserendo eventuali nomi di lavoro individuati di recente. 

1. Selezionare **Phrase lists** (Elenchi di frasi) dal riquadro di spostamento sinistro.

2. Selezionare l'elenco di frasi **Jobs**.

3. Aggiungere `Natural Language Processing` come valore e quindi selezionare **Save** (Salva). 

## <a name="train"></a>Eseguire il training

Language Understanding non riconosce le modifiche fino a quando non è stato eseguito il training. 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Pubblica

Se questa app è stata importata, è necessario selezionare **Sentiment analysis** (Analisi del sentiment).

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Ottenere finalità ed entità dall'endpoint

Provare a usare un'espressione simile all'espressione corretta. 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Andare alla fine dell'URL nell'indirizzo e immettere `Are there any natural language processing jobs in my department right now?`. L'ultimo parametro querystring è `q`, la **query** dell'espressione. 

  ```JSON
  {
    "query": "are there any natural language processing jobs in my department right now?",
    "topScoringIntent": {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    "intents": [
      {
        "intent": "GetJobInformation",
        "score": 0.9247605
      },
      {
        "intent": "ApplyForJob",
        "score": 0.129989788
      },
      {
        "intent": "FindForm",
        "score": 0.006438211
      },
      {
        "intent": "EmployeeFeedback",
        "score": 0.00408575451
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00194211153
      },
      {
        "intent": "None",
        "score": 0.00166400627
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00118593348
      },
      {
        "intent": "MoveEmployee",
        "score": 0.0007885918
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.0006373631
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0005980781
      },
      {
        "intent": "Utilities.Confirm",
        "score": 3.719905E-05
      }
    ],
    "entities": [
      {
        "entity": "right now",
        "type": "builtin.datetimeV2.datetime",
        "startIndex": 64,
        "endIndex": 72,
        "resolution": {
          "values": [
            {
              "timex": "PRESENT_REF",
              "type": "datetime",
              "value": "2018-07-05 15:23:18"
            }
          ]
        }
      },
      {
        "entity": "natural language processing",
        "type": "Job",
        "startIndex": 14,
        "endIndex": 40,
        "score": 0.9869922
      },
      {
        "entity": "natural language processing jobs",
        "type": "builtin.keyPhrase",
        "startIndex": 14,
        "endIndex": 45
      },
      {
        "entity": "department",
        "type": "builtin.keyPhrase",
        "startIndex": 53,
        "endIndex": 62
      }
    ],
    "sentimentAnalysis": {
      "label": "positive",
      "score": 0.8251864
    }
  }
  }
  ```

  La finalità corretta è stata prevista con un punteggio elevato e l'entità **Job** viene rilevata come `natural language processing`. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>Possibilità di sostituire la revisione tramite l'aggiunta di altre espressioni 
È possibile che ci si domandi se non sia possibile aggiungere altre espressioni di esempio e quale sia la finalità della revisione delle espressioni endpoint. In un'app LUIS concreta le espressioni endpoint vengono generate da utenti con scelte di parole e strutture di frase non ancora usate. Se fossero state usate le stesse scelte di parole e le stesse strutture, la previsione originale avrebbe avuto una percentuale maggiore. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>Ragioni per la presenza della finalità principale nell'elenco di espressioni 
Alcune espressioni endpoint presenteranno un punteggio di stima elevato nell'elenco di revisione. È comunque necessario esaminare e verificare tali espressioni. Sono incluse nell'elenco perché la finalità con punteggio più elevato successiva ha un punteggio troppo vicino a quello della finalità principale. La differenza tra le due finalità principali deve essere del 15%.

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione sono state esaminate espressioni inviate all'endpoint di cui LUIS non era sicuro. Una volta che queste espressioni sono state verificate e spostate nella finalità corretta come espressioni di esempio, LUIS migliorerà l'accuratezza della stima.

> [!div class="nextstepaction"]
> [Informazioni sull'uso dei criteri](luis-tutorial-pattern.md)
