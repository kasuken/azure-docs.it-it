---
title: 'Guida introduttiva: Ottenere la lunghezza delle frasi, Node.js - API Traduzione testuale'
titleSuffix: Azure Cognitive Services
description: In questa Guida introduttiva si apprenderà come determinare la lunghezza delle frasi (in caratteri) con Node.js e l'API REST Traduzione testuale.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 10/29/2018
ms.author: erhopf
ms.openlocfilehash: 08c01f8c73f8d25b824a97d31f1681d9a7eb302e
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249103"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-with-nodejs"></a>Guida introduttiva: Usare l'API Traduzione testuale per determinare la lunghezza delle frasi con Node.js

In questa Guida introduttiva si apprenderà come determinare la lunghezza delle frasi (in caratteri) con Node.js e l'API REST Traduzione testuale.

Per questa guida introduttiva è necessario avere un [account di Servizi cognitivi di Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con una risorsa Traduzione testuale. Se non si dispone di un account, è possibile usare la [versione di valutazione gratuita](https://azure.microsoft.com/try/cognitive-services/) per ottenere una chiave di sottoscrizione.

## <a name="prerequisites"></a>Prerequisiti

Questa guida introduttiva richiede:

* [Node 8.12.x o versione successiva](https://nodejs.org/en/)
* Una chiave di sottoscrizione di Azure per Traduzione testuale

## <a name="create-a-project-and-import-required-modules"></a>Creare un progetto e importare i moduli necessari

Creare un nuovo progetto tramite l'editor o l'IDE preferito. Copiare quindi questo frammento di codice nel progetto all'interno di un file denominato `sentence-length.js`.

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Se non si è mai usato questi moduli, sarà necessario installarli prima di eseguire il programma. Per installare questi pacchetti, eseguire: `npm install request uuidv4`.

Questi moduli sono necessari per costruire la richiesta HTTP e creare un identificatore univoco per l'intestazione `'X-ClientTraceId'`.

## <a name="set-the-subscription-key"></a>Impostare la chiave di sottoscrizione

Questo codice proverà a leggere la chiave di sottoscrizione di Traduzione testuale dalla variabile di ambiente `TRANSLATOR_TEXT_KEY`. Se non si ha familiarità con le variabili di ambiente, è possibile impostare `subscriptionKey` come stringa e l'istruzione condizionale come commento.

Copiare questo codice nel progetto:

```javascript
/* Checks to see if the subscription key is available
as an environment variable. If you are setting your subscription key as a
string, then comment these lines out.

If you want to set your subscription key as a string, replace the value for
the Ocp-Apim-Subscription-Key header as a string. */
const subscriptionKey = process.env.TRANSLATOR_TEXT_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};
```

## <a name="configure-the-request"></a>Configurare la richiesta

Il metodo `request()`, reso disponibile tramite il modulo di richiesta, consente di passare il metodo HTTP, l'URL, i parametri di richiesta, le intestazioni e il corpo JSON come oggetto `options`. In questo frammento di codice verrà configurata la richiesta:

>[!NOTE]
> Per altre informazioni sugli endpoint, le route e i parametri della richiesta, vedere [API Traduzione testuale 3.0: Dictionary Break Sentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence).

```javascript
let options = {
    method: 'POST',
    baseUrl: 'https://api.cognitive.microsofttranslator.com/',
    url: 'breaksentence',
    qs: {
      'api-version': '3.0',
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'How are you? I am fine. What did you do today?'
    }],
    json: true,
};
```

### <a name="authentication"></a>Authentication

Il modo più semplice per autenticare una richiesta consiste nel passare la chiave di sottoscrizione come intestazione `Ocp-Apim-Subscription-Key`, metodo che viene appunto usato in questo esempio. In alternativa, è possibile sostituire la chiave di sottoscrizione con un token di accesso e passare il token di accesso insieme a un'intestazione `Authorization` per convalidare la richiesta. Per altre informazioni, vedere [Autenticazione](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="make-the-request-and-print-the-response"></a>Eseguire una richiesta e stampare la risposta

A questo punto, verrà creata la richiesta usando il metodo `request()`. Accetta l'oggetto `options` creato nella sezione precedente come primo argomento, quindi stampa la risposta JSON modificata.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> In questo esempio viene definita la richiesta HTTP nell'oggetto `options`. Tuttavia, il modulo di richiesta supporta anche metodi pratici, ad esempio `.post` e `.get`. Per altre informazioni, vedere i [metodi pratici](https://github.com/request/request#convenience-methods).

## <a name="put-it-all-together"></a>Combinare tutti gli elementi

La procedura è completata. È stato realizzato un semplice programma che chiamerà l'API Traduzione testuale e restituirà una risposta JSON. A questo punto, è possibile eseguire il programma:

```console
node sentence-length.js
```

Se si vuole confrontare il proprio codice con quello già disponibile, l'esempio completo è pubblicato su [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS).

## <a name="sample-response"></a>Risposta di esempio

```json
[
    {
        "sentLen": [
            13,
            11,
            22
        ]
    }
]
```

## <a name="clean-up-resources"></a>Pulire le risorse

Se la chiave di sottoscrizione è stata impostata come hardcoded nel programma, assicurarsi di rimuoverla al termine di questa guida introduttiva.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Esaminare gli esempi di codice Node.js su GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS)

## <a name="see-also"></a>Vedere anche 

Oltre al rilevamento della lingua, scopri come usare l'API Traduzione testuale per:

* [Tradurre un testo](quickstart-nodejs-translate.md)
* [Traslitterare testo](quickstart-nodejs-transliterate.md)
* [Identificare la lingua da un input](quickstart-nodejs-detect.md)
* [Ottenere traduzioni alternative](quickstart-nodejs-dictionary.md)
* [Ottenere un elenco di lingue supportate](quickstart-nodejs-languages.md)
