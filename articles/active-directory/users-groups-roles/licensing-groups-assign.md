---
title: Assegnare licenze a un gruppo in Azure Active Directory | Microsoft Docs
description: Come assegnare licenze agli utenti usando le licenze dei gruppi di Azure Active Directory
services: active-directory
keywords: Licenze di Azure AD
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 10/29/2018
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e08ca3453cc43fa0f35102ca5563b4b07ce45dea
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215005"
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Assegnare licenze agli utenti in base all'appartenenza ai gruppi in Azure Active Directory

Questo articolo spiega come assegnare licenze di prodotti a un gruppo di utenti in Azure Active Directory (Azure AD) e quindi verificare che le licenze siano state assegnate in modo corretto.

In questo esempio il tenant contiene un gruppo di sicurezza denominato **HR Department**. Il gruppo include tutti i membri del reparto Risorse umane, circa 1.000 utenti. Si vogliono assegnare licenze di Office 365 Enterprise E3 all'intero reparto. Il servizio Enterprise Yammer incluso nel prodotto deve essere temporaneamente disattivato finché il reparto è pronto per iniziare a usarlo. Si prevede anche di distribuire le licenze Enterprise Mobility + Security allo stesso gruppo di utenti.

> [!NOTE]
> Alcuni servizi Microsoft non sono disponibili in tutte le posizioni. Per poter assegnare una licenza a un utente, l'amministratore deve prima specificare la proprietà relativa alla località di utilizzo per l'utente.

> Per l'assegnazione di licenze ai gruppi, eventuali utenti per cui non è specificata una località d'uso ereditano la località della directory. Se gli utenti si trovano in località diverse, è consigliabile impostare sempre la località di utilizzo nell'ambito del flusso di creazione utente in Azure AD, ad esempio nella configurazione di AAD Connect. In questo modo si garantisce che il risultato dell'assegnazione delle licenze sia sempre corretto e che gli utenti non ricevano i servizi in località che non sono consentite.

## <a name="step-1-assign-the-required-licenses"></a>Passaggio 1: assegnare le licenze necessarie

1. Accedere al [**portale di Azure**](https://portal.azure.com) con un account Administrator. Per gestire le licenze, l'account deve essere un ruolo amministratore globale o amministratore dell'account utente.

2. Fare clic su **Tutti i servizi** nel riquadro di spostamento sinistro e quindi selezionare **Azure Active Directory**. È possibile aggiungere questo riquadro ai Preferiti o al dashboard del portale.

3. Nel riquadro **Azure Active Directory** selezionare **Licenze** per aprire un riquadro in cui visualizzare e gestire tutti i prodotti del tenant a cui è possibile assegnare una licenza.

4. In **All products** (Tutti i prodotti) selezionare sia Office 365 Enterprise E3 sia Enterprise Mobility + Security scegliendo i nomi di prodotto. Per avviare l'assegnazione, selezionare **Assegna** nella parte superiore del riquadro.

   ![Tutti i prodotti, assegnare una licenza](./media/licensing-groups-assign/all-products-assign.png)

5. Nel riquadro**Assegna licenza** fare clic su **Utenti e gruppi** per aprire il riquadro **Utenti e gruppi**. Cercare il nome del gruppo *Reparto risorse Umane*, selezionare il gruppo e quindi assicurarsi di confermare facendo clic su **Seleziona** nella parte inferiore del riquadro.

   ![Selezionare un gruppo](./media/licensing-groups-assign/select-a-group.png)

6. Nel riquadro **Assegna licenza** fare clic su **Opzioni di assegnazione (facoltativo)** per visualizzare tutti i piani di servizio inclusi nei due prodotti selezionati in precedenza. Trovare **Yammer Enterprise** e impostarlo su **Disattivato** per disabilitare il servizio dalla licenza del prodotto. Confermare facendo clic su **OK** nella parte inferiore di **Opzioni di assegnazione**.

   ![Opzioni di assegnazione](./media/licensing-groups-assign/assignment-options.png)

7. Per completare l'assegnazione, fare clic su **Assegna** nella parte inferiore del riquadro **Assegna licenza**.

8. Verrà visualizzata una notifica nell'angolo in alto a destra per indicare lo stato e il risultato del processo. Se non è stato possibile completare l'assegnazione al gruppo, ad esempio a causa di licenze già esistenti nel gruppo, fare clic sulla notifica per visualizzare i dettagli dell'errore.

È stato specificato un modello di licenza nel gruppo del reparto risorse umane. È stato avviato un processo in background in Azure AD per elaborare tutti i membri esistenti di tale gruppo. L'operazione iniziale potrebbe richiedere molto tempo, in base alla dimensione corrente del gruppo. Nel passaggio successivo viene spiegato come verificare se il processo è stato completato e determinare se sono necessari altri interventi per la risoluzione dei problemi.

> [!NOTE]
> È possibile avviare la stessa assegnazione da un percorso alternativo: **Utenti e gruppi** in Azure AD. Passare ad **Azure Active Directory** > **Utenti e gruppi** > **Tutti i gruppi**. Per trovare il gruppo, selezionarlo e passare alla scheda **Licenze**. Con il pulsante **Assegna** nella parte superiore del riquadro è possibile aprire il riquadro di assegnazione delle licenze.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>Passaggio 2: verificare che l'assegnazione iniziale sia terminata

1. Passare ad **Azure Active Directory** > **Utenti e gruppi** > **Tutti i gruppi**. Trovare quindi il gruppo **HR Department** a cui sono state assegnate le licenze.

2. Nel riquadro del gruppo **Reparto Risorse umane** selezionare **Licenze**. Questo consente di verificare rapidamente se le licenze sono state completamente assegnate agli utenti e se ci sono errori che richiedono attenzione. Sono disponibili le informazioni seguenti:

   - Elenco delle licenze di prodotto attualmente assegnate al gruppo. Selezionare una voce per visualizzare i servizi specifici che sono stati abilitati e apportare modifiche.

   - Stato delle modifiche più recenti delle licenze apportate al gruppo, se è in corso l'elaborazione delle modifiche o se l'elaborazione è stata completata per tutti i membri utente.

   - Informazioni sugli utenti con stato di errore perché non è stato possibile assegnare le licenze a tali utenti.

   ![Opzioni di assegnazione](./media/licensing-groups-assign/assignment-errors.png)

3. Per informazioni più dettagliate sull'elaborazione delle licenze, vedere **Azure Active Directory** > **Utenti e gruppi** > *nome gruppo* > **Log di controllo**. Si notino le attività seguenti:

   - Attività: **iniziare ad applicare agli utenti le licenze basate sui gruppi**. Viene registrato quando il sistema preleva la modifica dell'assegnazione della licenza per il gruppo e inizia ad applicarla a tutti i membri utente. Contiene informazioni sulla modifica apportata.

   - Attività: **finire di applicare agli utenti le licenze basate sui gruppi**. Viene registrato quando il nostro sistema completa l'elaborazione di tutti gli utenti del gruppo. Contiene un riepilogo di quanti utenti sono stati elaborati correttamente e il numero di utenti a cui non è stato possibile assegnare le licenze di gruppo.

   [Leggere questa sezione](licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) per ulteriori informazioni su come usare i log di controllo per analizzare le modifiche apportate dalle licenze basate sui gruppi.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Passaggio 3: Controllare i problemi relativi alle licenze e risolverli

1. Passare ad **Azure Active Directory** > **Utenti e gruppi** > **Tutti i gruppi**, quindi trovare il gruppo **HR Department** a cui sono state assegnate le licenze.
2. Nel riquadro del gruppo **Reparto Risorse umane** selezionare **Licenze**. La notifica nella parte superiore del riquadro indica che ci sono 10 utenti per cui non è stato possibile assegnare le licenze. Facendo clic su di essa viene aperto un elenco di tutti gli utenti che presentano un errore di licenza per questo gruppo.
3. La colonna **N. assegnazioni con errori** indica che non è stato possibile assegnare entrambe le licenze prodotto agli utenti. La colonna **Principali cause dell'errore** contiene la causa dell'errore. In questo caso si tratta di **Piani di servizio in conflitto**.

   ![N. assegnazioni con errori](./media/licensing-groups-assign/failed-assignments.png)

4. Selezionare un utente per aprire il riquadro **Licenze**. Questo riquadro mostra tutte le licenze attualmente assegnate all'utente. In questo esempio l'utente ha la licenza di Office 365 Enterprise E1 ereditata dal gruppo **Kiosk users**. Questo genera un conflitto con la licenza E3 che il sistema ha provato ad applicare dal gruppo **HR Department**. Nessuna licenza del gruppo è stata quindi assegnata all'utente.

   ![Visualizzare le licenze per un utente](./media/licensing-groups-assign/user-license-view.png)

5. Per risolvere questo conflitto, rimuovere l'utente dal gruppo **Kiosk users**. Dopo che Azure AD elabora la modifica, le licenze del **Reparto risorse umane** sono assegnate correttamente.

   ![Licenza assegnata correttamente](./media/licensing-groups-assign/license-correctly-assigned.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle funzionalità disponibili per la gestione delle licenze con i gruppi, vedere i seguenti articoli:

* [Che cosa sono le licenze basate sui gruppi in Azure Active Directory?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Identificazione e risoluzione dei problemi relativi alle licenze per un gruppo in Azure Active Directory](licensing-groups-resolve-problems.md)
* [Come eseguire la migrazione di singoli utenti con licenza alle licenze basate sui gruppi in Azure Active Directory](licensing-groups-migrate-users.md)
* [Come eseguire la migrazione degli utenti tra licenze di prodotti diverse con la gestione delle licenze basate su gruppo in Azure Active Directory](licensing-groups-change-licenses.md)
* [Scenari aggiuntivi relativi alle licenze basate sui gruppi in Azure Active Directory](../active-directory-licensing-group-advanced.md)
* [Esempi di PowerShell per le licenze basate sui gruppi in Azure Active Directory](licensing-ps-examples.md)
