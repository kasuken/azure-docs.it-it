---
title: 'Esercitazione: Integrazione di Azure Active Directory con HappyFox | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e HappyFox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 95def1ce278b0a816f19e3cd4e1b47bd3f68f1a9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440290"
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a>Esercitazione: Integrazione di Azure Active Directory con HappyFox

Questa esercitazione illustra come integrare HappyFox con Azure Active Directory (Azure AD).

L'integrazione di HappyFox con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a HappyFox
- È possibile abilitare gli utenti per l'accesso automatico a HappyFox (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con HappyFox, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di HappyFox abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di HappyFox dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-happyfox-from-the-gallery"></a>Aggiunta di HappyFox dalla raccolta
Per configurare l'integrazione di HappyFox in Azure AD, è necessario aggiungere HappyFox dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere HappyFox dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **HappyFox**.

    ![Creazione di un utente test di Azure AD](./media/happyfox-tutorial/tutorial_happyfox_search.png)

1. Nel pannello dei risultati selezionare **HappyFox** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con HappyFox in base a un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di HappyFox che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in HappyFox.

Per stabilire la relazione di collegamento, in HappyFox assegnare il valore di **nome utente** in Azure AD come valore dell'attributo **Username**.

Per configurare e testare l'accesso Single Sign-On di Azure AD con HappyFox, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)**: per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente di test di HappyFox](#creating-a-happyfox-test-user)**: per avere una controparte di Britta Simon in HappyFox collegata alla rappresentazione in Azure AD dell'utente.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione HappyFox.

**Per configurare l'accesso Single Sign-On di Azure AD con HappyFox, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **HappyFox** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/happyfox-tutorial/tutorial_happyfox_samlbase.png)

1. Nella sezione **URL e dominio HappyFox** seguire questa procedura:

    ![Configure Single Sign-On](./media/happyfox-tutorial/tutorial_happyfox_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<subdomain>.happyfox.com/`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://<subdomain>.happyfox.com/saml/metadata/`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, Aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di HappyFox](https://support.happyfox.com/home). 
 
1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/happyfox-tutorial/tutorial_happyfox_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/happyfox-tutorial/tutorial_general_400.png)

1. Nella sezione **Configurazione di HappyFox** fare clic su **Configura HappyFox** per aprire la finestra **Configura accesso**. Copiare **l'URL del servizio Single Sign-On SAML** dalla **sezione Riferimento rapido**.

    ![Configure Single Sign-On](./media/happyfox-tutorial/tutorial_happyfox_configure.png) 

1. Accedere al portale del personale HappyFox, passare a **Manage** (Gestisci), quindi fare clic sulla scheda **Integrations** (Integrazioni).

    ![Configure Single Sign-On](./media/happyfox-tutorial/header.png) 

1. Nella scheda Integrations (Integrazioni) fare clic su **Configure** (Configura) in **SAML Integration** (Integrazione SAML) per aprire Single Sign On Settings (Impostazioni Single Sing-On).

    ![Configure Single Sign-On](./media/happyfox-tutorial/configure.png) 

1. Nella sezione di configurazione di SAML incollare **l'URL del servizio Single Sign-On SAML** copiato dal portale di Azure nella casella di testo **SSO Target URL** (URL di destinazione SSO).

    ![Configure Single Sign-On](./media/happyfox-tutorial/targeturl.png)

1. Aprire in Blocco note il certificato scaricato dal portale di Azure e incollarne il contenuto nella sezione **IdP Signature** (Firma IdP).
 
    ![Configure Single Sign-On](./media/happyfox-tutorial/cert.png)

1. Fare clic sul pulsante **Save Settings** (Salva impostazioni).

    ![Configure Single Sign-On](./media/happyfox-tutorial/savesettings.png)

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/happyfox-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/happyfox-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/happyfox-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/happyfox-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-happyfox-test-user"></a>Creazione di un utente di test di HappyFox

L'applicazione supporta il provisioning dell'utente just-in-time e dopo l'autenticazione gli utenti vengono automaticamente creati nell'applicazione.
        
### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione l'utente Britta Simon viene abilitato per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a HappyFox.

![Assegna utente][200] 

**Per assegnare Britta Simon a HappyFox, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **HappyFox**.

    ![Configure Single Sign-On](./media/happyfox-tutorial/tutorial_happyfox_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

1. Quando si fa clic sul riquadro HappyFox nel pannello di accesso, dovrebbe venire visualizzata la pagina di accesso dell'applicazione HappyFox. Nella pagina di accesso dovrebbe essere presente il pulsante **SAML**.

    ![Plug-in](./media/happyfox-tutorial/saml.png) 

1. Fare clic sul pulsante **SAML** per accedere a HappyFox usando l'account Azure AD.

Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/happyfox-tutorial/tutorial_general_01.png
[2]: ./media/happyfox-tutorial/tutorial_general_02.png
[3]: ./media/happyfox-tutorial/tutorial_general_03.png
[4]: ./media/happyfox-tutorial/tutorial_general_04.png

[100]: ./media/happyfox-tutorial/tutorial_general_100.png

[200]: ./media/happyfox-tutorial/tutorial_general_200.png
[201]: ./media/happyfox-tutorial/tutorial_general_201.png
[202]: ./media/happyfox-tutorial/tutorial_general_202.png
[203]: ./media/happyfox-tutorial/tutorial_general_203.png

