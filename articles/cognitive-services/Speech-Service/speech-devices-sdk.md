---
title: Informazioni su Speech Devices SDK
titleSuffix: Azure Cognitive Services
description: Introduzione a Speech Devices SDK.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: eac3542059f1bc5d32a91ef871e5185fad1d2798
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49464098"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Informazioni su Speech Devices SDK (anteprima)

Il [servizio Voce](overview.md) è compatibile con una vasta gamma di dispositivi e sorgenti audio. Ora è possibile portare le applicazioni vocali a un livello superiore con hardware e software abbinati. Speech Devices SDK è una libreria pre-ottimizzata abbinata a kit dedicati per lo sviluppo di matrici di microfoni. 

Speech Devices SDK consente di:
* Testare rapidamente nuovi scenari vocali.
* Integrare più facilmente il servizio Voce basato sul cloud nel dispositivo.
* Creare un'esperienza utente eccezionale per i clienti. 

Speech Devices SDK è integrato con [Speech SDK](speech-sdk.md). Usa Speech SDK per inviare audio elaborato dall'algoritmo di elaborazione audio avanzata dalla matrice di microfoni del dispositivo al [servizio Voce](overview.md). Usa inoltre l'audio multicanale per fornire un servizio più preciso di [riconoscimento vocale](speech-to-text.md) in campo lontano tramite eliminazione del rumore e dell'eco, beamforming e riduzione dell'effetto riverbero.

Speech Devices SDK consente anche di creare dispositivi per il proprio ambiente con una [parola di attivazione personalizzata](speech-devices-sdk-create-kws.md), in modo da definire una parola chiave esclusiva del proprio marchio per avviare un'interazione utente. 

Speech Devices SDK facilita una varietà di scenari vocali, come sistemi di ordinazione drive-thru, assistenti in negozio o in casa e altoparlanti intelligenti. È possibile rispondere agli utenti con testo, parlare con loro con una voce predefinita o [personalizzata](how-to-customize-voice-font.md), fornire risultati di ricerca, [tradurre](speech-translation.md) in altre lingue e altro ancora. Microsoft sarà lieta di ricevere notizie sulle soluzioni vocali realizzate dai propri utenti.

## <a name="development-kit-providers"></a>Provider di kit di sviluppo

Attualmente sono disponibili questi progetti di sistemi di riferimento end-to-end: 

|||
|-|-|
|[![Logo ROOBO](media/speech-devices-sdk/roobo-logo.png)](http://ddk.roobo.com/)|ROOBO fornisce soluzioni complete di sistemi di intelligenza artificiale per elettrodomestici, automobili, robot, giocattoli e altri settori. I progetti di riferimento di ROOBO riducono notevolmente il time-to-market di sviluppo grazie all'integrazione del servizio Microsoft Speech. [Visitare ROOBO](http://ddk.roobo.com/).|

## <a name="next-steps"></a>Passaggi successivi

Per iniziare, ottenere un [account gratuito Azure](https://azure.microsoft.com/free/ai/) e registrarsi per Speech Devices SDK.

> [!div class="nextstepaction"]
> [Registrarsi per Speech Devices SDK](get-speech-devices-sdk.md)

