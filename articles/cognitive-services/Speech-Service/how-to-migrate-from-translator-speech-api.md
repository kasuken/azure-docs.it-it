---
title: Eseguire la migrazione dall'API Traduzione vocale al Servizio di riconoscimento vocale
titleSuffix: Azure Cognitive Services
description: Informazioni su come eseguire la migrazione delle applicazioni dall'API Traduzione vocale al Servizio di riconoscimento vocale.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: 81513819fd60dc088c2ed4a781562684c84e803a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50415475"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Eseguire la migrazione dall'API Traduzione vocale al Servizio di riconoscimento vocale

Usare le informazioni di questo articolo per eseguire la migrazione delle applicazioni dall'API Traduzione vocale Microsoft al [Servizio di riconoscimento vocale](index.yml). Questa guida descrive le differenze tra l'API Traduzione vocale e il Servizio di riconoscimento vocale e suggerisce strategie per la migrazione delle applicazioni.

> [!NOTE]
> La chiave di sottoscrizione dell'API Traduzione vocale non verrà accettata dal Servizio di riconoscimento vocale. È necessario avviare una nuova sottoscrizione al Servizio di riconoscimento vocale.

## <a name="comparison-of-features"></a>Confronto delle funzionalità

| Funzionalità                                           | API Traduzione vocale                                  | Servizio di riconoscimento vocale | Dettagli                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Traduzione in testo                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Traduzione in voce                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Endpoint globale                                   | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Il Servizio di riconoscimento vocale attualmente non offre un endpoint globale. Un endpoint globale può indirizzare automaticamente il traffico all'endpoint più vicino a livello di area, riducendo la latenza dell'applicazione.                                                    |
| Endpoint a livello di area                                | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Limite di tempo della connessione                             | 90 minuti                                               | Senza limiti con l'SDK. 10 minuti con una connessione WebSocket.                                                                                                                                                                                                                                                                                   |
| Chiave di autenticazione nell'intestazione                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Più lingue tradotte in una singola richiesta | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDK disponibili                                    | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Per informazioni sugli SDK disponibili, vedere la [documentazione del Servizio di riconoscimento vocale](index.yml).                                                                                                                                                    |
| Connessioni WebSocket                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| API di lingue                                     | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Il Servizio di riconoscimento vocale supporta la stessa serie di lingue descritte nell'articolo delle [informazioni di riferimento per le lingue dell'API Translator](../translator-speech/languages-reference.md). |
| Marcatore e filtro per contenuto volgare                       | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| .WAV/PCM come input                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Altri tipi di file come input                         | :heavy_minus_sign:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Risultati parziali                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Informazioni di temporizzazione                                       | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                 |
| ID correlazione                                    | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Modelli conversione voce/testo personalizzati                              | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Il Servizio di riconoscimento vocale offre modelli conversione voce/testo personalizzati che consentono di personalizzare il riconoscimento vocale in base allo specifico vocabolario dell'organizzazione.                                                                                                                                           |
| Modelli di traduzione personalizzati                         | :heavy_minus_sign:                                              | :heavy_check_mark:                 | La sottoscrizione all'API di traduzione testo Microsoft consente di impiegare [Custom Translator](https://www.microsoft.com/translator/business/customization/) (attualmente in anteprima) per usare i propri dati per traduzioni più accurate.                                                 |

## <a name="migration-strategies"></a>Strategie di migrazione

Se l'organizzazione dispone di applicazioni in fase di sviluppo o in produzione che usano l'API Traduzione vocale, è necessario aggiornarle per l'uso del Servizio di riconoscimento vocale. Per informazioni sugli SDK disponibili, esempi di codice ed esercitazioni, vedere la [documentazione del Servizio di riconoscimento vocale](index.yml). Quando si esegue la migrazione, tenere presente quanto segue:

* Il Servizio di riconoscimento vocale attualmente non offre un endpoint globale. Determinare se l'applicazione funziona in modo efficiente quando usa un singolo endpoint a livello di area per tutto il traffico. In caso contrario, usare la georilevazione per determinare l'endpoint più efficiente.

* Se l'applicazione usa connessioni di lunga durata e non può usare gli SDK disponibili, è possibile usare una connessione WebSocket. Gestire il limite di timeout di 10 minuti, eseguendo la riconnessione nel momento opportuno.

* Se l'applicazione usa l'API Traduzione testuale e l'API Traduzione vocale per abilitare i modelli di traduzione personalizzati, è possibile aggiungere gli ID di categoria direttamente usando il Servizio di riconoscimento vocale.

* A differenza dell'API Traduzione vocale, il Servizio di riconoscimento vocale può completare le traduzioni in più lingue in un'unica richiesta.

## <a name="next-steps"></a>Passaggi successivi

* [Provare gratuitamente il Servizio di riconoscimento vocale](get-started.md)
* [Avvio rapido: Riconoscimento vocale in un'applicazione della piattaforma UWP con Speech SDK](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Vedere anche 

* [Informazioni sul servizio Voce](overview.md)
* [Documentazione sul Servizio di riconoscimento vocale e Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
