---
title: Procedura guidata di sicurezza in PIM - Azure | Microsoft Docs
description: Descrive la procedura guidata di sicurezza visualizzata la prima volta che si usa Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 178a4c5e978075f2a59b22a1cccf462138527964
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189080"
---
# <a name="security-wizard-in-pim"></a>Procedura guidata di sicurezza in PIM
Se si è il primo utente a eseguire Azure Privileged Identity Management (PIM) per l'organizzazione, viene visualizzata una procedura guidata. La procedura guidata offre informazioni sui rischi di sicurezza delle identità con privilegi e su come usare PIM per ridurre tali rischi. Se si vuole farlo in seguito, non è necessario apportare modifiche alle assegnazioni dei ruoli esistenti nella procedura guidata.

## <a name="what-to-expect"></a>Cosa aspettarsi
Prima che l'organizzazione inizi a usare PIM, tutte le assegnazioni dei ruoli sono permanenti, ovvero gli utenti sono sempre presenti nei ruoli anche se in quel momento non necessitano dei privilegi del ruolo.  Il primo passaggio della procedura guidata visualizza un elenco dei ruoli con privilegi elevati e il numero di utenti attualmente presenti in tali ruoli. È possibile visualizzare i dettagli di un ruolo particolare per visualizzare altre informazioni sugli utenti nel caso in cui uno o più utenti non siano noti.

Il secondo passaggio della procedura guidata offre la possibilità di modificare le assegnazioni dei ruoli di amministratore.  

> [!WARNING]
> È importante che siano presenti almeno un amministratore globale e più amministratori dei ruoli con privilegi con account aziendali e non account Microsoft. Se è presente un solo amministratore dei ruoli con privilegi, l'organizzazione non sarà in grado di gestire PIM se tale account viene eliminato.
> Mantenere inoltre assegnazioni permanenti di ruoli se l'utente dispone di un account Microsoft (usato per accedere a servizi Microsoft come Skype e Outlook.com). Se si prevede di richiedere l'autenticazione MFA per l'attivazione di questo ruolo, l'utente verrà bloccato.
> 
> 

Dopo aver apportato le modifiche, la procedura guidata non verrà più visualizzata. Al successivo uso di PIM, anche da parte di un altro amministratore dei ruoli con privilegi, verrà visualizzato il dashboard di PIM.  

* Se si vuole aggiungere o rimuovere utenti dai ruoli o modificare le assegnazioni da permanenti a idonee, vedere [Come aggiungere o rimuovere un ruolo utente](pim-how-to-add-role-to-user.md)per altre informazioni.
* Per concedere a più utenti l'accesso per la gestione di PIM, vedere l'articolo [Come concedere l'accesso per la gestione di Azure AD Privileged Identity Management](pim-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Passaggi successivi

- [Iniziare a usare PIM](pim-getting-started.md)
- [Assegnare ruoli della directory di Azure AD in PIM](pim-how-to-add-role-to-user.md)
- [Concedere l'accesso ad altri amministratori per gestire PIM](pim-how-to-give-access-to-pim.md)
