---
title: 'Esercitazione: Integrazione di Azure Active Directory con SkyDesk Email | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e SkyDesk Email.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 058aad72ea8e5741bc632b3c27c032613683ae78
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444083"
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Esercitazione: Integrazione di Azure Active Directory con SkyDesk Email

Questa esercitazione descrive come integrare SkyDesk Email con Azure Active Directory (Azure AD).

L'integrazione di SkyDesk Email con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a SkyDesk Email
- È possibile abilitare gli utenti per l'accesso automatico a SkyDesk Email (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con SkyDesk Email, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di SkyDesk Email abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese: [offerta prova](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di SkyDesk Email dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-skydesk-email-from-the-gallery"></a>Aggiunta di SkyDesk Email dalla raccolta
Per configurare l'integrazione di SkyDesk Email in Azure AD, è necessario aggiungere SkyDesk Email dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere SkyDesk Email dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **SkyDesk Email**.

    ![Creazione di un utente test di Azure AD](./media/skydeskemail-tutorial/tutorial_skydeskemail_search.png)

1. Nel pannello dei risultati selezionare **SkyDesk Email** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con SkyDesk Email con un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di SkyDesk Email che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in SkyDesk Email.

Per stabilire la relazione di collegamento, in SkyDesk Email assegnare il valore di **nome utente** di Azure AD come valore di **Username**.

Per configurare e testare l'accesso Single Sign-On di Azure AD con SkyDesk Email, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente di test di SkyDesk Email](#creating-a-skydesk-email-test-user)**: per avere una controparte di Britta Simon in SkyDesk Email che sia collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione SkyDesk Email.

**Per configurare l'accesso Single Sign-On di Azure AD con SkyDesk Email, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **SkyDesk Email** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

1. Nella sezione **URL e dominio SkyDesk Email** seguire questa procedura:

    ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE] 
    > Poiché non è reale, aggiornarlo con l'URL di accesso effettivo. Per ottenere il valore, contattare il [team di supporto client di SkyDesk Email](https://www.skydesk.sg/support/). 
 
1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_general_400.png)

1. Nella sezione **Configurazione di SkyDesk Email** scegliere **Configura SkyDesk Email** per aprire la finestra di dialogo **Configura accesso**. Copiare **l'URL di disconnessione e l'URL servizio Single Sign-On SAML** dalla **sezione di riferimento rapido**.

    ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

1. Per abilitare SSO in **SkyDesk Email**, seguire questa procedura:

    a. Accedere al proprio account SkyDesk Email come amministratore.

    b. Nel menu in alto fare clic su **Configura** e quindi fare clic selezionare **Org**. 
    
      ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. Nel pannello sinistro selezionare **Domini**.
    
      ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Fare clic su **Aggiungi dominio**.
    
      ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Immettere il nome di dominio e quindi verificarlo.
    
      ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Nel pannello di sinistra fare clic su **SAML Authentication** (Autenticazione SAML).
    
      ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_52.png)

1. Nella pagina della finestra di dialogo **SAML Authentication** seguire questa procedura:
   
      ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >Per usare l'autenticazione basata su SAML, è necessario che il **dominio sia verificato** e l'**URL del portale** sia impostato. È possibile impostare l'URL del portale usando un nome univoco.
    > 
    > 
   
    ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. Nella casella di testo **Login URL** (URL di accesso) incollare il valore dell'**URL del servizio Single Sign-On SAML** copiato dal portale di Azure.
   
    b. Nella casella di testo **URL di disconnessione** incollare il valore dell'**URL di disconnessione** copiato dal portale di Azure.

    c. **Modifica URL password** è facoltativo e può essere lasciato vuoto.

    d. Fare clic su **Get Key From File** (Ottieni chiave da file) per selezionare il certificato scaricato dal portale di Azure e quindi fare clic su **Apri** per caricare il certificato.

    e. In **Algorithm** (Algoritmo) selezionare **RSA**.

    f. Fare clic su **Ok** per salvare le modifiche.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/skydeskemail-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/skydeskemail-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/skydeskemail-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/skydeskemail-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-skydesk-email-test-user"></a>Creazione di un utente di test di SkyDesk Email

In questa sezione si crea un utente di nome Britta Simon in SkyDesk Email.

1. Fare clic su **User Access** (Accesso utente) nel pannello di sinistra di SkyDesk Email e quindi immettere il proprio nome utente. 

    ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Per creare utenti in blocco, è necessario contattare il [team di supporto client di SkyDesk Email](https://www.skydesk.sg/support/).


### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a SkyDesk Email.

![Assegna utente][200] 

**Per assegnare Britta Simon a SkyDesk Email, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **SkyDesk Email**.

    ![Configure Single Sign-On](./media/skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro SkyDesk Email nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione SkyDesk Email.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/skydeskemail-tutorial/tutorial_general_203.png

