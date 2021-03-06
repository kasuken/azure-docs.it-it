---
title: Vendita SaaS tramite Azure - API | Microsoft Docs
description: Viene illustrato come creare un'offerta SaaS tramite le API marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 9ffb67a2d3d07e75df29070ca198bac1661f95cc
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212965"
---
<a name="saas-sell-through-azure---apis"></a>Vendita SaaS tramite Azure - API
==============================

Questo articolo illustra come creare un'offerta SaaS con le API. Le API sono necessarie per garantire le sottoscrizioni all'offerta SaaS se è stato selezionato Vendita tramite Azure. Se si desidera compilare un elenco di SaaS regolare non abilitato per la commercializzazione, consultare [Guida alla pubblicazione tecnica dell'applicazione SaaS](./cloud-partner-portal-saas-offers-tech-publishing-guide.md).

Questo articolo è diviso in due sezioni:

-   Autenticazione da servizio a servizio tra un servizio SaaS e Azure Marketplace
-   Gli endpoint e i metodi dell'API

Le API seguenti sono fornite per semplificare l'integrazione del servizio SaaS con Azure:

-   Risolvi
-   Sottoscrivi
-   Conversione
-   Annullare la sottoscrizione

Il diagramma seguente mostra il flusso di sottoscrizione di un nuovo cliente e quando vengono usate queste API:

![Flusso di API dell'offerta SaaS](media/saas-offer-publish-with-subscription-apis/saas-offer-publish-api-flow.png)

<a name="service-to-service-authentication-between-saas-service-and-azure-marketplace"></a>Autenticazione da servizio a servizio tra un servizio SaaS e Azure Marketplace
----------------------------------------------------------------------------

Azure non impone i vincoli per l'autenticazione che il servizio SaaS espone ai propri utenti finali. Tuttavia, quando il servizio SaaS comunica con le API di Azure Marketplace, l'autenticazione viene eseguita nel contesto di un'applicazione Azure Active Directory (Azure AD).

La sezione seguente descrive come creare un'applicazione Azure AD.

### <a name="register-an-azure-ad-application"></a>Registrare un'applicazione Azure AD

Qualsiasi applicazione che vuole usare le funzionalità di Azure AD deve prima essere registrata in un tenant di Azure AD. Questo processo di registrazione comporta l'assegnazione di dettagli di Azure AD sull'applicazione, ad esempio l'URL in cui si trova, l'URL per inviare risposte dopo che un utente viene autenticato, l'URI che identifica l'app e così via.

Per registrare una nuova applicazione nel portale di Azure, seguire i passaggi seguenti:

1.  Accedere al [portale di Azure](https://portal.azure.com/).
2.  Se l'account consente di accedere a più tenant, fare clic sull'account in alto a destra e impostare la sessione del portale sul tenant di Azure AD desiderato.
3.  Nel riquadro di spostamento a sinistra fare clic sul servizio **Azure Active Directory**, fare clic su **Registrazioni per l'app** e fare clic su **Registrazione nuova applicazione**.

    ![Registrazioni di App SaaS AD](media/saas-offer-publish-with-subscription-apis/saas-offer-app-registration.png)

4.  Nella pagina Crea, immettere le informazioni di registrazione\' dell'applicazione:
    -   **Nome:** Immettere un nome significativo per l'applicazione
    -   **Tipo di applicazione**: 
        - Selezionare **Nativa** per le [applicazioni client](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) che sono installate localmente in un dispositivo. Questa impostazione viene usata per i [client nativi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#native-client) OAuth pubblici.
        - Selezionare **App Web/API** per le [applicazioni client](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) e le [applicazioni della risorsa/API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#resource-server) installate su un server protetto. Questa impostazione viene utilizzata per i [client Web](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#web-client) OAuth riservati e i [client basati su agente utente](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client) pubblici.
        La stessa applicazione può anche esporre sia un'API client che un'API di risorse.
    -   **URL di accesso**: per le applicazioni API/app Web, specificare l'URL di base dell'app. Ad esempio, **http://localhost:31544** potrebbe essere l'URL per un'app Web in esecuzione sul computer locale. Gli utenti possono quindi usare questo URL per accedere a un'applicazione client Web.
    -   **URI di reindirizzamento**: per le applicazioni native, fornire l'URI usato da Azure AD per restituire le risposte dei token. Immettere un valore specifico per l'applicazione, ad esempio **http://MyFirstAADApp**.

        ![Registrazioni per l'App AD SaaS](media/saas-offer-publish-with-subscription-apis/saas-offer-app-registration-2.png) per esempi specifici relativi alle applicazioni Web o native, controllare le impostazioni guidate dell'avvio rapido, disponibili nella sezione Introduzione della [Guida per sviluppatori Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide#get-started).

5.  Al termine, fare clic su **Crea**. Azure AD assegna un ID applicazione univoco all'applicazione e l'utente viene reindirizzato alla pagina di registrazione principale dell'applicazione. A seconda che si tratti di un'applicazione Web o nativa, sono fornite opzioni diverse per l'aggiunta di altre funzionalità all'applicazione.

    **Notare:** per impostazione predefinita, l'applicazione appena registrata viene configurata per consentire solo agli utenti dello stesso tenant di accedere all'applicazione.

<a name="api-methods-and-endpoints"></a>Endpoint e metodi API
-------------------------

Le sezioni seguenti descrivono i metodi dell'API e gli endpoint disponibili per l'abilitazione delle sottoscrizioni per un'offerta SaaS.

### <a name="get-a-token-based-on-the-azure-ad-app"></a>Ottenere un token basato sull'app Azure AD

Metodo HTTP

`GET`

*Request URL (URL richiesta)*

**https://login.microsoftonline.com/*{tenantId}*/oauth2/token**

*Parametro URI*

|  **Nome parametro**  | **Obbligatorio**  | **Descrizione**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| TenantId             | True           | ID tenant dell'applicazione AAD registrata   |
|  |  |  |


*Intestazione della richiesta*

|  **Nome dell'intestazione**  | **Obbligatorio** |  **Descrizione**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  Content-Type     | True          | Tipo di contenuto associato alla richiesta. Il valore predefinito è `application/x-www-form-urlencoded`.  |
|  |  |  |


*Corpo della richiesta*

| **Nome proprietà**   | **Obbligatorio** |  **Descrizione**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | True          | Tipo di concessione. Il valore predefinito è `client_credentials`.                    |
|  Client_id          | True          |  Identificatore del client/app associato all'app di Azure AD.                  |
|  client_secret      | True          |  Password associata all'app di Azure AD.                               |
|  Risorsa           | True          |  Risorsa di destinazione per cui è richiesto il token. Il valore predefinito è `62d94f6c-d599-489b-a797-3e10e42fbe22`. |
|  |  |  |


*Risposta*

|  **Nome**  | **Tipo**       |  **Descrizione**    |
| ---------- | -------------  | ------------------- |
| 200 - OK    | TokenResponse  | La richiesta ha avuto esito positivo   |
|  |  |  |

*TokenResponse*

Token di risposta di esempio:

``` json
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "b3cca048-ed2e-406c-aff2-40cf19fe7bf5",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }               
```

### <a name="marketplace-api-endpoint-and-api-version"></a>Endpoint per l'API Marketplace e versione dell'API

L'endpoint per l'API di Azure Marketplace è `https://marketplaceapi.microsoft.com`.

La versione API corrente è `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Risolvere la sottoscrizione

L'azione POST nel risolvere endpoint consente agli utenti di risolvere un token per un ID risorsa permanente.

*Richiesta*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Nome parametro** |     **Descrizione**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  api-version        |  La versione dell'operazione da usare per questa richiesta.   |
|  |  |


*Intestazioni*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | No            | Valore stringa univoco per tenere traccia della richiesta dal client, preferibilmente un GUID. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta.  |
| x-ms-correlationid | No            | Valore stringa univoco per l'operazione sul client. Mette in correlazione tutti gli eventi dall'operazione del client con gli eventi sul lato server. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta. |
| Tipo di contenuto       | Yes          | `application/json`                                        |
| autorizzazione      | Yes          | Il token di connessione JSON Web token (JWT).                    |
| x-ms-marketplace-token| Yes| Il parametro di query token nell'URL quando l'utente viene reindirizzato al sito Web dell'ISV SaaS da Azure. **Notare:** l'URL decodifica il valore del token dal browser prima dell'uso.|
|  |  |  |
  

*Corpo della risposta*

 ``` json       
    { 
        “id”: “”, 
        “subscriptionName”: “”,
        “offerId”:””, 
         “planId”:””
    }     
```

| **Nome parametro** | **Tipo di dati** | **Descrizione**                       |
|--------------------|---------------|---------------------------------------|
| id                 | string        | ID della sottoscrizione SaaS.          |
| subscriptionName| string| Nome della sottoscrizione SaaS impostata dall'utente in Azure durante la sottoscrizione al servizio SaaS.|
| OfferId            | string        | ID dell'offerta sottoscritta dall'utente. |
| planId             | string        | ID del piano sottoscritto dall'utente.  |
|  |  |  |


*Codici di risposta*

| **Codice di stato HTTP** | **Codice di errore**     | **Descrizione**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token risolti.                                                            |
| 400                  | `BadRequest`         | Mancano le intestazioni necessari oppure la versione dell'API specificata non è valida. Non è stato possibile risolvere il token in quanto non valido o scaduto. |
| 403                  | `Forbidden`          | Il chiamante non è autorizzato a eseguire questa operazione.                                 |
| 429                  | `RequestThrottleId`  | Il servizio sta eseguendo l'elaborazione delle richieste, riprovare più tardi.                                |
| 503                  | `ServiceUnavailable` | Il servizio è temporaneamente non disponibile, riprovare più tardi.                                        |
|  |  |  |


*Intestazioni di risposta*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | ID richiesta ricevuta dal client.                                                                   |
| x-ms-correlationid | Yes          | ID di correlazione, se viene passato dal client, in caso contrario, questo valore è l'ID di correlazione del server.                   |
| x-ms-activityid    | Yes          | Valore stringa univoco per tenere traccia della richiesta dal servizio. Viene usato per eventuali riconciliazioni. |
| Retry-After        | No            | Questo valore viene impostato solo per una risposta 429.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Sottoscrivi

L'endpoint di sottoscrizione consente agli utenti di avviare una sottoscrizione a un servizio SaaS per un determinato piano e attiva la fatturazione nel sistema commerciale.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Nome parametro**  | **Descrizione**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Id univoco della sottoscrizione di saas che si ottiene dopo avere risolto il token tramite Risolvere l'API.                              |
| api-version         | La versione dell'operazione da usare per questa richiesta. |
|  |  |

*Intestazioni*

|  **Chiave dell'intestazione**        | **Obbligatorio** |  **Descrizione**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   No          | Valore stringa univoco per tenere traccia della richiesta dal client, preferibilmente un GUID. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta. |
| x-ms-correlationid     |   No          | Valore stringa univoco per l'operazione sul client. Questo valore mette in correlazione tutti gli eventi dall'operazione del client con gli eventi sul lato server. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta. |
| If-Match/If-None-Match |   No          |   Valore assoluto del validator ETag.                                                          |
| content-type           |   Yes        |    `application/json`                                                                   |
|  autorizzazione         |   Yes        |    Il token di connessione JSON Web token (JWT).                                               |
| x-ms-marketplace-modalità sessione| No  | Flag per abilitare la modalità di esecuzione durante la sottoscrizione di un'offerta SaaS. Se impostato, non verrà addebitato nessun importo di sottoscrizione. Ciò è utile per scenari di test di ISV. Impostarlo su **"dryrun"**|
|  |  |  |

*Corpo*

``` json
  { 
      “planId”:””
   }      
```

| **Nome dell'elemento** | **Tipo di dati** | **Descrizione**                      |
|------------------|---------------|--------------------------------------|
| planId           | Stringa (Obbligatoria)        | Id del piano del servizio SaaS che l'utente sta sottoscrivendo.  |
|  |  |  |

*Codici di risposta*

| **Codice di stato HTTP** | **Codice di errore**     | **Descrizione**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Attivazione della sottoscrizione SaaS ricevuta per un determinato piano.                   |
| 400                  | `BadRequest`         | Mancano le intestazioni necessarie oppure il corpo della richiesta in formato JSON non è valido. |
| 403                  | `Forbidden`          | Il chiamante non è autorizzato a eseguire questa operazione.                   |
| 404                  | `NotFound`           | Sottoscrizione non trovata con l'ID specificato                                  |
| 409                  | `Conflict`           | Nella sottoscrione è in corso un'altra operazione.                     |
| 429                  | `RequestThrottleId`  | Il servizio sta eseguendo l’elaborazione delle richieste, riprovare più tardi.                  |
| 503                  | `ServiceUnavailable` | Il servizio è temporaneamente non disponibile, riprovare più tardi.                          |
|  |  |  |

In caso di risposta 202, seguire lo stato dell'operazione richiesta nell'intestazione "Operation-location". L'autenticazione è la stessa usata per le altre API Marketplace.

*Intestazioni di risposta*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | ID richiesta ricevuta dal client.                                                                   |
| x-ms-correlationid | Yes          | ID di correlazione, se viene passato dal client, in caso contrario, questo valore è l'ID di correlazione del server.                   |
| x-ms-activityid    | Yes          | Valore stringa univoco per tenere traccia della richiesta dal servizio. Questo valore viene usato per eventuali riconciliazioni. |
| Retry-After        | Yes          | Intervallo per cui il client può controllare lo stato.                                                       |
| Operation-Location | Yes          | Collegamento a una risorsa per ottenere lo stato dell'operazione.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Modificare il piano endpoint

L'endpoint di modifica consente all'utente di convertire il piano attualmente sottoscritto in un nuovo piano.

**PATCH**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Nome parametro**  | **Descrizione**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID della sottoscrizione SaaS.                              |
| api-version         | La versione dell'operazione da usare per questa richiesta. |
|  |  |

*Intestazioni*

| **Chiave dell'intestazione**          | **Obbligatorio** | **Descrizione**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | No            | Valore stringa univoco per tenere traccia della richiesta dal client. Consigliare un GUID. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta.   |
| x-ms-correlationid      | No            | Valore stringa univoco per l'operazione sul client. Questo valore mette in correlazione tutti gli eventi dall'operazione del client con gli eventi sul lato server. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta. |
| If-Match /If-None-Match | No            | Valore assoluto del validator ETag.                              |
| content-type            | Yes          | `application/json`                                        |
| autorizzazione           | Yes          | Il token di connessione JSON Web token (JWT).                    |
|  |  |  |


*Corpo*

``` json
                { 
                    “planId”:””
                } 
```


|  **Nome dell'elemento** |  **Tipo di dati**  | **Descrizione**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  Stringa (Obbligatoria)         | Id del piano del servizio SaaS che l'utente sta sottoscrivendo.          |
|  |  |  |

*Codici di risposta*

| **Codice di stato HTTP** | **Codice di errore**     | **Descrizione**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Attivazione della sottoscrizione SaaS ricevuta per un determinato piano.                   |
| 400                  | `BadRequest`         | Mancano le intestazioni necessarie oppure il corpo della richiesta in formato JSON non è valido. |
| 403                  | `Forbidden`          | Il chiamante non è autorizzato a eseguire questa operazione.                   |
| 404                  | `NotFound`           | Sottoscrizione non trovata con l'ID specificato                                  |
| 409                  | `Conflict`           | Nella sottoscrione è in corso un'altra operazione.                     |
| 429                  | `RequestThrottleId`  | Il servizio sta eseguendo l’elaborazione delle richieste, riprovare più tardi.                  |
| 503                  | `ServiceUnavailable` | Il servizio è temporaneamente non disponibile, riprovare più tardi.                          |
|  |  |  |

*Intestazioni di risposta*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | ID richiesta ricevuta dal client.                                                                   |
| x-ms-correlationid | Yes          | ID di correlazione, se viene passato dal client, in caso contrario, questo valore è l'ID di correlazione del server.                   |
| x-ms-activityid    | Yes          | Valore stringa univoco per tenere traccia della richiesta dal servizio. Questo valore viene usato per eventuali riconciliazioni. |
| Retry-After        | Yes          | Intervallo per cui il client può controllare lo stato.                                                       |
| Operation-Location | Yes          | Collegamento a una risorsa per ottenere lo stato dell'operazione.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Eliminare una sottoscrizione

L'azione Eliminare dell'endpoint "sottoscrivere" consente all'utente di eliminare una sottoscrizione con un ID specifico.

*Richiesta*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Nome parametro**  | **Descrizione**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID della sottoscrizione SaaS.                              |
| api-version         | La versione dell'operazione da usare per questa richiesta. |
|  |  |

*Intestazioni*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | No            | Valore stringa univoco per tenere traccia della richiesta dal client. Consigliare un GUID. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta.                                                           |
| x-ms-correlationid | No            | Valore stringa univoco per l'operazione sul client. Questo valore mette in correlazione tutti gli eventi dall'operazione del client con gli eventi sul lato server. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta. |
| autorizzazione      | Yes          | Il token di connessione JSON Web token (JWT).                    |
|  |  |  |
 

*Codici di risposta*

| **Codice di stato HTTP** | **Codice di errore**     | **Descrizione**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Attivazione della sottoscrizione SaaS ricevuta per un determinato piano.                   |
| 400                  | `BadRequest`         | Mancano le intestazioni necessarie oppure il corpo della richiesta in formato JSON non è valido. |
| 403                  | `Forbidden`          | Il chiamante non è autorizzato a eseguire questa operazione.                   |
| 404                  | `NotFound`           | Sottoscrizione non trovata con l'ID specificato                                  |
| 429                  | `RequestThrottleId`  | Il servizio è occupato a elaborare le richieste, riprovare più tardi.                  |
| 503                  | `ServiceUnavailable` | Il servizio è temporaneamente interrotto, riprovare più tardi. Riprovare più tardi.                          |
|  |  |  |

In caso di risposta 202, seguire lo stato dell'operazione richiesta nell'intestazione "Operation-location". L'autenticazione è la stessa usata per le altre API Marketplace.

*Intestazioni di risposta*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | ID richiesta ricevuta dal client.                                                                   |
| x-ms-correlationid | Yes          | ID di correlazione, se viene passato dal client, in caso contrario, questo è l'ID di correlazione del server.                   |
| x-ms-activityid    | Yes          | Valore stringa univoco per tenere traccia della richiesta dal servizio. Viene usato per eventuali riconciliazioni. |
| Retry-After        | Yes          | Intervallo per cui il client può controllare lo stato.                                                       |
| Operation-Location | Yes          | Collegamento a una risorsa per ottenere lo stato dell'operazione.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>Ottenere lo stato dell'operazione

Questo endpoint consente all'utente di tracciare lo stato di un'operazione asincrona attivata (piano di Sottoscrivere/Annullamento della sottoscrizione/Modificare).

*Richiesta*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/*{operationId}*?api-version=2017-04-15**

| **Nome parametro**  | **Descrizione**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | ID univoco per l'operazione attivata.                |
| api-version         | La versione dell'operazione da usare per questa richiesta. |
|  |  |


*Intestazioni*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | No            | Valore stringa univoco per tenere traccia della richiesta dal client. Consigliare un GUID. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta.   |
| x-ms-correlationid | No            | Valore stringa univoco per l'operazione sul client. Questo valore mette in correlazione tutti gli eventi dall'operazione del client con gli eventi sul lato server. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta.  |
| autorizzazione      | Yes          | Il token di connessione JSON Web token (JWT).                    |
|  |  |  | 
  

*Corpo della risposta*

``` json
  { 
      “id”: “”, 
      “status”:””, 
       “resourceLocation”:””, 
      “created”:””, 
      “lastModified”:”” 
  } 
```

| **Nome parametro** | **Tipo di dati** | **Descrizione**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | string        | ID dell'operazione.                                                                      |
| status             | Enum          | Stato dell'operazione, uno dei seguenti: `In Progress`, `Succeeded` o `Failed`.          |
| resourceLocation   | string        | Collegamento alla sottoscrizione creata o modificata. In questo modo, il client ottiene lo stato successivo all'operazione aggiornato. Questo valore non è impostato per le operazioni `Unsubscribe`. |
| created            | Datetime      | Ora della creazione dell'operazione in formato UTC.                                                           |
| LastModified       | Datetime      | Ultimo aggiornamento dell'operazione in formato UTC.                                                      |
|  |  |  |

*Codici di risposta*

| **Codice di stato HTTP** | **Codice di errore**     | **Descrizione**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | È stata risolta la richiesta di recupero e il corpo contiene la risposta.    |
| 400                  | `BadRequest`         | Mancano le intestazioni richieste oppure è la versione dell'API specificata non è valida. |
| 403                  | `Forbidden`          | Il chiamante non è autorizzato a eseguire questa operazione.                      |
| 404                  | `NotFound`           | Sottoscrizione non trovata con l'ID specificato                                     |
| 429                  | `RequestThrottleId`  | Il servizio sta eseguendo l'elaborazione delle richieste, riprovare più tardi.                     |
| 503                  | `ServiceUnavailable` | Il servizio è temporaneamente non disponibile, riprovare più tardi.                             |
|  |  |  |

*Intestazioni di risposta*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | ID richiesta ricevuta dal client.                                                                   |
| x-ms-correlationid | Yes          | ID di correlazione, se viene passato dal client, in caso contrario, questo è l'ID di correlazione del server.                   |
| x-ms-activityid    | Yes          | Valore stringa univoco per tenere traccia della richiesta dal servizio. Viene usato per eventuali riconciliazioni. |
| Retry-After        | Yes          | Intervallo per cui il client può controllare lo stato.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Ottenere una sottoscrizione

L'azione Ottieni nell'endpoint di sottoscrizione consente agli utenti di recuperare una sottoscrizione con un identificatore di risorsa specificato.

*Richiesta*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}*?api-version=2017-04-15**

| **Nome parametro**  | **Descrizione**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | ID della sottoscrizione SaaS.                              |
| api-version         | La versione dell'operazione da usare per questa richiesta. |
|  |  |

*Intestazioni*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | No            | Valore stringa univoco per tenere traccia della richiesta dal client, preferibilmente un GUID. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta.                                                           |
| x-ms-correlationid | No            | Valore stringa univoco per l'operazione sul client. Questo valore mette in correlazione tutti gli eventi dall'operazione del client con gli eventi sul lato server. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta. |
| autorizzazione      | Yes          | Il token di connessione JSON Web token (JWT).                                                                    |
|  |  |  |

*Corpo della risposta*

``` json
  { 
      “id”: “”, 
      “saasSubscriptionName”:””, 
      “offerId”:””, 
       “planId”:””, 
      “saasSubscriptionStatus”:””, 
      “created”:””, 
      “lastModified”: “” 
  }
```
| **Nome parametro**     | **Tipo di dati** | **Descrizione**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | string        | ID di una risorsa di sottoscrizione SaaS in Azure.    |
| offerId                | string        | ID dell'offerta sottoscritta dall'utente.         |
| planId                 | string        | ID del piano sottoscritto dall'utente.          |
| saasSubscriptionName   | string        | Nome della sottoscrizione SaaS.                |
| saasSubscriptionStatus | Enum          | Stato dell'operazione.  Uno dei seguenti:  <br/> - `Subscribed`: La sottoscrizione è attiva.  <br/> - `Pending`: La risorsa viene creata dall'utente ma non viene attivata dall'ISV.   <br/> - `Unsubscribed`: L'utente ha annullato la sottoscrizione.   <br/> - `Suspended`: L'utente ha sospeso la sottoscrizione.   <br/> - `Deactivated`: La sottoscrizione di Azure è sospesa.  |
| created                | Datetime      | Valore timestamp relativo alla creazione della sottoscrizione in formato UTC. |
| LastModified           | Datetime      | Valore timestamp relativo alla modifica della sottoscrizione in formato UTC. |
|  |  |  |

*Codici di risposta*

| **Codice di stato HTTP** | **Codice di errore**     | **Descrizione**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | È stata risolta la richiesta di recupero e il corpo contiene la risposta.    |
| 400                  | `BadRequest`         | Mancano le intestazioni richieste oppure è la versione dell'API specificata non è valida. |
| 403                  | `Forbidden`          | Il chiamante non è autorizzato a eseguire questa operazione.                      |
| 404                  | `NotFound`           | Sottoscrizione non trovata con l'ID specificato                                     |
| 429                  | `RequestThrottleId`  | Il servizio sta eseguendo l’elaborazione delle richieste, riprovare più tardi.                     |
| 503                  | `ServiceUnavailable` | Il servizio è temporaneamente non disponibile, riprovare più tardi.                             |
|  |  |  |

*Intestazioni di risposta*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | ID richiesta ricevuta dal client.                                                                   |
| x-ms-correlationid | Yes          | ID di correlazione, se viene passato dal client, in caso contrario, questo è l'ID di correlazione del server.                   |
| x-ms-activityid    | Yes          | Valore stringa univoco per tenere traccia della richiesta dal servizio. Viene usato per eventuali riconciliazioni. |
| Retry-After        | No            | Intervallo per cui il client può controllare lo stato.                                                       |
| eTag               | Yes          | Collegamento a una risorsa per ottenere lo stato dell'operazione.                                                        |
|  |  |  |


### <a name="get-subscriptions"></a>Ottenere le sottoscrizioni

L'azione Ottieni nell'endpoint di sottoscrizione consente all'utente di recuperare tutte le sottoscrizioni per tutte le offerte dall'ISV.

*Richiesta*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Nome parametro**  | **Descrizione**                                       |
|---------------------|-------------------------------------------------------|
| api-version         | La versione dell'operazione da usare per questa richiesta. |
|  |  |

*Intestazioni*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | No            | Valore stringa univoco per tenere traccia della richiesta dal client. Consigliare un GUID. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta.             |
| x-ms-correlationid | No            | Valore stringa univoco per l'operazione sul client. Questo valore mette in correlazione tutti gli eventi dall'operazione del client con gli eventi sul lato server. Se il valore non viene fornito, ne verrà generato e fornito uno nelle intestazioni della risposta. |
| autorizzazione      | Yes          | Il token di connessione JSON Web token (JWT).                    |
|  |  |  |


*Corpo della risposta*

``` json
  { 
      “id”: “”, 
      “saasSubscriptionName”:””, 
      “offerId”:””, 
       “planId”:””, 
      “saasSubscriptionStatus”:””, 
      “created”:””, 
      “lastModified”: “”
  }
```

| **Nome parametro**     | **Tipo di dati** | **Descrizione**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | string        | ID di una risorsa di sottoscrizione SaaS in Azure.    |
| offerId                | string        | ID dell'offerta sottoscritta dall'utente.         |
| planId                 | string        | ID del piano sottoscritto dall'utente.          |
| saasSubscriptionName   | string        | Nome della sottoscrizione SaaS.                |
| saasSubscriptionStatus | Enum          | Stato dell'operazione.  Uno dei seguenti:  <br/> - `Subscribed`: La sottoscrizione è attiva.  <br/> - `Pending`: La risorsa viene creata dall'utente ma non viene attivata dall'ISV.   <br/> - `Unsubscribed`: L'utente ha annullato la sottoscrizione.   <br/> - `Suspended`: L'utente ha sospeso la sottoscrizione.   <br/> - `Deactivated`: La sottoscrizione di Azure è sospesa.  |
| created                | Datetime      | Valore timestamp relativo alla creazione della sottoscrizione in formato UTC. |
| LastModified           | Datetime      | Valore timestamp relativo alla modifica della sottoscrizione in formato UTC. |
|  |  |  |

*Codici di risposta*

| **Codice di stato HTTP** | **Codice di errore**     | **Descrizione**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | È stata risolta la richiesta di recupero e il corpo contiene la risposta.    |
| 400                  | `BadRequest`         | Mancano le intestazioni richieste oppure è la versione dell'API specificata non è valida. |
| 403                  | `Forbidden`          | Il chiamante non è autorizzato a eseguire questa operazione.                      |
| 404                  | `NotFound`           | Sottoscrizione non trovata con l'ID specificato                                     |
| 429                  | `RequestThrottleId`  | Il servizio è occupato a elaborare le richieste, riprovare più tardi.                     |
| 503                  | `ServiceUnavailable` | Il servizio è temporaneamente interrotto, riprovare più tardi. Riprovare più tardi.                             |
|  |  |  |

*Intestazioni di risposta*

| **Chiave dell'intestazione**     | **Obbligatorio** | **Descrizione**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Yes          | ID richiesta ricevuta dal client.                                                                   |
| x-ms-correlationid | Yes          | ID di correlazione, se viene passato dal client, in caso contrario, questo è l'ID di correlazione del server.                   |
| x-ms-activityid    | Yes          | Valore stringa univoco per tenere traccia della richiesta dal servizio. Viene usato per eventuali riconciliazioni. |
| Retry-After        | No            | Intervallo per cui il client può controllare lo stato.                                                       |
|  |  |  |
