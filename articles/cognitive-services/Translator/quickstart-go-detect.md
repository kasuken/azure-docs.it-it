---
title: 'Guida introduttiva: Identificare la lingua del testo, Go - API Traduzione testuale'
titleSuffix: Azure Cognitive Services
description: In questa guida introduttiva si identifica la lingua del testo di origine usando l'API Traduzione testuale con Go.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: erhopf
ms.openlocfilehash: 0275b408e71ec967f6453c94566b4799b3dd4396
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647205"
---
# <a name="quickstart-identify-language-from-text-with-the-translator-text-rest-api-go"></a>Guida introduttiva: Identificare la lingua del testo con l'API REST Traduzione testuale (Go)

In questa guida introduttiva si identifica la lingua del testo di origine usando l'API Traduzione testuale.

## <a name="prerequisites"></a>Prerequisiti

Per eseguire il codice è necessario installare la [distribuzione Go](https://golang.org/doc/install). Il codice di esempio usa solo le librerie **principali**. Non sono quindi presenti dipendenze esterne.

Per usare l'API Traduzione testuale, è necessario avere anche una chiave di sottoscrizione. Per informazioni, vedere [Come registrarsi all'API Traduzione testuale](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Richiesta di identificazione

Il codice seguente identifica la lingua del testo di origine tramite il metodo [Detect](./reference/v3-0-detect.md).

1. Creare un nuovo progetto Go nell'editor di codice preferito.
2. Aggiungere il codice riportato di seguito.
3. Sostituire il valore di `subscriptionKey` con una chiave di accesso valida per la sottoscrizione.
4. Salvare il file con estensione "go".
5. Aprire un prompt dei comandi in un computer in cui è installato Go.
6. Compilare il file, ad esempio: go build quickstart-detect.go
7. Eseguire il file, ad esempio: quickstart-detect

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    const uriBase = "https://api.cognitive.microsofttranslator.com"
    const uriPath = "/detect?api-version=3.0"

    const uri = uriBase + uriPath

    const text = "Salve, mondo!"

    r := strings.NewReader("[{\"Text\" : \"" + text + "\"}]")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.FormatInt(req.ContentLength, 10))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Error on request: %v\n", err)
        return
    }
    defer resp.Body.Close()

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    var f interface{}
    json.Unmarshal(body, &f)

    jsonFormatted, err := json.MarshalIndent(f, "", "  ")
    if err != nil {
        fmt.Printf("Error producing JSON: %v\n", err)
        return
    }
    fmt.Println(string(jsonFormatted))
}
```

## <a name="detect-response"></a>Risposta alla richiesta di identificazione

Viene restituita una risposta con esito positivo in formato JSON, come illustrato nell'esempio seguente:

```json
[
  {
    "alternatives": [
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "pt",
        "score": 1
      },
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "en",
        "score": 1
      }
    ],
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "language": "it",
    "score": 1
  }
]
```

## <a name="next-steps"></a>Passaggi successivi

Esaminare i pacchetti Go per le API Servizi cognitivi da [Azure SDK per Go](https://github.com/Azure/azure-sdk-for-go) su GitHub.

> [!div class="nextstepaction"]
> [Esaminare i pacchetti Go su GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
