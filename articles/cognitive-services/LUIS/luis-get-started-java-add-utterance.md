---
title: Guida introduttiva per Java - modificare il modello ed eseguire il training di app LUIS
titleSuffix: Azure Cognitive Services
description: In questa guida introduttiva per Java si aggiungono espressioni di esempio in un'app Home Automation e si esegue il training dell'app. Le espressioni di esempio sono costituite da testo utente discorsivo di cui è stato eseguito il mapping a una finalità. Fornendo espressioni di esempio per le finalità, si insegna all'app LUIS quale tipo di testo specificato dall'utente appartiene a un determinato tipo di finalità.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: bfe4aae25a34799370e52c8677871a62935ab5c2
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419742"
---
# <a name="quickstart-change-model-using-java"></a>Guida introduttiva: Modificare il modello con Java 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [Quickstart prerequisites for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [JDK SE](https://aka.ms/azure-jdks) (Java Development Kit, Standard Edition)
* [Libreria JSON GSON di Google](https://github.com/google/gson).

[!INCLUDE [Quickstart note about code repository](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>File JSON delle espressioni di esempio

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Creare il codice della guida introduttiva

1. Aggiungere le dipendenze Java al file denominato `AddUtterances.java`.

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=23-26 "Java Dependencies")]

2. Creare la classe `AddUtterances`. Questa classe contiene tutti i frammenti di codice che seguono.

    ```Java
    public class AddUtterances {
        // Insert code here
    }
    ```

3. Aggiungere le costanti di Language Understanding alla classe. Copiare il codice seguente e immettere la propria chiave di creazione, insieme all'ID applicazione e all'ID versione.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=33-44 "LUIS-based IDs")]

4. Aggiungere alla classe AddUtterances il metodo da chiamare nell'API LUIS. 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=46-168 "HTTP request to LUIS")]

5. Aggiungere alla classe AddUtterances il metodo per la risposta HTTP inviata dall'API LUIS.

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=170-202 "HTTP response from LUIS")]

6. Aggiungere alla classe AddUtterances il codice per la gestione delle eccezioni. 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=205-243 "Exception Handling")]

7. Aggiungere alla classe AddUtterances la funzione main.

   [!code-java[Add main function](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=245-278 "Add main function")]

## <a name="build-code"></a>Compilare il codice

Compilare AddUtterance con le dipendenze

```CMD
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

## <a name="run-code"></a>Eseguire il codice
Chiamando `AddUtterance` senza argomenti verranno aggiunte le espressioni di Language Understanding all'app senza eseguirne il training.

```CMD
> java -classpath .;gson-2.8.2.jar AddUtterances
```

Questa riga di comando visualizza i risultati della chiamata all'API di aggiunta delle espressioni. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Pulire le risorse
Dopo aver completato la procedura illustrata, rimuovere tutti i file creati in questa guida introduttiva. 

## <a name="next-steps"></a>Passaggi successivi
> [!div class="nextstepaction"] 
> [Compilare un'app di Language Understanding Intelligent Service (LUIS) a livello di codice](luis-tutorial-node-import-utterances-csv.md) 