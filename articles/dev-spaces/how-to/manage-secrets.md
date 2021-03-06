---
title: Come gestire i segreti quando si lavora in uno spazio Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: iainfoulds
ms.author: iainfou
ms.date: 05/11/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Sviluppo rapido Kubernetes con contenitori e microservizi in Azure
keywords: Docker, Kubernetes, Azure, AKS, servizio contenitore di Azure, contenitori
ms.openlocfilehash: d7c8d0e1aca03a47e55605889c9f3fa73837d85a
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2018
ms.locfileid: "50977159"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Come gestire i segreti quando si lavora in uno spazio Azure Dev Spaces

I servizi potrebbero richiedere alcune password, stringhe di connessione e altri segreti, ad esempio per i database o altri servizi sicuri di Azure. Impostando i valori di questi segreti nei file di configurazione, è possibile renderli disponibili nel codice come variabili di ambiente,  che devono essere gestite con attenzione per evitare di compromettere la sicurezza dei segreti.

Azure Dev Spaces offre due opzioni consigliate per l'archiviazione dei segreti: nel file values.dev.yaml e inline direttamente in azds.yaml. Non è consigliabile archiviare i segreti in values.yaml.
 
## <a name="method-1-valuesdevyaml"></a>Metodo 1: values.dev.yaml
1. Aprire Visual Studio Code con il progetto abilitato per Azure Dev Spaces.
2. Aggiungere un file denominato _values.dev.yaml_ nella stessa cartella del file _values.yaml_ esistente e definire la chiave privata e i valori, come nell'esempio seguente:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. Aggiornare _azds.yaml_ per indicare ad Azure Dev Spaces di usare il nuovo file _values.dev.yaml_. A tale scopo, aggiungere questa configurazione nella sezione configurations.develop.container:

    ```yaml
           container:
             values:
             - "charts/webfrontend/values.dev.yaml"
    ```
 
4. Modificare il codice di servizio in modo che faccia riferimento a questi segreti come variabili di ambiente, come nell'esempio seguente:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Aggiornare i servizi in esecuzione nel cluster con queste modifiche. Nella riga di comando eseguire il comando:

    ```
    azds up
    ```
 
6. (Facoltativo) Dalla riga di comando verificare che siano stati creati questi segreti:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Assicurarsi di aggiungere _values.dev.yaml_ al file con estensione _gitignore_ per evitare di eseguire il commit dei segreti nel controllo del codice sorgente.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Metodo 2: inline direttamente in azds.yaml
1.  In _azds.yaml_ impostare i segreti nella sezione yaml configurations/develop/install. Anche se è possibile immettere i valori dei segreti direttamente in questo punto, non è consigliabile perché _azds.yaml_ è archiviato nel controllo del codice sorgente. Al contrario, aggiungere i segnaposto usando la sintassi "$PLACEHOLDER".

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT_DEV"
                host: "$REDIS_HOST_DEV"
                key: "$REDIS_KEY_DEV"
    ```
     
2.  Creare un file con estensione _env_ nella stessa cartella di _azds.yaml_. Immettere i segreti usando una chiave standard = notazione del valore. Non eseguire il commit del file con estensione _env_ al controllo del codice sorgente (per ometterlo dal controllo del codice sorgente in sistemi di controllo della versione basati su git, aggiungerlo al file con estensione _gitignore_). L'esempio seguente mostra un file con estensione _env_:

    ```
    REDIS_PORT_DEV=3333
    REDIS_HOST_DEV=myredishost
    REDIS_KEY_DEV=myrediskey
    ```
2.  Modificare il codice sorgente del servizio in modo che faccia riferimento a questi segreti nel codice, come nell'esempio seguente:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Aggiornare i servizi in esecuzione nel cluster con queste modifiche. Nella riga di comando eseguire il comando:

    ```
    azds up
    ```

4.  (Facoltativo) Visualizzare i segreti da kubectl:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Passaggi successivi

Con questi metodi, è ora possibile connettersi in modo sicuro a un database o una cache Redis o accedere ai servizi sicuri di Azure.
 