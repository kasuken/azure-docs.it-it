---
title: Eseguire un test in batch sull'app LUIS - Language Understanding
titleSuffix: Azure Cognitive Services
description: Usare test in batch per lavorare continuamente all'applicazione allo scopo di perfezionarla e di migliorarne la comprensione del linguaggio.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: a35135d6d19ba10e1fd7473053be747715202440
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637256"
---
# <a name="batch-testing-in-luis"></a>Test in batch in LUIS

I test in batch convalidano il modello con training [attivo](luis-concept-version.md#active-version) per misurare l'accuratezza delle previsioni. Un test in batch consente di visualizzare l'accuratezza di ogni finalità ed entità del modello con training corrente in un grafico. Esaminare i risultati dei test in batch per intraprendere le azioni appropriate per migliorare l'accuratezza, ad esempio aggiungendo altre espressioni di esempio a una finalità se l'app spesso non riesce a identificare la finalità corretta.

## <a name="group-data-for-batch-test"></a>Raggruppare i dati per il test in batch
È importante che le espressioni usate per i test in batch siano nuove per Language Understanding. Se è presente un set di dati di espressioni, dividere le espressioni in tre gruppi: espressioni aggiunte a una finalità, espressioni ricevute dall'endpoint pubblicato ed espressioni usate per i test in batch di Language Understanding dopo il training. 

## <a name="a-dataset-of-utterances"></a>Set di dati di espressioni
Inviare un file batch di espressioni, noto come *set di dati*, per i test in batch. Il set di dati è un file in formato JSON contenente un massimo di 1.000 espressioni etichettate **non duplicate**. È possibile testare fino a 10 set di dati in un'app. Se è necessario testarne di più, eliminare un set di dati e quindi aggiungerne uno nuovo.

|**Regole**|
|--|
|*Nessuna espressione duplicata|
|Nessun elemento figlio di entità gerarchica|
|1000 espressioni o meno|

*I duplicati sono considerati corrispondenze di stringa esatte, non corrispondenze che vengono prima tokenizzate. 

## <a name="entities-allowed-in-batch-tests"></a>Entità consentite nel test in batch
Le entità includono elementi padre semplici, gerarchici e compositi. Tutte le entità di questi tipi vengono visualizzate nel filtro del test in batch anche se nel file batch non sono presenti entità corrispondente.


<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>
## <a name="batch-file-format"></a>Formato file batch
Il file batch è costituito da espressioni. Ogni espressione deve avere una previsione di finalità e le [entità di Machine Learning](luis-concept-entity-types.md#types-of-entities) che si prevede vengano individuate. 

Di seguito è riportato un esempio di un file batch con la sintassi appropriata:

```JSON
[
  {
    "text": "Are there any janitorial jobs currently open?",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 14,
            "endPos": 23
        }
    ]
  },
  {
    "text": "I would like a fullstack typescript programming with azure job",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 15,
            "endPos": 46
        }
    ]
  },
  {
    "text": "Is there a database position open in Los Colinas?",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 11,
            "endPos": 18
        }
    ]
  },
  {
    "text": "Please find database jobs open today in Seattle",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 12,
            "endPos": 19
        }
    ]
  }
]
```

## <a name="batch-syntax-template"></a>Modello di sintassi di batch

Usare il modello seguente per avviare il file batch:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": 
    [
        {
            "entity": "entity name 1 goes here",
            "startPos": 14,
            "endPos": 23
        },
        {
            "entity": "entity name 2 goes here",
            "startPos": 14,
            "endPos": 23
        }
    ]
  }
]
```

Il file batch usare le proprietà **startPos** e **endPos** per notare l'inizio e la fine di un'entità. I valori sono basati su zero e non devono iniziare o terminare in uno spazio. 

Questo è diverso dai log di query, che usano proprietà startIndex ed endIndex. 


## <a name="common-errors-importing-a-batch"></a>Errori comuni nell'importazione di un batch
Di seguito sono riportati gli errori più comuni. 

> * Più di 1.000 espressioni
> * Un oggetto JSON di espressione che non ha una proprietà di entità
> * Parole etichettata in più entità

## <a name="batch-test-state"></a>Stato del test in batch
Language Understanding tiene traccia dello stato dell'ultimo test di ogni set di dati, con le dimensioni (numero di espressioni nel batch), la data dell'ultima esecuzione e l'ultimo risultato (numero di espressioni previste correttamente).

<a name="sections-of-the-results-chart"></a>
## <a name="batch-test-results"></a>Risultati del test in batch
Il risultato del test in batch è un grafico a dispersione, noto come matrice di errore. Questo grafico è un confronto a quattro delle espressioni nel file e della finalità e delle entità previste dal modello corrente. 

I punti dati nelle sezioni **False Positive** (Falso positivo) e **False Negative** (Falso negativo) indicano errori che devono essere esaminati. Se tutti i punti dati si trovano nelle sezioni **True Positive** (Vero positivo) e **True Negative** (Vero negativo), l'accuratezza dell'app è perfetta per questo set di dati.

![Quattro sezioni del grafico](./media/luis-concept-batch-test/chart-sections.png)

Questo grafico consente di trovare le espressioni che Language Understanding prevede in modo errato in base al training attuale. I risultati vengono visualizzati per ogni area del grafico. Selezionare i singoli punti del grafico per esaminare le informazioni sulle espressioni oppure selezionare il nome dell'area per esaminare i risultati delle espressioni in quell'area.

![Test in batch](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Errori nei risultati
Gli errori nel test in batch indicano finalità che non vengono previste come indicato nel file batch. Gli errori sono indicati nelle due sezioni rosse del grafico. 

La sezione dei falsi positivi indica che un'espressione corrispondeva a una finalità o a un'entità quando non avrebbe dovuto. La sezione dei falsi negativi indica che un'espressione non corrispondeva a una finalità o a un'entità quando avrebbe dovuto. 

## <a name="fixing-batch-errors"></a>Correzione di errori batch
In presenza di errori nei test in batch, è possibile aggiungere più espressioni a una finalità e/o etichettare più espressioni con l'entità per consentire a Language Understanding di distinguere tra le finalità. Se sono state aggiunte espressioni che sono state etichettate e si continuano a riscontrare errori di previsione nei test in batch, è possibile aggiungere un [elenco di frasi](luis-concept-feature.md) con un vocabolario settoriale specifico per consentire a Language Understanding di apprendere più rapidamente. 

## <a name="next-steps"></a>Passaggi successivi

* Informazioni su come [testare un batch](luis-how-to-batch-test.md)
