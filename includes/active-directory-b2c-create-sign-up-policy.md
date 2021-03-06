---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 511b05e6cae769a5b39ae81a3e67efd05d374511
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132816"
---
Se si vuole abilitare solo l'iscrizione nell'applicazione, si usano i criteri di **iscrizione**. Questi criteri descrivono l'esperienza utente durante la procedura di iscrizione e il contenuto dei token che l'applicazione riceverà al completamento dell'iscrizione.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Fare clic su **Criteri di iscrizione**.

Fare clic su **+Aggiungi** nella parte superiore del pannello.

Il **Nome** determina il nome dei criteri di iscrizione usati dall'applicazione. Immettere ad esempio **SiUp**.

Fare clic su **Provider di identità** e selezionare **Iscrizione posta elettronica**. Facoltativamente, è anche possibile selezionare i provider di identità tramite social network, se già configurati. Fare clic su **OK**.

Fare clic su **Attributi iscrizione**. Qui è possibile scegliere gli attributi che si desidera raccogliere dall'utente durante l'iscrizione. Selezionare ad esempio **Paese/Area**, **Nome visualizzato** e **Codice postale**. Fare clic su **OK**.

Fare clic su **Attestazioni applicazione**. Qui si scelgono le attestazioni che verranno restituite all'applicazione nei token inviati al termine di una corretta esperienza di iscrizione. Selezionare ad esempio **Nome visualizzato**, **Provider di identità**, **Codice postale**, **L'utente è nuovo** e l'**ID oggetto dell'utente**.

Fare clic su **Create**(Crea). Il criterio creato viene visualizzato come **B2C_1_SiUp** (il frammento **B2C\_1\_** viene aggiunto automaticamente) nel pannello **Criteri di iscrizione**.

Aprire il criterio facendo clic su **B2C_1_SiUp**.

Selezionare **Contoso B2C app** nel menu a discesa **Applicazioni** e `https://localhost:44321/` nel menu a discesa **URL di risposta/URI di reindirizzamento**.

Fare clic su **Esegui adesso**. Verrà visualizzata una nuova scheda del browser in cui è possibile eseguire l'esperienza utente di iscrizione per l'applicazione.

> [!NOTE]
> La creazione e gli aggiornamenti dei criteri avranno effetto dopo circa un minuto.
>