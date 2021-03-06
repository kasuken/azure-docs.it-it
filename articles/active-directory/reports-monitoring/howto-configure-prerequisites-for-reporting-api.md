---
title: Prerequisiti di accesso all'API di creazione report di Azure AD | Microsoft Docs
description: Informazioni sui prerequisiti di accesso all'API di creazione report di Azure AD
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f72d15707d9f56b9e9b5a5d527d1204007c40afa
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621973"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Prerequisiti di accesso all'API di creazione report di Azure AD

Le [API di creazione report di Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) forniscono l'accesso ai dati dal codice tramite un set di API basate su REST. È possibile chiamare le API da numerosi linguaggi di programmazione e strumenti.

L'API di creazione report usa l'autenticazione [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) per autorizzare l'accesso alle API Web.

Per preparare l'accesso all'API di creazione report, è necessario:

1. [Assegnare ruoli](#assign-roles)
2. [Registrare un'applicazione](#register-an-application)
3. [Concedere le autorizzazioni](#grant-permissions)
4. [Ottenere le impostazioni di configurazione](#gather-configuration-settings)

## <a name="assign-roles"></a>Assegnare ruoli

Per accedere ai dati di creazione dei report tramite l'API, è necessario disporre di uno dei seguenti ruoli assegnati:

- Ruolo con autorizzazioni di lettura per la sicurezza

- Amministratore della sicurezza

- Amministratore globale


## <a name="register-an-application"></a>Registrare un'applicazione

È necessario registrare un'applicazione, anche se si accede all'API di creazione dei report tramite uno script. Questo consente di avere un **ID applicazione** necessario per le chiamate di autorizzazione e consente al codice di ricevere i token.

Per configurare la directory per l'accesso all'API di creazione report di Azure AD, è necessario accedere al [portale di Azure](https://portal.azure.com) con un account di amministratore di Azure che è anche membro del ruolo di directory **Amministratore globale** nel tenant di Azure AD.

> [!IMPORTANT]
> Le applicazioni eseguite con credenziali con privilegi di amministratore possono essere molto potenti, quindi assicurarsi di conservare l'ID e il segreto dell'applicazione in un luogo sicuro.
> 

**Per registrare un'applicazione Azure AD:**

1. Nel [portale di Azure](https://portal.azure.com) selezionare **Azure Active Directory** nel riquadro di spostamento sinistro.
   
    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Nella pagina **Azure Active Directory** selezionare **Registrazioni per l'app**.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Nella pagina **Registrazioni per l'app** selezionare **Registrazione nuova applicazione**.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. Nella pagina **Crea** seguire la procedura seguente:

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. Nella casella di testo **Nome** digitare `Reporting API application`.

    b. Per **Tipo di applicazione** selezionare **App Web/API**.

    c. Nella casella di testo **URL di accesso** digitare `https://localhost`.

    d. Selezionare **Create**. 


## <a name="grant-permissions"></a>Concedere le autorizzazioni 

A seconda delle API a cui si desidera accedere, è necessario concedere all'app le autorizzazioni seguenti:  

| API | Autorizzazione |
| --- | --- |
| Microsoft Azure Active Directory | Leggi i dati della directory |
| Microsoft Graph | Leggere tutti i dati dei log di controllo |


![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/36.png)

Nella sezione seguente viene elencata la procedura per entrambe le API. Se non si desidera accedere a una delle API, è possibile ignorare le procedure relative.

**Per concedere l'autorizzazione dell'applicazione per l'uso delle API:**

1. Selezionare l'applicazione nella pagina **Registrazioni per l'app** e selezionare **impostazioni**. 

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. Nella pagina **Impostazioni** selezionare **Autorizzazioni necessarie**. 

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. Nella pagina **Autorizzazioni necessarie** fare clic su Windows **Azure Active Directory** nell'elenco delle **API**. 

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. Nella pagina **Abilita l'accesso**, selezionare **Lettura dati directory** e deselezionare **Accedi e leggi il profilo di un altro utente**. 

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Nel barra degli strumenti in alto fare clic su **Salva**.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/15.png)

6. Nella pagina **Autorizzazioni necessarie** fare clic su **Aggiungi** sulla barra degli strumenti in alto.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/32.png)

7. Nella pagina **Aggiungi accesso all'API** fare clic su **Selezionare un'API**.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/31.png)

8. Nella pagina **Selezionare un'API** fare clic su **Microsoft Graph** e quindi fare clic su **Seleziona**.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/33.png)

9. Nella pagina **Abilita l'accesso**, selezionare **Leggere tutti i dati del log di controllo**, quindi fare clic su **Seleziona**.  

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/34.png)

10. Nella pagina **Aggiungi accesso all'API** fare clic su **Fatto**.  

11. Nella pagina **Autorizzazioni necessarie**, sulla barra degli strumenti in alto. fare clic su **Concedi autorizzazioni** e quindi su **Sì**.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Ottenere le impostazioni di configurazione 

Questa sezione illustra come ottenere le impostazioni seguenti dalla directory:

- Nome di dominio
- ID client
- Segreto client

Questi valori sono necessari quando si configurano le chiamate all'API di creazione report. 

### <a name="get-your-domain-name"></a>Ottenere il nome di dominio

**Per ottenere il nome di dominio:**

1. Nel [portale di Azure](https://portal.azure.com) selezionare **Azure Active Directory** nel riquadro di spostamento a sinistra.
   
    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Nella pagina **Azure Active Directory** selezionare **Nomi di dominio personalizzati**.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Copiare il nome del dominio dall'elenco dei domini.


### <a name="get-your-applications-client-id"></a>Ottenere l'ID client dell'applicazione

**Per ottenere l'ID client dell'applicazione:**

1. Nel [portale di Azure](https://portal.azure.com) fare clic su **Azure Active Directory** nel riquadro di spostamento sinistro.
   
    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Selezionare l'applicazione nella pagina **Registrazioni per l'app**.

3. Nella pagina dell'applicazione, passare a **ID applicazione** e selezionare **Fare clic per copiare**.

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Ottenere il segreto client dell'applicazione
Per ottenere il segreto client dell'applicazione, è necessario creare una nuova chiave e salvare il relativo valore durante il salvataggio della nuova chiave perché non è più possibile recuperare il valore in un secondo momento.

**Per ottenere il segreto client dell'applicazione:**

1. Nel [portale di Azure](https://portal.azure.com) fare clic su **Azure Active Directory** nel riquadro di spostamento sinistro.
   
    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Selezionare l'applicazione nella pagina **Registrazioni per l'app**.

3. Nella pagina dell'applicazione selezionare **Impostazioni** nella barra degli strumenti nella parte superiore. 

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. Nella pagina **Impostazioni** fare clic su **Chiavi** nella sezione **Accesso all'API**. 

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. Nel pagina **Chiavi** seguire questa procedura:

    ![Registrare l'applicazione](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. Nella casella di testo **Descrizione** digitare `Reporting API`.

    b. Per **Scadenza** selezionare **In 2 years** (In 2 anni).

    c. Fare clic su **Save**.

    d. Copiare il valore della chiave.


## <a name="next-steps"></a>Passaggi successivi

* [Ottenere dati con l'API di creazione report di Azure Active Directory con certificati](tutorial-access-api-with-certificates.md)
* [Informazioni di riferimento sulle API di controllo](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Informazioni di riferimento sulle API di report di attività di accesso](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
