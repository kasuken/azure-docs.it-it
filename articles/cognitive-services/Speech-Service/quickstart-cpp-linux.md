---
title: "Guida introduttiva: Riconoscimento vocale in C++ su Linux con l'SDK del servizio Voce"
titleSuffix: Azure Cognitive Services
description: Informazioni sul riconoscimento vocale in C++ su Linux con l'SDK del servizio Voce
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: wolfma
ms.openlocfilehash: bfb71c000eea56e705b33fb97827aead23de8cbb
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219274"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>Avvio rapido: Riconoscimento vocale in C++ su Linux con Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In questo articolo, viene creata un'applicazione console C++ per Linux Ubuntu 16.04. Utilizzare i Servizi cognitivi [Speech SDK](speech-sdk.md) per trascrivere il riconoscimento vocale in tempo reale dal microfono del PC. L'applicazione viene compilata con il [Speech SDK per Linux](https://aka.ms/csspeech/linuxbinary) e il compilatore C++ della distribuzione di Linux (ad esempio, `g++`).

## <a name="prerequisites"></a>Prerequisiti

È necessaria una chiave di sottoscrizione del Servizio di riconoscimento vocale per completare la guida introduttiva. È possibile ottenerne una gratuitamente. Vedere [Provare gratuitamente il Servizio di riconoscimento vocale](get-started.md) per informazioni dettagliate.

## <a name="install-speech-sdk"></a>Installare Speech SDK

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

La versione corrente di Speech SDK di Servizi cognitivi è `1.1.0`.

Speech SDK per Linux è utilizzabile per compilare applicazioni sia a 64 bit che a 32 bit. Le librerie necessarie e i file di intestazione possono essere scaricati come un tarfile da https://aka.ms/csspeech/linuxbinary.

Scaricare e installare SDK come indicato di seguito:

1. Assicurarsi che le dipendenze SDK siano installate.

   ```sh
   sudo apt-get update
   sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
   ```

1. Scegliere una directory in cui estrarre i file di Speech SDK e impostare la `SPEECHSDK_ROOT` variabile di ambiente in modo che punti a tale directory. La variabile semplifica il riferimento alla directory nei comandi futuri. Ad esempio, se si desidera usare la directory `speechsdk` nella home directory, usare un comando simile al seguente:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Creare la directory se non esiste.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Scaricare ed estrarre l'`.tar.gz`archivio con i file binari di Speech SDK:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Convalidare il contenuto della directory di primo livello del pacchetto estratto:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   L'elenco di directory deve contenere la notifica di terzi e i file di licenza, nonché una `include` directory contenente i file (`.h`) di intestazione e una `lib` directory contenente le librerie.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-sample-code"></a>Aggiungere codice di esempio

1. Creare un file di origine C++ denominato `helloworld.cpp`e incollare il codice seguente al suo interno.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Nel nuovo file, sostituire la stringa `YourSubscriptionKey` con la chiave di sottoscrizione del Servizio di riconoscimento vocale.

1. Sostituire la stringa `YourServiceRegion` con la [regione](regions.md) associata alla sottoscrizione (ad esempio, `westus` per la sottoscrizione di valutazione gratuita).

## <a name="build-the-app"></a>Compilare l'app

> [!NOTE]
> Assicurarsi di immettere i comandi seguenti come una_riga di comando singola_. Il modo più semplice per farlo consiste nel copiare il comando usando il pulsante **Copia** accanto a ogni comando e quindi incollarlo nel prompt di shell.

* In un sistema **x64**  (64 bit), eseguire il comando seguente per compilare l'applicazione.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* In un sistema **x86** (32 bit), eseguire il comando seguente per compilare l'applicazione.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-app"></a>Esecuzione dell'app

1. Configurare il percorso di libreria del caricatore in modo da puntare alla libreria Speech SDK.

   * In un sistema**x64** (64 bit), immettere il comando seguente.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * In un sistema **x86** (32 bit), immettere questo comando.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Eseguire l'applicazione.

   ```sh
   ./helloworld
   ```

1.  Nella finestra della console, viene visualizzato un prompt che richiede di dire qualcosa. Pronunciare una frase o un'espressione in inglese. Il riconoscimento vocale viene trasmesso al Servizio di riconoscimento vocale e trascritto in formato testo, che appare nella stessa finestra.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Ricercare questo esempio nella cartella `quickstart/cpp-linux`.

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Riconoscere le finalità dai contenuti vocali con Speech SDK per C++](how-to-recognize-intents-from-speech-cpp.md)

## <a name="see-also"></a>Vedere anche 

- [Traduzione vocale](how-to-translate-speech-csharp.md)
- [Personalizzare modelli acustici](how-to-customize-acoustic-models.md)
- [Personalizzare modelli linguistici](how-to-customize-language-model.md)
