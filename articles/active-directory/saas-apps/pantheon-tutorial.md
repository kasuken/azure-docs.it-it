---
title: 'Esercitazione: Integrazione di Azure Active Directory con Pantheon | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Pantheon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 25f8b09f31bd9eecc454444312ea02182a71a77a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448853"
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a>Esercitazione: integrazione di Azure Active Directory con Pantheon

Questa esercitazione descrive come integrare Pantheon con Azure Active Directory (Azure AD).

L'integrazione di Pantheon con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Pantheon
- È possibile abilitare gli utenti per l'accesso automatico a Pantheon (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Pantheon, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Pantheon abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Pantheon dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-pantheon-from-the-gallery"></a>Aggiunta di Pantheon dalla raccolta
Per configurare l'integrazione di Pantheon in Azure AD, è necessario aggiungere Pantheon dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Pantheon dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Pantheon**.

    ![Creazione di un utente test di Azure AD](./media/pantheon-tutorial/tutorial_pantheon_search.png)

1. Nel pannello dei risultati selezionare **Pantheon** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Pantheon con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Pantheon che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Pantheon.

Per stabilire la relazione di collegamento, in Pantheon assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Pantheon, è necessario eseguire le operazioni fondamentali seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente test di Pantheon](#creating-a-pantheon-test-user)**: per avere una controparte di Britta Simon in Pantheon collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Pantheon.

**Per configurare Single Sign-On di Azure AD con Pantheon, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Pantheon** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_samlbase.png)

1. Nella sezione **URL e dominio Pantheon** seguire questa procedura:

    ![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_url.png)

    a. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `urn:auth0:pantheon:<orgname>-SSO`

    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'identificatore e l'URL di risposta effettivi. Per ottenere questi valori, contattare il [team di supporto di Pantheon](https://pantheon.io/docs/getting-support/).

1. L'applicazione Pantheon si aspetta un formato specifico per l'asserzione SAML ed è richiesta l'impostazione del valore dell'attributo UserIdentifier con l'indirizzo e-mail dell'utente. Per impostazione predefinita, Azure AD usa UserPrincipalName per l'attributo UserIdentifier. Per la corretta integrazione è però necessario modificare questo valore in modo che corrisponda all'indirizzo di posta elettronica dell'utente. L'integrazione funzionerà solo dopo aver completato correttamente il mapping.

    ![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_attribute.png)    


1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_certificate.png)

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_general_400.png)

1. Nella sezione **Configurazione di Pantheon** fare clic su **Configura Pantheon** per aprire la finestra **Configura accesso**. Copiare l'**URL servizio Single Sign-On SAML** dalla **sezione Riferimento rapido.**

    ![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_configure.png) 

1. Per configurare l'accesso Single Sign-On sul lato **Pantheon**, è necessario inviare il **Certificato** scaricato e l'**URL del servizio Single Sign-On SAML** al [team di supporto di Pantheon](https://pantheon.io/docs/getting-support/).

     > [!Note]
     > È inoltre necessario fornire le informazioni sui domini di posta elettronica e la data e ora in cui si vuole abilitare la connessione. Per informazioni più dettagliate, vedere [qui](https://pantheon.io/docs/sso-organizations/).

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/pantheon-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/pantheon-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/pantheon-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/pantheon-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-pantheon-test-user"></a>Creazione di un utente di test di Pantheon

In questa sezione viene creato un utente di nome Britta Simon in Pantheon. Seguire i passaggi seguenti per aggiungere l'utente in Pantheon. 

>[!NOTE] 
>Per il corretto funzionamento di Single Sign-On è prima necessario creare l'utente in Pantheon.

1. Accedere a Pantheon con credenziali di amministratore.

1. Passare alla pagina del dashboard **Organization** (Organizzazione).
 
1. Fare clic su **Persone**.

1. Fare clic su **Add User**.

1. Immettere l'indirizzo di posta elettronica dell'utente.

1. Scegliere il ruolo dell'utente.

1. Fare clic su **Add User**.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione, Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a Pantheon.

![Assegna utente][200] 

**Per assegnare Britta Simon a Pantheon, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **Pantheon**.

    ![Configure Single Sign-On](./media/pantheon-tutorial/tutorial_pantheon_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Pantheon nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Pantheon.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/pantheon-tutorial/tutorial_general_01.png
[2]: ./media/pantheon-tutorial/tutorial_general_02.png
[3]: ./media/pantheon-tutorial/tutorial_general_03.png
[4]: ./media/pantheon-tutorial/tutorial_general_04.png

[100]: ./media/pantheon-tutorial/tutorial_general_100.png

[200]: ./media/pantheon-tutorial/tutorial_general_200.png
[201]: ./media/pantheon-tutorial/tutorial_general_201.png
[202]: ./media/pantheon-tutorial/tutorial_general_202.png
[203]: ./media/pantheon-tutorial/tutorial_general_203.png

