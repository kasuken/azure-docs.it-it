---
title: "Guida introduttiva: Inviare una query all'API di ricerca di attività locali Bing mediante Node.js | Microsoft Docs"
titleSuffix: Azure Cognitive Services
description: Iniziare a usare l'API di ricerca di attività locali Bing in Node.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: ca05941a499d6bc3e183919de7b407cfba53038d
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958254"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Guida introduttiva: Inviare una query all'API di ricerca di attività locali Bing mediante Node.js

Usare questa Guida introduttiva per inviare richieste all'API di ricerca di attività locali Bing, che è un servizio cognitivo di Azure. L'applicazione è scritta in Node.js, ma l'API è un servizio Web RESTful compatibile con qualsiasi linguaggio di programmazione in grado di eseguire richieste HTTP e analizzare dati JSON.
 
Questa applicazione di esempio recupera i dati di risposta locali dall'API per la query di ricerca `hotel in Bellevue`.

## <a name="prerequisites"></a>Prerequisiti

* La versione più recente di [Node.js](https://nodejs.org/en/download/).

* [Libreria di richieste JavaScript](https://github.com/request/request)

È necessario avere un [account delle API Servizi cognitivi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con le API Bing. Per questa guida introduttiva è sufficiente la [versione di valutazione gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api). Usare la chiave di accesso fornita dalla versione di prova gratuita.

##<a name="code-scenario"></a>Scenario di codice
Il codice seguente definisce e invia la richiesta. Viene implementato nei passaggi seguenti:

1. Dichiarare le variabili per specificare l'endpoint dall'host e il percorso.
2. Specificare la query e aggiungere il parametro di query. 
3. Creare una funzione del gestore per la risposta.
4. Definire la funzione di ricerca che crea la richiesta e aggiunge l'intestazione Ocp-Apim-Subscription-Key.
5. Eseguire la funzione di ricerca. 

Il codice completo per questa demo è il seguente:

````
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'your-access-key';

let host = 'api.cognitive.microsoft.com/bing';
let path = '/v7.0/localbusinesses/search';

let mkt = 'en-US';
let q = 'hotel in Bellevue';

let params = '?q=' + encodeURI(q) + "&appid=" + accessKey + "&traffictype=Internal_monitor&mkt=" + mkt;

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

````

## <a name="next-steps"></a>Passaggi successivi
- [Local Business Search quickstart](local-quickstart.md) (Guida introduttiva alla ricerca di attività locali)
- [Local Business Search Java quickstart](local-search-java-quickstart.md) (Guida introduttiva alla ricerca di attività locali in Java)
- [Local Business Search Python quickstart](local-search-python-quickstart.md) (Guida introduttiva alla ricerca di attività locali in Python)
