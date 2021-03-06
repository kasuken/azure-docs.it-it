---
title: Creare un certificato LDAP sicuro per un dominio gestito di Azure Active Directory Domain Services | Microsoft Docs
description: Creare un certificato LDAP sicuro per un dominio gestito di Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2017
ms.author: ergreenl
ms.openlocfilehash: a97b16451392ce0e84eb7b49a6fc71fb03adab12
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157293"
---
# <a name="create-a-pfx-file-with-the-secure-ldap-ldaps-certificate-for-a-managed-domain"></a>Creare un file PFX con il certificato LDAP sicuro (LDAPS) per un dominio gestito

## <a name="before-you-begin"></a>Prima di iniziare
Completare l'[Attività 1: Ottenere un certificato per l'accesso LDAP sicuro](active-directory-ds-admin-guide-configure-secure-ldap.md).


## <a name="task-2-export-the-secure-ldap-certificate-to-a-pfx-file"></a>Attività 2: Esportare il certificato LDAP sicuro in un file PFX
Prima di iniziare questa attività, ottenere il certificato LDAP sicuro da un'autorità di certificazione pubblica oppure creare un certificato autofirmato.

Per esportare il certificato LDAPS in un file PFX:

1. Fare clic sul pulsante **Start** e digitare **R**. Nella finestra di dialogo **Esegui** digitare **mmc** e fare clic su **OK**.

    ![Avviare la console MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. Nel prompt di **Controllo dell'account utente** fare clic su **Sì** per avviare Microsoft Management Console (MMC) come amministratore.
3. Nel menu **File** fare clic su **Aggiungi/Rimuovi snap-in...**.

    ![Aggiungere lo snap-in alla console MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. Nella finestra di dialogo **Aggiungi o rimuovi snap-in** selezionare lo snap-in **Certificati** e fare clic sul pulsante **Aggiungi >**.

    ![Aggiungere lo snap-in certificati alla console MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. Nella procedura guidata **Snap-in certificati** selezionare **Account del computer** e quindi fare clic su **Avanti**.

    ![Aggiungere lo snap-in certificati per l'account del computer](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. Nella pagina **Seleziona computer** selezionare **Computer locale (il computer su cui è in esecuzione questa console)** e fare clic su **Fine**.

    ![Aggiungere lo snap-in certificati, Seleziona computer](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. Nella finestra di dialogo **Aggiungi o rimuovi snap-in** fare clic su **OK** per aggiungere lo snap-in Certificati a MMC.

    ![Aggiungere lo snap-in certificati a MMC, Fatto](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. Nella finestra di MMC selezionare **Radice console**. Verrà visualizzato lo snap-in certificati caricato. Selezionare **Certificati (computer locale)** per espandere la voce. Selezionare il nodo **Personale** per espanderlo, poi selezionare il nodo **Certificati**.

    ![Aprire l'archivio certificati personali](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. Verrà visualizzato il certificato autofirmato che è stato creato. È possibile esaminare le proprietà del certificato per verificare che l'identificazione personale corrisponda a quella indicata nelle finestre di PowerShell al momento della creazione del certificato.
10. Selezionare il certificato autofirmato e **fare clic con il pulsante destro del mouse**. Selezionare **Tutte le attività** dal menu di scelta rapida e quindi **Esporta...**.

    ![Esportare il certificato](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. Nell'**esportazione guidata dei certificati** fare clic su **Avanti**.

    ![Esportare il certificato, procedura guidata](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. Nella pagina **Esportazione della chiave privata con il certificato** selezionare **Sì, esporta la chiave privata** e quindi fare clic su **Avanti**.

    ![Esportare il certificato, chiave privata](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > È NECESSARIO esportare la chiave privata insieme al certificato. Se si abilita l'accesso LDAP sicuro per il dominio gestito specificando un file PFX che non contiene la chiave privata per il certificato, l'operazione avrà esito negativo.
    >
    >

13. Nella pagina **Formato file di esportazione** selezionare **Scambio di informazioni personali - PKCS #12 (.PFX)** come formato di file per il certificato esportato.

    ![Esportare il certificato, formato di file](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > È supportato solo il formato di file PFX. Non esportare il certificato nel formato di file CER.
    >
    >

14. Nella pagina **Sicurezza** selezionare l'opzione **Password** e digitare una password per proteggere il file con estensione pfx. Prendere nota della password perché sarà necessaria nell'attività successiva. Fare clic su **Avanti**.

    ![Password per l'esportazione del certificato ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > Prendere nota della password. Sarà necessaria per i passaggi descritti nell'[Attività 3: Abilitare l'accesso LDAP sicuro per il dominio gestito](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
    >
    >

15. Nella pagina **File da esportare** specificare il nome del file e il percorso in cui esportare il certificato.

    ![Percorso per l'esportazione del certificato](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Nella pagina successiva fare clic su **Fine** per esportare il certificato in un file con estensione pfx. Al termine dell'esportazione del certificato verrà visualizzata una finestra di dialogo di conferma.

    ![Esportare il certificato, Fatto](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a>Passaggio successivo
[Attività 3: Abilitare l'accesso LDAP sicuro per il dominio gestito](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
