---
title: Come gestire un'identità gestita assegnata dall'utente tramite il portale di Azure
description: Istruzioni dettagliate su come creare, elencare, eliminare e assegnare un ruolo a un'identità gestita assegnata dall'utente.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 2f2f338f7c4ddb885bc909f49815783e616b94c8
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300456"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Creare, elencare, eliminare o assegnare un ruolo a un'identità gestita assegnata dall'utente mediante il portale di Azure

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Le identità gestite per le risorse Azure forniscono ai servizi di Azure un'identità gestita in Azure Active Directory. È possibile usare questa identità per l'autenticazione ai servizi che supportano l'autenticazione di Azure AD senza dover inserire le credenziali nel codice. 

Questo articolo illustra come creare, elencare, eliminare o assegnare un ruolo a un'identità gestita assegnata dall'utente tramite il portale di Azure.

## <a name="prerequisites"></a>Prerequisiti

- Se non si ha familiarità con le identità gestite per le risorse di Azure, vedere la [sezione sulla panoramica](overview.md). **Assicurarsi di conoscere la [differenza tra identità assegnata dal sistema e identità gestita assegnata dall'utente](overview.md#how-does-it-work)**.
- Se non si ha un account Azure, [registrarsi per ottenere un account gratuito](https://azure.microsoft.com/free/) prima di continuare.

## <a name="create-a-user-assigned-managed-identity"></a>Creare un'identità gestita assegnata dall'utente

Per creare un'identità gestita assegnata dall'utente, all'account deve essere assegnato il ruolo [Collaboratore di identità gestite](/azure/role-based-access-control/built-in-roles#managed-identity-contributor).

1. Accedere al [portale di Azure](https://portal.azure.com) usando un account associato alla sottoscrizione di Azure per creare l'identità gestita assegnata dall'utente.
2. Nella casella di ricerca digitare *Identità gestite* e in **Servizi** fare clic su **Identità gestite**.
3. Fare clic su **Aggiungi** e immettere i valori nei campi seguenti nel riquadro **Crea identità gestita assegnata dall'utente**:
   - **Nome risorsa**: si tratta del nome dell'identità gestita assegnata dall'utente, ad esempio UAI1.
   - **Sottoscrizione**: scegliere la sottoscrizione in cui creare l'identità gestita assegnata dall'utente
   - **Gruppo di risorse**: creare un nuovo gruppo di risorse che conterrà l'identità gestita assegnata dall'utente o scegliere **Usa esistente** per creare l'identità gestita assegnata dall'utente in un gruppo di risorse esistente.
   - **Posizione**: scegliere la posizione in cui distribuire l'identità gestita assegnata dall'utente, ad esempio **Stati Uniti occidentali**.
4. Fare clic su **Create**(Crea).

![Creare un'identità gestita assegnata dall'utente](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Elencare le identità gestite assegnate dall'utente

Per elencare/leggere un'identità gestita assegnata dall'utente, all'account deve essere assegnato il ruolo [Operatore di identità gestite](/azure/role-based-access-control/built-in-roles#managed-identity-operator) o [Collaboratore di identità gestite](/azure/role-based-access-control/built-in-roles#managed-identity-contributor).

1. Accedere al [portale di Azure](https://portal.azure.com) usando un account associato alla sottoscrizione di Azure per elencare le identità gestite assegnate dall'utente.
2. Nella casella di ricerca digitare *Identità gestite* e in Servizi fare clic su **Identità gestite**.
3. Verrà restituito un elenco delle identità gestite assegnate dall'utente per la sottoscrizione.  Per visualizzare i dettagli di un'identità gestita assegnata dall'utente, fare clic sul nome dell'identità.

![Elencare l'identità gestita assegnata dall'utente](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Eliminare un'identità gestita assegnata dall'utente

Per eliminare un'identità gestita assegnata dall'utente, all'account deve essere assegnato il ruolo [Collaboratore di identità gestite](/azure/role-based-access-control/built-in-roles#managed-identity-contributor).

1. Accedere al [portale di Azure](https://portal.azure.com) usando un account associato alla sottoscrizione di Azure per eliminare l'identità gestita assegnata dall'utente.
2. Selezionare l'identità gestita assegnata dall'utente e fare clic su **Elimina**.
3. Nella finestra di conferma scegliere **Sì**.

![Eliminare un'identità gestita assegnata dall'utente](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Assegnare un ruolo a un'identità gestita assegnata dall'utente

Per assegnare un ruolo a un'identità gestita assegnata dall'utente, all'account deve essere assegnato il ruolo [Amministratore Accesso utenti](/azure/role-based-access-control/built-in-roles#user-access-administrator).

1. Accedere al [portale di Azure](https://portal.azure.com) usando un account associato alla sottoscrizione di Azure per elencare le identità gestite assegnate dall'utente.
2. Nella casella di ricerca digitare *Identità gestite* e in Servizi fare clic su **Identità gestite**.
3. Verrà restituito un elenco delle identità gestite assegnate dall'utente per la sottoscrizione.  Selezionare l'identità gestita assegnata dall'utente a cui si vuole assegnare un ruolo.
4. Selezionare **Controllo di accesso (IAM)** e quindi **Aggiungi**.

   ![Avvio dell'identità gestita assegnata dall'utente](./media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. Nel pannello Aggiungi autorizzazioni configurare i valori seguenti e quindi fare clic su **Salva**:
   - **Ruolo**: il ruolo da assegnare
   - **Assegna accesso a**: la risorsa a cui assegnare l'identità gestita assegnata dall'utente
   - **Seleziona**: il membro a cui assegnare l'accesso
   
   ![Identità gestita assegnata dall'utente - IAM](./media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)  