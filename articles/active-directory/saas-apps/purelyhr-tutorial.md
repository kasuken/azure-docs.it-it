---
title: 'Esercitazione: Integrazione di Azure Active Directory con PurelyHR | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e PurelyHR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: c02dc274c5d22c16b2bda6d7896ee64c41d6e2ab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438995"
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a>Esercitazione: Integrazione di Azure Active Directory con PurelyHR

Questa esercitazione descrive come integrare PurelyHR con Azure Active Directory (Azure AD).

L'integrazione di PurelyHR con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a PurelyHR.
- È possibile abilitare gli utenti per l'accesso automatico a PurelyHR (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con PurelyHR, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di PurelyHR abilitata per l'accesso Single Sign-On.

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di PurelyHR dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-purelyhr-from-the-gallery"></a>Aggiunta di PurelyHR dalla raccolta
Per configurare l'integrazione di PurelyHR in Azure AD, è necessario aggiungere PurelyHR dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere PurelyHR dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **PurelyHR**.

    ![Creazione di un utente test di Azure AD](./media/purelyhr-tutorial/tutorial_purelyhr_search.png)

1. Nel pannello dei risultati selezionare **PurelyHR** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con PurelyHR con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di PurelyHR che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in PurelyHR.

Per stabilire la relazione di collegamento, in PurelyHR assegnare il valore di **nome utente** in Azure AD come valore dell'attributo **Username**.

Per configurare e testare l'accesso Single Sign-On di Azure AD con PurelyHR, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente test di PurelyHR](#creating-a-purelyhr-test-user)**: per avere una controparte di Britta Simon in PurelyHR collegata alla relativa rappresentazione in Azure AD dell'utente.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione PurelyHR.

**Per configurare Single Sign-On di Azure AD con PurelyHR, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **PurelyHR** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

1. Nella sezione **URL e dominio PurelyHR**, se si vuole configurare l'applicazione in modalità avviata da **IDP**, seguire questa procedura:

    ![Configure Single Sign-On](./media/purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: `https://<companyID>.purelyhr.com/sso-consume`

1. Selezionare **Mostra impostazioni URL avanzate**, se si desidera configurare l'applicazione in modalità avviata da **SP**:

    ![Configure Single Sign-On](./media/purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    Nella casella di testo **URL di accesso** digitare il valore usando il modello seguente: `https://<companyID>.purelyhr.com/sso-initiate`
     
    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'URL di risposta e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di PurelyHR](http://support.purelyhr.com/). 

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/purelyhr-tutorial/tutorial_general_400.png)
    
1. Nella sezione **Configurazione di PurelyHR** fare clic su **Configura PurelyHR** per aprire la finestra **Configura accesso**. Copiare l'**ID di entità SAML e l'URL del servizio Single Sign-On SAML** dalla sezione **Riferimento rapido.**

    ![Configure Single Sign-On](./media/purelyhr-tutorial/tutorial_purelyhr_configure.png) 

1. Per configurare l'accesso Single Sign-On sul lato **PurelyHR**, accedere al sito aziendale come amministratore.

1. Aprire il **dashboard** dalle opzioni nella barra degli strumenti e fare clic su **SSO Settings** (Impostazioni SSO).

1. Incollare i valori nelle caselle come descritto di seguito.

    ![Configure Single Sign-On](./media/purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)  

    a. Aprire il certificato **Certificate(Bas64)** scaricato dal portale di Azure nel blocco note e copiare il valore del certificato. Incollare il valore copiato nella casella **x509 Certificate** (Certificato x509).

    b. Nella casella **IdP Issuer URL** (URL autorità emittente IdP), incollare l'**ID di entità SAML** copiato dal portale di Azure.

    c. Nella casella **IdP Endpoint URL**(URL Endpoint IdP) incollare l'**URL del servizio Single Sign-On SAML** copiato dal portale di Azure 

    d. Selezionare la casella di controllo **Auto-Create Users** (Creazione automatica utenti) per abilitare il provisioning utenti automatico in PurelyHR.

    e. Fare clic su **Salva** per salvare le impostazioni.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/purelyhr-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/purelyhr-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/purelyhr-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/purelyhr-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-purelyhr-test-user"></a>Creazione di un utente test di PurelyHR

Per consentire agli utenti di Azure AD di accedere a PurelyHR, è necessario eseguirne il provisioning in PurelyHR. In PurelyHR, il provisioning è un'attività automatica e non sono necessari passaggi manuali se è abilitato il provisioning utente automatico.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a PurelyHR.

![Assegna utente][200] 

**Per assegnare Britta Simon a PurelyHR, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **PurelyHR**.

    ![Configure Single Sign-On](./media/purelyhr-tutorial/tutorial_purelyhr_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Absorb LMS nel pannello di accesso, si accede automaticamente all'applicazione Absorb LMS.

Per ulteriori informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/purelyhr-tutorial/tutorial_general_203.png

