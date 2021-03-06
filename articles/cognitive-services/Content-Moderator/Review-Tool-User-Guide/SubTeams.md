---
title: Team e sottoteam nell'API Content Moderator | Microsoft Docs
description: Informazioni su come usare team e sottoteam nell'API Content Moderator per Servizi cognitivi.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: 161c7cd8bac07d5ffc138297d98a40317a8d88fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372948"
---
# <a name="team-and-subteams"></a>Team e sottoteam #

Prima di usare Content Moderator, è necessario creare un team. Quando ci si registra e si assegna un nome al team, questo diventerà il team predefinito. Si può avere solo un team nello strumento di revisione. È tuttavia possibile creare più sottoteam. I sottoteam sono utili per la creazione di team di escalation o di team dedicati alla revisione di specifiche categorie di contenuto. Potrebbe essere necessario, ad esempio, inviare un contenuto per adulti a un team diverso per un'ulteriore revisione.

Questo argomento illustra come creare i sottoteam e assegnare rapidamente le revisioni in tempo reale. È tuttavia possibile usare i [flussi di lavoro](workflows.md) per assegnare le revisioni in base a criteri specifici.

## <a name="go-to-the-teams-setting"></a>Andare alle impostazioni dei team ##

Per iniziare a creare un sottoteam, selezionare l'opzione **Teams** (Team) in Settings (Impostazioni).

![Impostazioni team](images/0-teams-1.png)

## <a name="create-subteams"></a>Creare i sottoteam ##

Il team predefinito contiene tutti i potenziali revisori. I sottoteam sono subset del team predefinito. Poiché non è possibile assegnare qualcuno a un sottoteam se non è già membro del team predefinito, è necessario aggiungere ora i revisori al team predefinito. Fare clic sul pulsante (Invite) Invita nella pagina Team.

![Invitare gli utenti](images/invite-users.png)

### <a name="1-create-a-subteam"></a>1. Creare un sottoteam.
Scorrere verso il basso la pagina Team fino alla sezione Subteam (Sottoteam). Fare clic sul pulsante Add Subteam (Aggiungi sottoteam). 

![Aggiungere il sottoteam](images/1-teams-1.png)

### <a name="2-name-your-subteam"></a>2. Assegnare un nome al sottoteam.
Immettere il nome del sottoteam nella finestra di dialogo, quindi fare clic su Save (Salva).

![Nome del sottoteam](images/1-Teams-2.PNG)

### <a name="3-assign-members-from-your-default-team"></a>3. Assegnare i membri del team predefinito.
Fare clic sul pulsante Add Member (Aggiungi membro) per assegnare i membri del team predefinito a uno o più sottoteam. È possibile aggiungere a un sottoteam solo utenti esistenti. Per aggiungere nuovi utenti non presenti nello strumento di revisione, invitarli usando il pulsante "Invite" (Invita) nella pagina Team Settings (Impostazioni team).

![Assegnare i membri al sottoteam](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Assegnare le revisioni ai sottoteam ##

Dopo avere creato i sottoteam e avere assegnato i membri del team, è possibile iniziare ad assegnare le revisioni di immagini e testi a tali sottoteam. Questa operazione viene eseguita dalla finestra di revisione.
Per assegnare una singola immagine a un sottoteam, fare clic sui puntini di sospensione nell'angolo superiore destro dell'immagine, selezionare Move to (Sposta in) e selezionare un sottoteam.

![Assegnare la revisione di un'immagine a un sottoteam](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Passare da un sottoteam a un altro per la revisione del contenuto assegnato ##

Se si è membri di uno o più sottoteam, è possibile passare da uno all'altro dal dashboard degli strumenti di revisione. Per visualizzare tutte le revisioni correnti in sospeso appartenenti a un sottoteam, selezionare Choose Subteam (Scegli sottoteam) nella scheda Image (Immagine).

![Passare da un sottoteam a un altro](images/3-review-image-subteam-2.png)
