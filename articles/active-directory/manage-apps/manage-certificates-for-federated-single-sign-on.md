---
title: Gestione di certificati di federazione in Azure AD | Documentazione Microsoft
description: Informazioni su come personalizzare la data di scadenza per i certificati di federazione e su come rinnovare i certificati con scadenza imminente.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: barbkess
ms.reviewer: jeedes
ms.openlocfilehash: 0f6e690bc80ae8004fba4faf53c0403b0cb7edd9
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035337"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Gestione di certificati per accesso Single Sign-On federato in Azure Active Directory
Questo articolo tratta domande comuni e informazioni relative ai certificati creati da Azure Active Directory (Azure AD) per stabilire l'accesso Single Sign-On federato (SSO) alle applicazioni SaaS. Queste applicazioni possono essere aggiunte dalla raccolta di app di Azure AD o usando il modello di applicazione non inclusa nella raccolta. Configurare l'applicazione utilizzando l'opzione di SSO federato.

Questo articolo è applicabile solo alle app configurate per l'uso dell'accesso SSO di Microsoft Azure AD tramite la federazione SAML, come illustrato nell'esempio seguente:

![Single Sign-On di Microsoft Azure AD](./media/manage-certificates-for-federated-single-sign-on/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Certificato generato automaticamente per le applicazioni incluse e non incluse nella raccolta
Se si aggiunge una nuova applicazione dalla raccolta e si configura l'accesso basato su SAML, Azure AD genera un certificato dalla validità di tre anni per l'applicazione. È possibile scaricare questo certificato dalla sezione **Certificato di firma SAML**. Per le applicazioni incluse nella raccolta, in questa sezione può apparire un'opzione di download del certificato o dei metadati, in base al requisito dell'applicazione.

![Accesso Single Sign-On di Azure AD](./media/manage-certificates-for-federated-single-sign-on/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Personalizzare la data di scadenza per il certificato di federazione ed eseguire il rollover di un nuovo certificato
Per impostazione predefinita, i certificati sono impostati in modo da scadere dopo tre anni. È possibile scegliere una data di scadenza diversa per il certificato eseguendo i passaggi descritti di seguito.
Le schermate usano Salesforce per le finalità dell'esempio, ma questi passaggi possono essere applicati a qualsiasi app SaaS federata.

1. Nel [portale di Azure](https://aad.portal.azure.com), fare clic su **applicazione Enterprise** nel riquadro sinistro e quindi fare clic su **Nuova applicazione** nella pagina **Panoramica**:

   ![Aprire la configurazione guidata di SSO](./media/manage-certificates-for-federated-single-sign-on/enterprise_application_new_application.png)

2. Cercare l'applicazione inclusa nella raccolta e quindi selezionarla per aggiungerla. Se non si riesce a trovare l'applicazione richiesta, aggiungere l'applicazione usando l'opzione **Applicazione non nella raccolta**. Questa funzionalità è disponibile solo nello SKU di Azure AD Premium (P1 e P2).

    ![Accesso Single Sign-On di Azure AD](./media/manage-certificates-for-federated-single-sign-on/add_gallery_application.png)

3. Fare clic sul collegamento **Single Sign-On** nel riquadro a sinistra e modificare la **modalità di accesso Single Sign-On** in **Accesso basato su SAML**. Questo passaggio genera un certificato valido tre anni per l'applicazione.

4. Per creare un nuovo certificato, fare clic sul collegamento **Crea nuovo certificato** nella sezione **Certificato di firma SAML**.

    ![Genera un nuovo certificato](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

5. Il collegamento **Crea un nuovo certificato** apre il controllo del calendario. È possibile impostare data e ora fino a tre anni dalla data corrente. La data e l'ora selezionate e sono la data e l'ora di scadenza del nuovo certificato. Fare clic su **Save**.

    ![Scaricare e quindi caricare il certificato](./media/manage-certificates-for-federated-single-sign-on/certifcate_date_selection.PNG)

6. Ora il nuovo certificato è disponibile per il download. Fare clic sul collegamento **Certificato** per scaricarlo. Per il momento il certificato non è attivo. Per eseguire il rollover del certificato, selezionare la casella di **attivazione del nuovo certificato** e fare clic su **Salva**. Da questo momento in poi Azure AD si avvia usando il nuovo certificato per firmare la risposta.

7.  Per informazioni su come caricare il certificato in un'applicazione SaaS specifica, fare clic sul collegamento che consente di **visualizzare l'esercitazione sulla configurazione dell'applicazione**.

## <a name="certificate-expiration-notification-email"></a>E-mail di notifica per la scadenza del certificato

Azure AD invierà una notifica di posta elettronica 60, 30 e 7 giorni prima che il certificato SAML scada. Per specificare l'indirizzo di posta elettronica a cui inviare la notifica:

- Nella pagina Single Sign-On dell'applicazione Azure Active Directory andare al campo E-mail di notifica.
- Immettere l'indirizzo di posta elettronica che riceverà il messaggio di notifica per la scadenza del certificato. Per impostazione predefinita, in questo campo viene utilizzato l'indirizzo di posta elettronica dell'amministratore che ha aggiunto l'applicazione.

Si riceverà la notifica tramite posta elettronica da aadnotification@microsoft.com. Per evitare che il messaggio di notifica finisca nella cartella della posta indesiderata, assicurarsi di aggiungere questo indirizzo e-mail ai contatti. 

## <a name="renew-a-certificate-that-will-soon-expire"></a>Rinnovare un certificato con scadenza imminente
I passaggi per il rinnovo illustrati di seguito non dovrebbero idealmente comportare tempi di inattività significativi per gli utenti. Le schermate di questa sezione includono Salesforce come esempio, ma i passaggi possono essere applicati a qualsiasi app SaaS federata.

1. Nella pagina **Single Sign-On** dell'applicazione **Azure Active Directory** generare il nuovo certificato per l'applicazione. Per creare il certificato, fare clic sul collegamento **Crea nuovo certificato** nella sezione **Certificato di firma SAML**.

    ![Genera un nuovo certificato](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

2. Selezionare la data e l'ora di scadenza per il nuovo certificato e fare clic su **Salva**. La selezione di una data che si sovrappone al certificato esistente consente di limitare l'eventuale tempo di inattività causato dalla scadenza del certificato. 

3. Se l'app può eseguire automaticamente il rollover di un certificato, impostare il nuovo certificato su attivo.  Accedere all'app per verificare se funziona correttamente.

4. Se l'app non rileva automaticamente il nuovo certificato, ma può gestire più di un certificato di firma, prima della scadenza di quello precedente, caricare il nuovo certificato nell'app e quindi tornare al portale e impostarlo come certificato attivo. 

5. Se l'app può gestire solo un certificato alla volta, scegliere un intervallo di inattività, scaricare il nuovo certificato, caricarlo nell'applicazione, tornare al portale di Azure e impostare il nuovo certificato come attivo. 
   
6. Per attivare il nuovo certificato in Azure AD selezionare l'opzione **Attiva nuovo certificato** e fare clic sul pulsante **Salva** nella parte superiore della pagina. In questo modo viene eseguito il rollover del certificato sul lato Azure AD. Lo stato del certificato passa da **Nuovo** ad **Attivo**. Da questo momento in poi Azure AD si avvia usando il nuovo certificato per firmare la risposta. 
   
    ![Genera un nuovo certificato](./media/manage-certificates-for-federated-single-sign-on/new_certificate_download.png)

## <a name="related-articles"></a>Articoli correlati
* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](../saas-apps/tutorial-list.md)
* [Gestione di applicazioni con Azure Active Directory](what-is-application-management.md)
* [Accesso alle applicazioni e Single Sign-On con Azure Active Directory](what-is-single-sign-on.md)
* [Risoluzione dei problemi dell'accesso Single Sign-On basato su SAML](../develop/howto-v1-debug-saml-sso-issues.md)
