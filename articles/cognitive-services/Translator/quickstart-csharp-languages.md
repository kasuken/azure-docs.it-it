---
title: 'Guida introduttiva: Ottenere le lingue supportate, C# - API Traduzione testuale'
titleSuffix: Azure Cognitive Services
description: In questa guida introduttiva si ottiene un elenco di lingue supportate per la traduzione, la traslitterazione e la ricerca nei dizionari insieme a esempi usando l'API Traduzione testuale con C#.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: erhopf
ms.openlocfilehash: 9ac881adcf7d273c9a3bcea55d084acced59c107
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645406"
---
# <a name="quickstart-get-supported-languages-with-the-translator-text-rest-api-c"></a>Guida introduttiva: Ottenere le lingue supportate con l'API REST Traduzione testuale (C#)

In questa guida introduttiva si ottiene un elenco di lingue supportate per la traduzione, la traslitterazione e la ricerca nei dizionari insieme a esempi usando l'API Traduzione testuale.

## <a name="prerequisites"></a>Prerequisiti

Per eseguire questo codice in Windows, è necessario [Visual Studio 2017](https://www.visualstudio.com/downloads/). È possibile usare la versione gratuita Community Edition.

Per usare l'API Traduzione testuale, è necessario avere anche una chiave di sottoscrizione. Per informazioni, vedere [Come registrarsi all'API Traduzione testuale](translator-text-how-to-signup.md).

## <a name="languages-request"></a>Richiesta di lingue

> [!TIP]
> Ottenere la versione più recente del codice da [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp).

Il codice seguente ottiene un elenco di lingue supportate per la traduzione, la traslitterazione e la ricerca nei dizionari insieme a esempi tramite il metodo [Languages](./reference/v3-0-languages.md).

1. Creare un nuovo progetto C# nell'ambiente di sviluppo integrato preferito.
2. Aggiungere il codice riportato di seguito.
3. Sostituire il valore di `key` con una chiave di accesso valida per la sottoscrizione.
4. Eseguire il programma.

```csharp
using System;
using System.Net.Http;
using System.Text;
// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace TranslatorTextQuickStart
{
    class Program
    {
        static string host = "https://api.cognitive.microsofttranslator.com";
        static string path = "/languages?api-version=3.0";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        async static void GetLanguages()
        {
            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);
            var uri = host + path;
            var response = await client.GetAsync(uri);
            var result = await response.Content.ReadAsStringAsync();
            var json = JsonConvert.SerializeObject(JsonConvert.DeserializeObject(result), Formatting.Indented);
            // Note: If writing to the console, set this.
            // Console.OutputEncoding = UnicodeEncoding.UTF8;
            System.IO.File.WriteAllBytes("output.txt", Encoding.UTF8.GetBytes(json));
        }

        static void Main(string[] args)
        {
            GetLanguages();
            Console.ReadLine();
        }
    }
}
```

## <a name="languages-response"></a>Risposta alla richiesta di lingue

Viene restituita una risposta con esito positivo in formato JSON, come illustrato nell'esempio seguente:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Passaggi successivi

Esaminare il codice di esempio per questa guida introduttiva e per altre, incluse quelle relative alla traduzione e alla traslitterazione, e anche altri progetti di esempio di Traduzione testuale su GitHub.

> [!div class="nextstepaction"]
> [Esaminare gli esempi di codice C# su GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
