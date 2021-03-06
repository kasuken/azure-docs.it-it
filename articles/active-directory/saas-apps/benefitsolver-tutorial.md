---
title: 'Esercitazione: Integrazione di Azure Active Directory con Benefitsolver | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Benefitsolver.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 333394c1-b5a7-489c-8f7b-d1a5b4e782ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: jeedes
ms.openlocfilehash: a14ac0d0b7cae515c2daad055e542fda93986392
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430271"
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Esercitazione: Integrazione di Azure Active Directory con Benefitsolver

Questa esercitazione descrive come integrare Benefitsolver con Azure Active Directory (Azure AD).

L'integrazione di Benefitsolver con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Benefitsolver.
- È possibile abilitare gli utenti per l'accesso automatico a Benefitsolver (Single Sign-On) con gli account Azure AD personali.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Benefitsolver, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Benefitsolver abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Benefitsolver dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-benefitsolver-from-the-gallery"></a>Aggiunta di Benefitsolver dalla raccolta
Per configurare l'integrazione di Benefitsolver in Azure AD, è necessario aggiungere Benefitsolver dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere Benefitsolver dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **Benefitsolver**, nel pannello dei risultati selezionare **Benefitsolver** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Benefitsolver nell'elenco risultati](./media/benefitsolver-tutorial/tutorial_benefitsolver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Benefitsolver usando un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Benefitsolver corrispondente a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Benefitsolver.

Per stabilire la relazione di collegamento, in Benefitsolver assegnare il valore del **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Benefitsolver, è necessario completare le procedure di base seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente di test di Benefitsolver](#create-a-benefitsolver-test-user)**: per avere una controparte di Britta Simon in Benefitsolver collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Benefitsolver.

**Per configurare l'accesso Single Sign-On di Azure AD con Benefitsolver, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Benefitsolver** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/benefitsolver-tutorial/tutorial_benefitsolver_samlbase.png)

1. Nella sezione **URL e dominio Benefitsolver** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di Benefitsolver](./media/benefitsolver-tutorial/tutorial_benefitsolver_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `http://<companyname>.benefitsolver.com`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://<companyname>.benefitsolver.com/saml20`

    c. Nella casella di testo **URL di risposta** digitare l'URL: `https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'URL di accesso, l'identificatore e l'URL di risposta effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di Benefitsolver](https://www.businessolver.com/contact).

1. L'applicazione Benefitsolver prevede un formato specifico per le asserzioni SAML. È quindi necessario aggiungere mapping di attributi personalizzati alla configurazione di **Attributi token SAML**.

    ![Sezione degli attributi di Benefitsolver](./media/benefitsolver-tutorial/tutorial_attribute.png)

1. Nella sezione **Attributi utente** della finestra di dialogo **Single Sign-On** configurare l'attributo del token SAML come illustrato nell'immagine e seguire questa procedura:
    
    | Nome attributo| Valore attributo|
    |---------------|----------------|
    | ClientID | È necessario ottenere questo valore dal [team di supporto clienti di Benefitsolver](https://www.businessolver.com/contact).|
    | ClientKey | È necessario ottenere questo valore dal [team di supporto clienti di Benefitsolver](https://www.businessolver.com/contact).|
    | LogoutURL | È necessario ottenere questo valore dal [team di supporto clienti di Benefitsolver](https://www.businessolver.com/contact).|
    | EmployeeID | È necessario ottenere questo valore dal [team di supporto clienti di Benefitsolver](https://www.businessolver.com/contact).|

    a. Fare clic su Aggiungi attributo per aprire la finestra di dialogo Aggiungi attributo.

    ![Sezione degli attributi di Benefitsolver](./media/benefitsolver-tutorial/tutorial_attribute_04.png)
    
    ![Sezione degli attributi di Benefitsolver](./media/benefitsolver-tutorial/tutorial_attribute_05.png)

    b. Nella casella di testo **Nome** digitare il nome dell'attributo indicato per la riga.
    
    c. Nell'elenco **Valore** digitare il valore dell'attributo indicato per la riga.
    
    d. Fare clic su **OK**.

1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Collegamento di download del certificato](./media/benefitsolver-tutorial/tutorial_benefitsolver_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/benefitsolver-tutorial/tutorial_general_400.png)

1. Per configurare l'accesso Single Sign-On sul lato **Benefitsolver**, è necessario inviare il file **XML metadati** scaricato al [team di supporto di Benefitsolver](https://www.businessolver.com/contact).

    > [!NOTE]
    > Il team di supporto di Benefitsolver si occuperà dell'effettiva configurazione dell'accesso Single Sign-On. Una volta completata l'abilitazione dell'accesso Single Sign-On per la sottoscrizione, si riceverà una notifica.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili in [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985) (Documentazione incorporata di Azure AD).
> 

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/benefitsolver-tutorial/create_aaduser_01.png)

1. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/benefitsolver-tutorial/create_aaduser_02.png)

1. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/benefitsolver-tutorial/create_aaduser_03.png)

1. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/benefitsolver-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="create-a-benefitsolver-test-user"></a>Creare un utente di test di Benefitsolver

Per consentire agli utenti di Azure AD di accedere a Benefitsolver, è necessario eseguirne il provisioning in Benefitsolver. Nel caso di Benefitsolver, i dati dei dipendenti si trovano nell'applicazione popolata tramite un file di censimento del sistema HRIS, in genere durante la notte.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Benefitsolver per eseguire il provisioning degli account utente di Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Benefitsolver.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a Benefitsolver, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **Benefitsolver**.

    ![Collegamento di Benefitsolver nell'elenco delle applicazioni](./media/benefitsolver-tutorial/tutorial_benefitsolver_app.png)  

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Benefitsolver nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Benefitsolver.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/benefitsolver-tutorial/tutorial_general_01.png
[2]: ./media/benefitsolver-tutorial/tutorial_general_02.png
[3]: ./media/benefitsolver-tutorial/tutorial_general_03.png
[4]: ./media/benefitsolver-tutorial/tutorial_general_04.png

[100]: ./media/benefitsolver-tutorial/tutorial_general_100.png

[200]: ./media/benefitsolver-tutorial/tutorial_general_200.png
[201]: ./media/benefitsolver-tutorial/tutorial_general_201.png
[202]: ./media/benefitsolver-tutorial/tutorial_general_202.png
[203]: ./media/benefitsolver-tutorial/tutorial_general_203.png

