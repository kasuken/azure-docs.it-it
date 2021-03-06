---
title: Endpoint di Ricerca Web | Microsoft Docs
description: Riepilogo dell'endpoint dell'API Ricerca Web.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 72fbe1a0eb4379ad032e649f7299e37a0214adfb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372945"
---
# <a name="web-search-endpoint"></a>Endpoint di Ricerca Web
L'**API Ricerca Web** restituisce pagine Web, notizie, immagini, video ed [entità](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Le entità contengono informazioni di riepilogo su una persona, un luogo o un argomento.
## <a name="endpoint"></a>Endpoint
Per ottenere risultati della ricerca Web tramite l'API Bing, inviare una richiesta `GET` all'endpoint seguente. Le intestazioni e i parametri URL definiscono ulteriori specifiche.

**Endpoint:** restituisce risultati Web attinenti alla query di ricerca dell'utente definita da `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Endpoint: per i dettagli relativi a intestazioni, parametri, codici di mercato, oggetti della risposta, errori e così via, vedere [Web Search API v7 reference](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) (Informazioni di riferimento sull'API Ricerca Web versione 7).

## <a name="response-json"></a>Risposta JSON
I risultati inclusi nella risposta a una richiesta di ricerca Web sono tutti costituiti da oggetti JSON. L'analisi dei risultati richiede procedure che gestiscano gli elementi di ogni tipo. Per alcuni esempi, vedere l'[esercitazione](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) e il [codice sorgente](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source).

## <a name="next-steps"></a>Passaggi successivi
Le API **Bing** supportano azioni di ricerca che restituiscono risultati in base al tipo. Tutti gli endpoint di ricerca restituiscono risultati come oggetti della risposta JSON.  Tutti gli endpoint supportano query che restituiscono una posizione e/o una lingua specifica in base a longitudine, latitudine e raggio di ricerca.

Per informazioni complete sui parametri supportati da ogni endpoint, vedere le pagine di riferimento per ogni tipo.
Per esempi di richieste di base eseguite con l'API Ricerca Web, vedere le [guide introduttive per la ricerca Web](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).