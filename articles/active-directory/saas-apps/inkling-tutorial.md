---
title: 'Esercitazione: Integrazione di Azure Active Directory con Inkling | Documentazione Microsoft'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Inkling.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 64c7ee45-ee8a-42f7-bf04-fd0e00833ea9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: cd7f8871cedb36157f3a16f093b09073576fe56e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443852"
---
# <a name="tutorial-azure-active-directory-integration-with-inkling"></a>Esercitazione: Integrazione di Azure Active Directory con Inkling

Questa esercitazione descrive come integrare Inkling con Azure Active Directory (Azure AD).

L'integrazione di Inkling con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Inkling
- È possibile abilitare gli utenti per l'accesso automatico a Inkling (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account da una posizione centrale: il portale di gestione di Azure

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Inkling sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Inkling abilitata per l'accesso Single Sign-On


> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di prova di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Inkling dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD


## <a name="adding-inkling-from-the-gallery"></a>Aggiunta di Inkling dalla raccolta
Per configurare l'integrazione di Inkling in Azure AD è necessario aggiungere Inkling dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Inkling dalla raccolta seguire questa procedura:**

1. Nel **[portale di gestione di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Aggiungi** nella parte superiore della finestra di dialogo.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Inkling**.

    ![Creazione di un utente test di Azure AD](./media/inkling-tutorial/tutorial_inkling_001.png)

1. Nel pannello dei risultati selezionare **Inkling** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/inkling-tutorial/tutorial_inkling_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Inkling in base a un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve sapere qual è l'utente di Inkling che corrisponde a un utente di Azure AD. In altre parole deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Inkling.

La relazione di collegamento viene stabilita assegnando il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente) in Inkling.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Inkling è necessario completare le operazioni seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente di test di Inkling](#creating-an-inkling-test-user)**: per avere una controparte di Britta Simon in Inkling collegata alla relativa rappresentazione in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di gestione di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Inkling.

**Per configurare l'accesso Single Sign-On di Azure AD con Inkling, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Inkling** del portale di gestione di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** in **Modalità** selezionare **Accesso basato su SAML** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/inkling-tutorial/tutorial_general_300.png)
    
1. Nella sezione **URL e dominio Inkling** seguire questa procedura:
    
    ![Configure Single Sign-On](./media/inkling-tutorial/tutorial_inkling_01.png)

    a. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://api.inkling.com/saml/v2/metadata/<user-id>`

    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: `https://api.inkling.com/saml/v2/acs/<user-id>`

    > [!NOTE] 
    > Si noti che questi non sono i valori reali. È necessario aggiornare questi valori con l'identificatore e l'URL di risposta effettivi. Per ottenere questi valori contattare il [team di supporto di Inkling](mailto:press@inkling.com).

1. Nella sezione **Certificato di firma SAML** fare clic su **Crea nuovo certificato**.

    ![Configure Single Sign-On](./media/inkling-tutorial/tutorial_general_400.png)  

1. Nella finestra di dialogo **Crea nuovo certificato** fare clic sull'icona del calendario e selezionare una **data di scadenza**. Fare quindi clic sul pulsante **Salva**.

    ![Configure Single Sign-On](./media/inkling-tutorial/tutorial_general_500.png)

1. Nella sezione **Certificato di firma SAML** selezionare **Make new certificate active** (Rendi attivo il nuovo certificato) e fare clic sul pulsante **Salva**.

    ![Configure Single Sign-On](./media/inkling-tutorial/tutorial_inkling_02.png)

1. Nella finestra popup **Rollover certificate** (Certificato di rollover) fare clic su **OK**.

    ![Configure Single Sign-On](./media/inkling-tutorial/tutorial_general_600.png)

1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Configure Single Sign-On](./media/inkling-tutorial/tutorial_inkling_03.png) 

1. Per ottenere la configurazione dell'accesso Single Sign-On per l'applicazione contattare il [team di supporto di Inkling](mailto:press@inkling.com) e fornire i **metadati** scaricati. 


### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di gestione di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di gestione di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/inkling-tutorial/create_aaduser_01.png) 

1. Andare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/inkling-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/inkling-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/inkling-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea). 



### <a name="creating-an-inkling-test-user"></a>Creazione di un utente di test di Inkling

In questa sezione viene creato un utente chiamato Britta Simon in Inkling. Collaborare con il [team di supporto di Inkling](mailto:press@inkling.com) per aggiungere gli utenti alla piattaforma Inkling.


### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Inkling.

![Assegna utente][200] 

**Per assegnare Britta Simon a Inkling seguire questa procedura:**

1. Nel portale di gestione di Azure aprire la visualizzazione applicazioni, passare alla visualizzazione directory e andare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **Inkling**.

    ![Configure Single Sign-On](./media/inkling-tutorial/tutorial_inkling_50.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    


### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Inkling nel pannello di accesso, si accederà automaticamente all'applicazione Inkling.


## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/inkling-tutorial/tutorial_general_01.png
[2]: ./media/inkling-tutorial/tutorial_general_02.png
[3]: ./media/inkling-tutorial/tutorial_general_03.png
[4]: ./media/inkling-tutorial/tutorial_general_04.png

[100]: ./media/inkling-tutorial/tutorial_general_100.png

[200]: ./media/inkling-tutorial/tutorial_general_200.png
[201]: ./media/inkling-tutorial/tutorial_general_201.png
[202]: ./media/inkling-tutorial/tutorial_general_202.png
[203]: ./media/inkling-tutorial/tutorial_general_203.png