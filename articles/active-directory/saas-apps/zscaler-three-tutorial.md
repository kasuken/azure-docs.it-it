---
title: 'Esercitazione: Integrazione di Azure Active Directory con Zscaler Three | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Zscaler Three.
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
ms.date: 10/25/2018
ms.author: jeedes
ms.openlocfilehash: b148967af0882993d8ab113bdf0fd3ad3835296f
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092611"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Esercitazione: Integrazione di Azure Active Directory con Zscaler Three

Questa esercitazione descrive come integrare Zscaler Three con Azure Active Directory (Azure AD).

L'integrazione di Zscaler Three con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Zscaler Three
- È possibile abilitare gli utenti per l'accesso automatico a Zscaler Three (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Zscaler Three, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Zscaler Three abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese: [offerta prova](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.
Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Zscaler Three dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-zscaler-three-from-the-gallery"></a>Aggiunta di Zscaler Three dalla raccolta

Per configurare l'integrazione di Zscaler Three in Azure AD, è necessario aggiungere Zscaler Three dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere Zscaler Three dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Active Directory][1]

2. Passare ad **Applicazioni aziendali** e quindi selezionare l'opzione **Tutte le applicazioni**.

    ![APPLICAZIONI][2]

3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo.

    ![APPLICAZIONI][3]

4. Nel pannello dei risultati selezionare **Zscaler Three** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente di test di Azure AD](./media/zscaler-three-tutorial/tutorial_zscalerthree_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Zscaler Three usando un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Zscaler Three corrispondente a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Zscaler Three.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Zscaler Three, è necessario completare le procedure di base seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Configurazione delle impostazioni proxy](#configuring-proxy-settings)**: per configurare le impostazioni proxy in Internet Explorer
3. **[Creazione di un utente di test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Creazione di un utente di test di Zscaler Three](#creating-a-zscaler-three-test-user)**: per avere una controparte di Britta Simon in Zscaler Three collegata alla rappresentazione dell'utente in Azure AD.
5. **[Assegnazione dell'utente di test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
6. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Zscaler Three.

**Per configurare l'accesso Single Sign-On di Azure AD con Zscaler Three, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Zscaler Three** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

2. Nella finestra di dialogo **Selezionare un metodo di accesso Single Sign-On** selezionare la modalità **SAML/WS-Fed** per abilitare il Single Sign-On.

    ![Configure Single Sign-On](./media/zscaler-three-tutorial/tutorial_general_301.png)

3. Se è necessario passare alla modalità **SAML** da qualsiasi altra modalità, fare clic su **Modifica modalità Single Sign-On** nella parte superiore della schermata.

    ![Configure Single Sign-On](./media/zscaler-three-tutorial/tutorial_general_300.png)

4. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona **Modifica** per aprire la finestra di dialogo **Configurazione SAML di base**.

    ![Configure Single Sign-On](./media/zscaler-three-tutorial/tutorial_general_302.png)

5. Nella sezione **Configurazione SAML di base** seguire questa procedura:

    ![Configure Single Sign-On](./media/zscaler-three-tutorial/tutorial_zscalerthree_url.png)

    Nella casella di testo URL di accesso inserire l'URL: `https://login.zscalerthree.net/sfc_sso`

6. Nella sezione **Certificato di firma SAML** fare clic su **Scarica** per scaricare il **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/zscaler-three-tutorial/tutorial_zscalerthree_certificate.png)

8. Nella sezione **Configura Zscaler Three** copiare l'**URL di accesso**.

    ![Configure Single Sign-On](./media/zscaler-three-tutorial/tutorial_zscalerthree_configure.png)

9. In un'altra finestra del Web browser accedere al sito aziendale di Zscaler Three come amministratore.

10. Scegliere **Amministrazione**dal menu disponibile nella parte superiore.

    ![Amministrazione](./media/zscaler-three-tutorial/ic800206.png "Amministrazione")

9. In **Manage Administrators & Roles** (Gestisci amministratori e ruoli) fare clic su **Manage Users & Authentication** (Gestisci utenti e autenticazione).

    ![Gestire utenti e autenticazione](./media/zscaler-three-tutorial/ic800207.png "Gestire utenti e autenticazione")

10. Nella sezione **Choose Authentication Options for your Organization** seguire questa procedura:

    ![Autenticazione](./media/zscaler-three-tutorial/ic800208.png "Autenticazione")

    a. Selezionare **Authenticate using SAML Single Sign-On**.

    b. Fare clic su **Configure SAML Single Sign-On Parameters**.

11. Nella pagina della finestra di dialogo **Configure SAML Single Sign-On Parameters** (Configura parametri accesso Single Sign-On SAML) procedere come descritto di seguito e quindi fare clic su **Done** (Fine)

    ![Single Sign-On](./media/zscaler-three-tutorial/ic800209.png "Single Sign-On")

    a. Incollare il valore **URL di accesso**  copiato dal portale di Azure nella casella di testo **URL of the SAML Portal to which users are sent for authentication** (URL del portale di SAML a cui vengono indirizzati gli utenti per l'autenticazione).

    b. Nella casella di testo **Attribute containing Login Name** digitare **NameID**.

    c. Per caricare il certificato scaricato fare clic su **Zscaler pem**.

    d. Selezionare **Enable SAML Auto-Provisioning**.

12. Nella pagina della finestra di dialogo **Configure User Authentication** seguire questa procedura:

    ![Amministrazione](./media/zscaler-three-tutorial/ic800210.png "Amministrazione")

    a. Fare clic su **Save**.

    b. Fare clic su **Attiva subito**.

## <a name="configuring-proxy-settings"></a>Configurazione delle impostazioni proxy

### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Per configurare le impostazioni proxy in Internet Explorer

1. Avviare **Internet Explorer**.

2. Selezionare **Opzioni Internet** dal menu **Strumenti** per aprire la finestra di dialogo **Opzioni Internet**.

     ![Opzioni Internet](./media/zscaler-three-tutorial/ic769492.png "Opzioni Internet")

3. Fare clic sulla scheda **Connessioni** .
  
     ![Connessioni](./media/zscaler-three-tutorial/ic769493.png "Connessioni")

4. Fare clic su **Impostazioni LAN** per aprire la finestra di dialogo **Impostazioni LAN**.

5. Nella sezione del server proxy seguire questa procedura:

    ![Server proxy](./media/zscaler-three-tutorial/ic769494.png "Server proxy")

    a. Selezionare **Usa un server proxy per la LAN**.

    b. Nella casella di testo Indirizzo digitare **gateway.zscalerthree.net**.

    c. Nella casella di testo Porta digitare **80**.

    d. Selezionare **Ignora server proxy per indirizzi locali**.

    e. Fare clic su **OK** per chiudere la finestra di dialogo **Impostazioni rete locale (LAN)**.

6. Fare clic su **OK** per chiudere la finestra di dialogo **Opzioni Internet**.

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente di test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

1. Nel riquadro sinistro del portale di Azure, selezionare **Azure Active Directory**, **Utenti** e quindi **Tutti gli utenti**.

    ![Creare un utente di Azure AD][100]

2. Selezionare **Nuovo utente** in alto nella schermata.

    ![Creazione di un utente di test di Azure AD](./media/zscaler-three-tutorial/create_aaduser_01.png) 

3. In Proprietà utente seguire questa procedura.

    ![Creazione di un utente di test di Azure AD](./media/zscaler-three-tutorial/create_aaduser_02.png)

    a. Nel campo **Nome** immettere **BrittaSimon**.
  
    b. Nel campo **Nome utente** digitare **brittasimon@yourcompanydomain.extension**  
    Ad esempio: BrittaSimon@contoso.com

    c. Selezionare **Proprietà**, selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella Password.

    d. Fare clic su **Create**(Crea).

### <a name="creating-a-zscaler-three-test-user"></a>Creazione di un utente di test di Zscaler Three

Per consentire agli utenti di Azure AD di accedere a Zscaler Three, è necessario eseguirne il provisioning in Zscaler Three. Nel caso di Zscaler Three, il provisioning è un'attività manuale.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Per configurare il provisioning utente, eseguire la procedura seguente:

1. Accedere al tenant **Zscaler Three**.

2. Fare clic su **Administration**.

    ![Amministrazione](./media/zscaler-three-tutorial/ic781035.png "Amministrazione")

3. Fare clic su **User Management**.

     ![Aggiungi](./media/zscaler-three-tutorial/ic781036.png "Aggiungi")

4. Nella scheda **Utenti** fare clic su **Aggiungi**.

    ![Aggiungi](./media/zscaler-three-tutorial/ic781037.png "Aggiungi")

5. Nella sezione Add User seguire questa procedura:

    ![Aggiungere un utente](./media/zscaler-three-tutorial/ic781038.png "Aggiungere un utente")

    a. Digitare **ID utente**, **nome visualizzato per l'utente**, **password**, **conferma della password** e quindi selezionare **gruppi** e **reparto** per un account Azure AD valido di cui si vuole eseguire il provisioning.

    b. Fare clic su **Save**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Zscaler Three per eseguire il provisioning degli account utente Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente di test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Zscaler Three.

1. Nel portale di Azure selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni**.

    ![Assegna utente][201]

2. Nell'elenco delle applicazioni selezionare **Zscaler Three**.

    ![Configure Single Sign-On](./media/zscaler-three-tutorial/tutorial_zscalerthree_app.png)

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202]

4. Fare clic su **Aggiungi utente** e quindi selezionare **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti e quindi fare clic sul pulsante **Seleziona** in basso nella schermata.

6. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna**.

### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Zscaler Three nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Zscaler Three.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-three-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-three-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-three-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-three-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-three-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-three-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-three-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-three-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-three-tutorial/tutorial_general_203.png