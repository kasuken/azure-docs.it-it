---
title: 'Guida introduttiva: Analizzare il contenuto delle immagini per individuare materiale inappropriato in C#'
titlesuffix: Azure Cognitive Services
description: Come analizzare il contenuto delle immagini per individuare materiale inappropriato di diverso tipo usando Content Moderator SDK per .NET
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 10/26/2018
ms.author: sajagtap
ms.openlocfilehash: 8f407a42ab2e1538193206dec1955257a5f9940a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51005496"
---
# <a name="quickstart-analyze-image-content-for-objectionable-material-in-c"></a>Guida introduttiva: Analizzare il contenuto delle immagini per individuare materiale inappropriato in C#

Questo articolo contiene informazioni ed esempi di codice utili per iniziare a usare [Content Moderator SDK per .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/). Si imparerà a rilevare contenuti spinti o per adulti, testo estraibile e visi umani allo scopo di moderare materiale potenzialmente inappropriato.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare. 

## <a name="prerequisites"></a>Prerequisiti

- Una chiave di sottoscrizione di Content Moderator. Seguire le istruzioni in [Creare un account Servizi cognitivi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) per effettuare la sottoscrizione a Content Moderator e ottenere la chiave.
- Qualsiasi edizione di [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).


> [!NOTE]
> In questa guida viene usata una sottoscrizione di Content Moderator del livello gratuito. Per informazioni su quanto offerto da ogni livello di sottoscrizione, vedere la pagina [Prezzi e limiti](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/).

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

1. In Visual Studio creare un nuovo progetto **App console (.NET Framework)** e denominarlo **ImageModeration**. 
1. Se la soluzione contiene anche altri progetti, selezionare questo come solo progetto di avvio.
1. Ottenere i pacchetti NuGet necessari. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere **Gestisci pacchetti NuGet**, quindi trovare e installare i pacchetti seguenti:
    - Microsoft.Azure.CognitiveServices.ContentModerator
    - Microsoft.Rest.ClientRuntime
    - Newtonsoft.Json

## <a name="add-image-moderation-code"></a>Aggiungere codice per la moderazione delle immagini

Copiare e incollare nel progetto il codice contenuto in questa guida per implementare uno scenario di base di moderazione del contenuto.

### <a name="include-namespaces"></a>Includere gli spazi dei nomi

Aggiungere le istruzioni `using` seguenti all'inizio del file *Program.cs*.

```csharp
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
```

### <a name="create-the-content-moderator-client"></a>Creare il client di Content Moderator

Aggiungere il codice seguente al file *Program.cs* per creare un provider client di Content Moderator per la sottoscrizione. Aggiungere il codice accanto alla classe **Program**, nello stesso spazio dei nomi. Sarà necessario aggiornare i campi **AzureRegion** e **CMSubscriptionKey** con i valori rispettivamente dell'identificatore di area e della chiave di sottoscrizione.

```csharp
// Wraps the creation and configuration of a Content Moderator client.
public static class Clients
{
    // The region/location for your Content Moderator account, 
    // for example, westus.
    private static readonly string AzureRegion = "YOUR API REGION";

    // The base URL fragment for Content Moderator calls.
    private static readonly string AzureBaseURL =
        $"https://{AzureRegion}.api.cognitive.microsoft.com";

    // Your Content Moderator subscription key.
    private static readonly string CMSubscriptionKey = "YOUR API KEY";

    // Returns a new Content Moderator client for your subscription.
    public static ContentModeratorClient NewClient()
    {
        // Create and initialize an instance of the Content Moderator API wrapper.
        ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

        client.Endpoint = AzureBaseURL;
        return client;
    }
}
```

### <a name="set-up-input-and-output-targets"></a>Configurare le destinazioni di input e output

Aggiungere i campi statici seguenti alla classe **Program** in _Program.cs_. Questi campi specificano i file per il contenuto delle immagini di input e il contenuto JSON di output.

```csharp
//The name of the file that contains the image URLs to evaluate.
private static string ImageUrlFile = "ImageFiles.txt";

///The name of the file to contain the output from the evaluation.
private static string OutputFile = "ModerationOutput.json";
```

Occorrerà creare il file di input *_ImageFiles.txt* e aggiornarne il percorso di conseguenza (i percorsi relativi sono relativi alla directory di esecuzione). Aprire _ImageFiles.txt_ e aggiungere gli URL delle immagini da moderare. Questa guida introduttiva usa gli URL seguenti come input di esempio.
```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

### <a name="create-a-class-to-handle-results"></a>Creare una classe per gestire i risultati

Aggiungere il codice seguente a *Program.cs*, accanto alla classe **Program** nello stesso spazio dei nomi. Per registrare i risultati della moderazione per ognuna delle immagini riviste, si userà un'istanza di questa classe.

```csharp
// Contains the image moderation results for an image, 
// including text and face detection results.
public class EvaluationData
{
    // The URL of the evaluated image.
    public string ImageUrl;

    // The image moderation results.
    public Evaluate ImageModeration;

    // The text detection results.
    public OCR TextDetection;

    // The face detection results;
    public FoundFaces FaceDetection;
}
```

### <a name="define-the-image-evaluation-method"></a>Definire il metodo di valutazione delle immagini

Aggiungere il metodo seguente alla classe **Program**. Questo metodo valuta una singola immagine in tre modi diversi e restituisce il risultato della valutazione. Per informazioni più approfondite sulle diverse operazioni, selezionare il collegamento nella sezione [Passaggi successivi](#next-steps).

```csharp
// Evaluates an image using the Image Moderation APIs.
private static EvaluationData EvaluateImage(
  ContentModeratorClient client, string imageUrl)
{
    var url = new BodyModel("URL", imageUrl.Trim());

    var imageData = new EvaluationData();

    imageData.ImageUrl = url.Value;

    // Evaluate for adult and racy content.
    imageData.ImageModeration =
        client.ImageModeration.EvaluateUrlInput("application/json", url, true);
    Thread.Sleep(1000);

    // Detect and extract text.
    imageData.TextDetection =
        client.ImageModeration.OCRUrlInput("eng", "application/json", url, true);
    Thread.Sleep(1000);

    // Detect faces.
    imageData.FaceDetection =
        client.ImageModeration.FindFacesUrlInput("application/json", url, true);
    Thread.Sleep(1000);

    return imageData;
}
```

### <a name="load-the-input-images"></a>Caricare le immagini di input

Aggiungere il codice seguente al metodo **Main** nella classe **Program**. In questo modo viene configurato il programma per recuperare i dati di valutazione per ogni URL di immagine nel file di input. Questi dati vengono quindi scritti in un singolo file di output.

```csharp
// Create an object to store the image moderation results.
List<EvaluationData> evaluationData = new List<EvaluationData>();

// Create an instance of the Content Moderator API wrapper.
using (var client = Clients.NewClient())
{
    // Read image URLs from the input file and evaluate each one.
    using (StreamReader inputReader = new StreamReader(ImageUrlFile))
    {
        while (!inputReader.EndOfStream)
        {
            string line = inputReader.ReadLine().Trim();
            if (line != String.Empty)
            {
                EvaluationData imageData = EvaluateImage(client, line);
                evaluationData.Add(imageData);
            }
        }
    }
}

// Save the moderation results to a file.
using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
{
    outputWriter.WriteLine(JsonConvert.SerializeObject(
        evaluationData, Formatting.Indented));

    outputWriter.Flush();
    outputWriter.Close();
}
```

## <a name="run-the-program"></a>Eseguire il programma

Il programma scriverà i dati di stringa JSON nel file _ModerationOutput.json_. Le immagini di esempio usate in questa guida introduttiva producono l'output seguente. Si noti che per ogni immagine sono presenti sezioni `ImageModeration`, `FaceDetection` e `TextDetection` diverse, che corrispondono alle tre chiamate API del metodo **EvaluateImage**.

```json
[{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg",
  "ImageModeration": {
    "cacheID": "7733c303-3b95-4710-a41e-7a322ae81a15_636488005858745661",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_687c356d-0f00-4aeb-ae5f-c7555af80247",
    "adultClassificationScore": 0.019196987152099609,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.032390203326940536,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "116"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "12"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_814fa162-c5d6-4ca6-997b-30ed0686ca83",
    "cacheId": "3fb69496-c64b-4de9-affd-6dd6d23f3e78_636488005876558920",
    "language": "eng",
    "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_a2c40dbe-609d-4eb8-b01c-9988388804ea",
    "cacheId": "e4c0b500-ea8e-4a31-8fb3-35f98c4fbd65_636488005889528303",
    "result": false,
    "count": 0,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "11"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "273405"
      }
    ],
    "faces": []
  }
},
{
  "ImageUrl": "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png",
  "ImageModeration": {
    "cacheID": "b4866aa2-5e69-44ed-806a-f9a5d618c8ae_636488005930693926",
    "result": false,
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_fdce5510-f689-4791-b081-c2ad54dcfe78",
    "adultClassificationScore": 0.0035635426174849272,
    "isImageAdultClassified": false,
    "racyClassificationScore": 0.021369094029068947,
    "isImageRacyClassified": false,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "109"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    }
  },
  "TextDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "metadata": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "46"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_08a4bc19-6010-41bb-a440-a77278e167d8",
    "cacheId": "28b37471-41b3-4f79-bd23-965498bcff51_636488005950851288",
    "language": "eng",
    "text": "",
    "candidates": []
  },
  "FaceDetection": {
    "status": {
      "code": 3000.0,
      "description": "OK",
      "exception": null
    },
    "trackingId": "WE_e1f20803b4ed471fb5de7df551f5bd9f_ContentModerator.Preview_40f2ce07-14ba-47cd-ba09-58b557a89854",
    "cacheId": "ec9c1be3-99b7-4bd9-8bc4-dc958c74459f_636488005964914299",
    "result": true,
    "count": 6,
    "advancedInfo": [
      {
        "key": "ImageDownloadTimeInMs",
        "value": "60"
      },
      {
        "key": "ImageSizeInBytes",
        "value": "2278902"
      }
    ],
    "faces": [
      {
        "bottom": 598,
        "left": 44,
        "right": 268,
        "top": 374
      },
      {
        "bottom": 620,
        "left": 308,
        "right": 532,
        "top": 396
      },
      {
        "bottom": 575,
        "left": 594,
        "right": 773,
        "top": 396
      },
      {
        "bottom": 563,
        "left": 812,
        "right": 955,
        "top": 420
      },
      {
        "bottom": 611,
        "left": 972,
        "right": 1151,
        "top": 432
      },
      {
        "bottom": 510,
        "left": 1232,
        "right": 1456,
        "top": 286
      }
    ]
  }
}]
```

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva è stata sviluppata una semplice applicazione .NET che usa il servizio Content Moderator per restituire informazioni rilevanti su un determinato esempio di immagine. Nel prossimo articolo si imparerà il significato dei diversi flag e delle diverse classificazioni, in modo da poter decidere quali dati sono necessari e in che modo devono essere gestiti dall'app.

> [!div class="nextstepaction"]
> [Guida alla moderazione delle immagini](image-moderation-api.md)
