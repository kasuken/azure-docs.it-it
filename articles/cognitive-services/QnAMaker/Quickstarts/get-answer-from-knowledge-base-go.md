---
title: 'Guida introduttiva: Ottenere una risposta dalla knowledge base in REST, Go - QnA Maker'
titlesuffix: Azure Cognitive Services
description: Questa Guida introduttiva Go basata su REST assiste nell'ottenimento di una risposta da una knowledge base a livello di codice.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: 54c2133e35de09cae411f3bcec9f4421e4586f18
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977826"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-go"></a>Ottenere risposte a una domanda da una knowledge base con Go

Questa guida introduttiva illustra come ottenere, a livello di codice, una risposta da una knowledge base QnA Maker pubblicata. QnA Maker estrae automaticamente domande e risposte da contenuto semistrutturato, come le domande frequenti, delle [origini dati](../Concepts/data-sources-supported.md). La domanda, in formato JSON, viene inviata nel corpo della richiesta all'API. 

## <a name="prerequisites"></a>Prerequisiti

* [Go 1.10.1](https://golang.org/dl/)
* [Visual Studio Code](https://code.visualstudio.com/)
* È necessario disporre di un [servizio QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Per recuperare la chiave, selezionare **Chiavi** in **Gestione risorse** nel dashboard di Azure per la risorsa di QnA Maker. 
* Impostazioni pagina di **Pubblicazione**. Se non si dispone di una knowledge base pubblicata, creare una knowledge base vuota, importare una knowledge base nella pagina **Impostazioni** e quindi pubblicarla. È possibile scaricare e usare [questa knowledge base di base](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    Le impostazioni della pagina di pubblicazione includono il valore di route POST, il valore Host e il valore EndpointKey. 

    ![Impostazioni di pubblicazione](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Il codice per questo avvio rapido si trova nel repository [https://github.com/Azure-Samples/cognitive-services-qnamaker-Go](https://github.com/Azure-Samples/cognitive-services-qnamaker-Go/tree/master/documentation-samples/quickstarts/get-answer). 

## <a name="create-a-go-file"></a>Creare un file Go

Aprire VSCode, creare un nuovo file denominato `get-answer.go` e aggiungere la classe seguente:

```Go
package main

func main() {

}
```

## <a name="add-the-required-dependencies"></a>Aggiungere le dipendenze richieste

Sopra la funzione `main`, all'inizio del file `get-answer.go`, aggiungere le dipendenze necessarie al progetto:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=3-9 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Aggiungere le costanti obbligatorie

Nella parte superiore della funzione `main` aggiungere le costanti seguenti per accedere a QnA Maker. Questi dati sono contenuti nella pagina **Pubblica** dopo la pubblicazione della knowledge base. 

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=17-33 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Aggiungere una richiesta POST per inviare domande e ottenere una risposta

Nel codice seguente viene effettuata una richiesta HTTPS all'API di QnA Maker per inviare la domanda alla knowledge base e ricevere la risposta:

[!code-go[Add a POST request to send question to knowledge base](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to send question to knowledge base")]

Il valore dell'intestazione di `Authorization` include la stringa `EndpointKey `. 

## <a name="build-and-run-the-program"></a>Compilare ed eseguire il programma

Compilare ed eseguire il programma dalla riga di comando. Verrà inviata automaticamente la richiesta all'API di QnA Maker, quindi verrà visualizzato l'output nella finestra della console.

1. Compilare il file:

    ```bash
    go build get-answer.go
    ```

1. Eseguire il file:

    ```bash
    ./get-answer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Informazioni di riferimento sull'API REST QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)