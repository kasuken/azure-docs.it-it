---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 505eac0996129a17b6b68e8ab4ea2d4fc80fd473
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134037"
---
1. Visitare il [portale di Azure]. Fare clic su **Esplora tutto** > **App per dispositivi mobili** > back-end appena creato. Nelle impostazioni dell'app per dispositivi mobili fare clic su **Guida introduttiva** > **Android**. In **Configure your client application** (Configurare l'applicazione client) fare clic su **Scarica**. In questo modo, viene scaricato il progetto Android per un'app preconfigurata da connettere al back-end. 
2. Aprire il progetto usando **Android Studio**, con **Import project (Eclipse ADT, Gradle, etc.)** (Importa progetto - Eclipse ADT, Gradle e così via). Assicurarsi di selezionare questa importazione per evitare eventuali errori JDK.
3. Fare clic sul pulsante **Esegui ‘app’** per creare il progetto e avviare l'app nel simulatore Android.
4. Nell'app digitare un testo significativo, ad esempio *Complete the tutorial* quindi fare clic sul pulsante ’Add’. Verrà inviata una richiesta POST al back-end di Azure distribuito in precedenza. Il back-end inserisce i dati della richiesta nella tabella SQL TodoItem e restituisce le informazioni sugli elementi appena archiviati all'app per dispositivi mobili. L'app per dispositivi mobili consente di visualizzare questi dati nell'elenco. 
   
    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Portale di Azure]: https://portal.azure.com/
