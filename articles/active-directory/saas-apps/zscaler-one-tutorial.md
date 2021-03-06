---
title: 'Esercitazione: Integrazione di Azure Active Directory con Zscaler One | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Zscaler One.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: a7a6abe3a9af98574ffd0a00d21fd0247785f7a5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438291"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a>Esercitazione: Integrazione di Azure Active Directory con Zscaler One

Questa esercitazione descrive come integrare Zscaler One con Azure Active Directory (Azure AD).

L'integrazione di Zscaler One con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Zscaler One
- È possibile abilitare gli utenti per l'accesso automatico a Zscaler One (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Zscaler One, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Zscaler One abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese: [offerta prova](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Zscaler One dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-zscaler-one-from-the-gallery"></a>Aggiunta di Zscaler One dalla raccolta
Per configurare l'integrazione di Zscaler One in Azure AD, è necessario aggiungere Zscaler One dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere Zscaler One dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Zscaler One**.

    ![Creazione di un utente test di Azure AD](./media/zscaler-one-tutorial/tutorial_zscalerone_search.png)

1. Nel pannello dei risultati selezionare **Zscaler One** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/zscaler-one-tutorial/tutorial_zscalerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Zscaler One usando un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Zscaler One corrispondente a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Zscaler One.

Per stabilire la relazione di collegamento, in Zscaler One assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Zscaler One, è necessario completare le procedure di base seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Configurazione delle impostazioni proxy](#configuring-proxy-settings)**: per configurare le impostazioni proxy in Internet Explorer
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente di test di Zscaler One](#creating-a-zscaler-one-test-user)**: per avere una controparte di Britta Simon in Zscaler One collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Zscaler One.

**Per configurare l'accesso Single Sign-On di Azure AD con Zscaler One, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Zscaler One** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/zscaler-one-tutorial/tutorial_zscalerone_samlbase.png)

1. Nella sezione **URL e dominio Zscaler One** seguire questa procedura:

    ![Configure Single Sign-On](./media/zscaler-one-tutorial/tutorial_zscalerone_url.png)

    Nella casella di testo dell'URL di accesso digitare l'URL usato dagli utenti per accedere all'applicazione Zscaler One.

    > [!NOTE] 
    > È necessario aggiornare questo valore con l'URL di accesso effettivo. Per ottenere questi valori contattare il [team di supporto clienti di Zscaler One](https://www.zscaler.com/company/contact).

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/zscaler-one-tutorial/tutorial_zscalerone_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/zscaler-one-tutorial/tutorial_general_400.png)

1. Nella sezione **Configurazione di Zscaler One** fare clic su **Configura Zscaler One** per aprire la finestra **Configura accesso**. Copiare l'**URL servizio Single Sign-On SAML** dalla **sezione Riferimento rapido.**

    ![Configure Single Sign-On](./media/zscaler-one-tutorial/tutorial_zscalerone_configure.png) 

1. In un'altra finestra del Web browser accedere al sito aziendale di Zscaler One come amministratore.

1. Scegliere **Amministrazione**dal menu disponibile nella parte superiore.
   
    ![Amministrazione](./media/zscaler-one-tutorial/ic800206.png "Amministrazione")

1. In **Manage Administrators & Roles** (Gestisci amministratori e ruoli) fare clic su **Manage Users & Authentication** (Gestisci utenti e autenticazione).   
            
    ![Gestire utenti e autenticazione](./media/zscaler-one-tutorial/ic800207.png "Gestire utenti e autenticazione")

1. Nella sezione **Choose Authentication Options for your Organization** seguire questa procedura:   
                
    ![Autenticazione](./media/zscaler-one-tutorial/ic800208.png "Autenticazione")
   
    a. Selezionare **Authenticate using SAML Single Sign-On**.

    b. Fare clic su **Configure SAML Single Sign-On Parameters**.

1. Nella pagina della finestra di dialogo **Configure SAML Single Sign-On Parameters** (Configura parametri accesso Single Sign-On SAML) procedere come descritto di seguito e quindi fare clic su **Done** (Fine)

    ![Single Sign-On](./media/zscaler-one-tutorial/ic800209.png "Single Sign-On")
    
    a. Incollare il valore dell'**URL del servizio Single Sign-On SAML** copiato dal portale di Azure nella casella di testo **URL of the SAML Portal to which users are sent for authentication** (URL del portale di SAML a cui vengono indirizzati gli utenti per l'autenticazione).
    
    b. Nella casella di testo **Attribute containing Login Name** digitare **NameID**.
    
    c. Per caricare il certificato scaricato fare clic su **Zscaler pem**.
    
    d. Selezionare **Enable SAML Auto-Provisioning**.

1. Nella pagina della finestra di dialogo **Configure User Authentication** seguire questa procedura:

    ![Amministrazione](./media/zscaler-one-tutorial/ic800210.png "Amministrazione")
    
    a. Fare clic su **Save**.

    b. Fare clic su **Attiva subito**.

## <a name="configuring-proxy-settings"></a>Configurazione delle impostazioni proxy
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Per configurare le impostazioni proxy in Internet Explorer

1. Avviare **Internet Explorer**.

1. Selezionare **Opzioni Internet** dal menu **Strumenti** per aprire la finestra di dialogo **Opzioni Internet**.   
    
     ![Opzioni Internet](./media/zscaler-one-tutorial/ic769492.png "Opzioni Internet")

1. Fare clic sulla scheda **Connessioni** .   
  
     ![Connessioni](./media/zscaler-one-tutorial/ic769493.png "Connessioni")

1. Fare clic su **Impostazioni LAN** per aprire la finestra di dialogo **Impostazioni LAN**.

1. Nella sezione del server proxy seguire questa procedura:   
   
    ![Server proxy](./media/zscaler-one-tutorial/ic769494.png "Server proxy")

    a. Selezionare **Usa un server proxy per la LAN**.

    b. Nella casella di testo Indirizzo digitare **gateway.zscalerone.net**.

    c. Nella casella di testo Porta digitare **80**.

    d. Selezionare **Ignora server proxy per indirizzi locali**.

    e. Fare clic su **OK** per chiudere la finestra di dialogo **Impostazioni rete locale (LAN)**.

1. Fare clic su **OK** per chiudere la finestra di dialogo **Opzioni Internet**.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/zscaler-one-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/zscaler-one-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/zscaler-one-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/zscaler-one-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-zscaler-one-test-user"></a>Creazione di un utente di test di Zscaler One

Per consentire agli utenti di Azure AD di accedere a Zscaler One, è necessario eseguirne il provisioning in Zscaler One. Nel caso di Zscaler One, il provisioning è un'attività manuale.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Per configurare il provisioning utente, eseguire la procedura seguente:

1. Accedere al tenant **Zscaler One** .

1. Fare clic su **Administration**.   
   
    ![Amministrazione](./media/zscaler-one-tutorial/ic781035.png "Amministrazione")

1. Fare clic su **User Management**.   
        
     ![Aggiungi](./media/zscaler-one-tutorial/ic781036.png "Aggiungi")

1. Nella scheda **Utenti** fare clic su **Aggiungi**.
      
    ![Aggiungi](./media/zscaler-one-tutorial/ic781037.png "Aggiungi")

1. Nella sezione Add User seguire questa procedura:
        
    ![Aggiungere un utente](./media/zscaler-one-tutorial/ic781038.png "Aggiungere un utente")
   
    a. Digitare **ID utente**, **nome visualizzato per l'utente**, **password**, **conferma della password** e quindi selezionare **gruppi** e **reparto** per un account Azure AD valido di cui si vuole eseguire il provisioning.

    b. Fare clic su **Save**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Zscaler One per eseguire il provisioning degli account utente Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Zscaler One.

![Assegna utente][200] 

**Per assegnare Britta Simon a Zscaler One, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **Zscaler One**.

    ![Configure Single Sign-On](./media/zscaler-one-tutorial/tutorial_zscalerone_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Zscaler One nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Zscaler One.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-one-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-one-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-one-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-one-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-one-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-one-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-one-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-one-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-one-tutorial/tutorial_general_203.png

