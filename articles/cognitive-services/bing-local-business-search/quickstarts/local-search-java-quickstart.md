---
title: "Guida introduttiva: Inviare una query all'API Bing Local Business Search mediante Java | Microsoft Docs"
titleSuffix: Azure Cognitive Services
description: Seguire questo articolo per iniziare a usare l'API Bing Local Business Search in Java.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: e55c06c7e8a2d0952cac809b622fa9a63ccee399
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959801"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-java"></a>Guida introduttiva: Inviare una query all'API Bing Local Business Search mediante Java

Seguire questa Guida introduttiva per inviare richieste all'API Bing Local Business Search, che è un servizio cognitivo di Azure. L'applicazione è scritta in Java, ma l'API è un servizio Web RESTful compatibile con qualsiasi linguaggio di programmazione in grado di eseguire richieste HTTP e analizzare dati JSON.

Questa applicazione di esempio recupera i dati di risposta locali dall'API per la query di ricerca `hotel in Bellevue`.

## <a name="prerequisites"></a>Prerequisiti

* [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

È necessario disporre di un [account API Servizi cognitivi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con le API di ricerca Bing. Per questa guida introduttiva è sufficiente la [versione di valutazione gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api). È necessaria la chiave di accesso che viene fornita quando si attiva la versione di prova gratuita.

Questa applicazione di esempio recupera i dati di risposta locali dall'API dalla query per un *hotel a Bellevue*.

## <a name="create-the-request"></a>Creare la richiesta 

Il codice seguente crea `WebRequest`, imposta l'intestazione della chiave di accesso e aggiunge una stringa di query per "hotel a Bellevue".  Quindi, invia la richiesta e assegna la risposta a una stringa per contenere il testo JSON.

````
    // construct URL of search request (endpoint + query string)
     URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "appid=AEA845921DC03F506DC317A90EDDBF33074523F7&traffictype=Internal_monitor&market=en-us");
    HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
    //connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

    // receive JSON body
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();

    // construct result object for return
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);
````

## <a name="run-the-complete-application"></a>Eseguire l'applicazione completa

L'API Bing Local Business Search restituisce i risultati dal motore di ricerca Bing.
1. Scaricare o installare la libreria gson.
2. Creare un nuovo progetto Java nell'ambiente di sviluppo integrato o nell'editor preferito.
3. Aggiungere il codice fornito di seguito.
4. Sostituire il valore subscriptionKey con una chiave di accesso valida per la sottoscrizione.
5. Eseguire il programma.

````
package localSearch;
import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * Once you have compiled or downloaded gson-2.8.1.jar, assuming you have placed it in the
 * same folder as this file (localSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac localSearch.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar localSearch
 */
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class LocalSearchCls {

    // ***********************************************
    // *** Update or verify the following values. ***
    // **********************************************

        // Replace the subscriptionKey string value with your valid subscription key.
        static String subscriptionKey = "YOUR-ACCESS-KEY";

        static String host = "https://api.cognitive.microsoft.com/bing";
        static String path = "/v7.0/localbusinesses/search";

        static String searchTerm = "Hotel in Bellevue";

        public static SearchResults SearchLocal (String searchQuery) throws Exception {
            // construct URL of search request (endpoint + query string)
            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + 
                         "&appid=" + subscriptionKey + "&traffictype=Internal_monitor&market=en-us");
            HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
            //connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

            // receive JSON body
            InputStream stream = connection.getInputStream();
            String response = new Scanner(stream).useDelimiter("\\A").next();

            // construct result object for return
            SearchResults results = new SearchResults(new HashMap<String, String>(), response);

            // extract Bing-related HTTP headers
            Map<String, List<String>> headers = connection.getHeaderFields();
            for (String header : headers.keySet()) {
                if (header == null) continue;      // may have null key
                if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
                    results.relevantHeaders.put(header, headers.get(header).get(0));
                }
            }

            stream.close();
            return results;
        }

        // pretty-printer for JSON; uses GSON parser to parse and re-serialize
        public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }

        public static void main (String[] args) {
            try {
                System.out.println("Searching the Web for: " + searchTerm);

                SearchResults result = SearchLocal(searchTerm);

                System.out.println("\nRelevant HTTP Headers:\n");
                for (String header : result.relevantHeaders.keySet())
                    System.out.println(header + ": " + result.relevantHeaders.get(header));

                System.out.println("\nJSON Response:\n");
                System.out.println(prettify(result.jsonResponse));
            }
            catch (Exception e) {
                e.printStackTrace(System.out);
                System.exit(1);
            }
        }
    }

    // Container class for search results encapsulates relevant headers and JSON data
    class SearchResults{
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
    }

````

## <a name="next-steps"></a>Passaggi successivi
- [Guida introduttiva a Local Business Search](local-quickstart.md)
- [Guida introduttiva a Local Business Search con Node](local-search-node-quickstart.md)
- [Guida introduttiva a Local Business Search con Python](local-search-python-quickstart.md)