---
title: Guida alla migrazione dalla versione 1 alla versione 2 dell'interfaccia API
titleSuffix: Azure Cognitive Services
description: Informazioni sulla migrazione al set di API più recente.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 57742d72c8be0dcd999bc8b73f6d598263429c12
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646576"
---
# <a name="api-v2-migration-guide"></a>Guida alla migrazione di API a v2
Le API [endpoint](https://aka.ms/v1-endpoint-api-docs) e [creazione ](https://aka.ms/v1-authoring-api-docs) versione 1 verranno deprecate. Usare questa guida per imparare a migrare le API [endpoint](https://aka.ms/luis-endpoint-apis) e [creazione](https://aka.ms/luis-authoring-apis) alla versione 2. 

## <a name="new-azure-regions"></a>Nuove regioni di Azure
LUIS offre nuove [regioni](https://aka.ms/LUIS-regions) per le API LUIS. LUIS fornisce un sito Web diverso per i gruppi di regioni. L'applicazione deve essere creata nella stessa regione in cui si prevede di eseguire le query. Le applicazioni non eseguono automaticamente la migrazione delle regioni. Esportare l'app da una regione, quindi importarla in un'altra affinché sia disponibile in una nuova regione.

## <a name="authoring-route-changes"></a>Modifiche alla route di creazione
L'API di creazione non è più basata sulla route **prog** ma sulla route **api**.


| version | Route |
|--|--|
|1|/luis/v1.0/**prog**/apps|
|2|/luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Modifiche alla route endpoint
L'API endpoint presenta nuovi parametri querystring e una risposta diversa. Se il flag verbose è true, tutte le finalità, indipendentemente dal punteggio, vengono restituite in una matrice di finalità denominate, oltre a topScoringIntent.

| version | GET route |
|--|--|
|1|/luis/v1/application?ID={appId}&q={q}|
|2|/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]|


v1 endpoint success response:
```JSON
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

v2 endpoint success response:
```JSON
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>Gestione chiavi non più nell'API
Le API chiavi endpoint di sottoscrizione sono deprecate e viene restituito l'errore 410 GONE.

| version | Route |
|--|--|
|1|/luis/v1.0/prog/subscriptions|
|1|/luis/v1.0/prog/subscriptions/{subscriptionKey}|

Le [chiavi endpoint](luis-how-to-azure-subscription.md) di Azure vengono generate nel portale di Azure. Si assegna la chiave a un'app LUIS nella pagina **[Publish](luis-how-to-manage-keys.md)** (Pubblica). Non è necessario conoscere il valore effettivo della chiave. LUIS usa il nome della sottoscrizione per effettuare l'assegnazione. 

## <a name="new-versioning-route"></a>Nuova route controllo versioni
Il modello v2 è ora contenuto in una [versione](luis-how-to-manage-versions.md). Il nome di una versione è dato da 10 caratteri nella route. La versione predefinita è "0.1".

| version | Route |
|--|--|
|1|/luis/v1.0/**prog**/apps/{appId}/entities|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/entities|

## <a name="metadata-renamed"></a>Metadati rinominati
Diverse API restituiscono metadati LUIS presentano nuovi nomi.

| nome route v1 | nome route v2 |
|--|--|
|PersonalAssistantApps |assistants|
|applicationcultures|cultures|
|applicationdomains|domains|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Sample" rinominata in "suggest"
LUIS suggerisce espressioni da [espressioni endpoint](luis-how-to-review-endoint-utt.md) esistenti che potrebbero migliorare il modello. Nella versione precedente il nome era **sample**. Nella nuova versione il nome è **suggest**. Si tratta della **[revisione delle espressioni endpoint](luis-how-to-review-endoint-utt.md)** nel sito Web LUIS.

| version | Route |
|--|--|
|1|/luis/v1.0/**prog**/apps/{appId}/entities/{entityId}/**sample**|
|1|/luis/v1.0/**prog**/apps/{appId}/intents/{intentId}/**sample**|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/entities/{entityId}/**suggest**|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/intents/{intentId}/**suggest**|


## <a name="create-app-from-prebuilt-domains"></a>Creare app da domini predefiniti
I [domini predefiniti](luis-how-to-use-prebuilt-domains.md) rappresentano modelli di dominio preimpostati. I domini predefiniti consentono di sviluppare rapidamente l'applicazione LUIS per domini comuni. Questa API consente di creare una nuova app in base a un dominio predefinito. La risposta è il nuovo appID.

|v2 route|verb|
|--|--|
|/luis/api/v2.0/apps/customprebuiltdomains  |get, post|
|/luis/api/v2.0/apps/customprebuiltdomains/{culture}  |get|

## <a name="importing-1x-app-into-2x"></a>Importazione dell'app 1.x in 2.x
Alcune aree del JSON dell'app 1.x esportata devono essere modificate prima di importarlo in [LUIS][LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Entità predefinite 
Le [entità predefinite](luis-prebuilt-entities.md) sono cambiate. Assicurarsi di usare entità predefinite V2. È incluso l'uso di [datetimeV2](luis-reference-prebuilt-datetimev2.md) anziché datetime. 

### <a name="actions"></a>Azioni
La proprietà actions non è più valida. Deve essere vuota 

### <a name="labeled-utterances"></a>Espressioni etichettate
Nella versione 1 le espressioni etichettate potevano includere spazi all'inizio o alla fine della parola o della frase. Gli spazi sono stati rimossi. 

## <a name="common-reasons-for-http-response-status-codes"></a>Cause comuni per i codici di stato risposta HTTP
Vedere [Codice risposta API LUIS](luis-reference-response-codes.md).

## <a name="next-steps"></a>Passaggi successivi

Usare la documentazione API v2 per aggiornare le chiamate REST alle API [endpoint](https://aka.ms/luis-endpoint-apis) e [creazione](https://aka.ms/luis-authoring-apis) a LUIS. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions