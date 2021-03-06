---
title: "Guida introduttiva: eseguire un'applicazione in Istanze di contenitore di Azure"
description: In questa guida introduttiva si userà l'interfaccia della riga di comando di Azure per distribuire un'applicazione in esecuzione in un contenitore Docker in Istanze di contenitore di Azure
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7db3d9a076fe9ff5b8bbf970705b82a3f0d5ce54
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855664"
---
# <a name="quickstart-run-an-application-in-azure-container-instances"></a>Guida introduttiva: eseguire un'applicazione in Istanze di contenitore di Azure

Le Istanze di contenitore di Azure consentono di eseguire i contenitori Docker in Azure in modo semplice e rapido, senza la necessità di distribuire macchine virtuali o usare una piattaforma di orchestrazione di contenitori completa come Kubernetes. In questa guida introduttiva viene usato il portale di Azure per creare un contenitore in Azure e renderne disponibile l'applicazione con un nome di dominio completo (FQDN). Pochi secondi dopo aver eseguito un comando di distribuzione singola, è possibile passare all'applicazione in esecuzione:

![App distribuita in Istanze di contenitore di Azure visualizzata nel browser][aci-app-browser]

Se non si ha una sottoscrizione di Azure, creare un [account gratuito][azure-account] prima di iniziare.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Per completare questa guida introduttiva è possibile usare Azure Cloud Shell o un'installazione locale dell'interfaccia della riga di comando di Azure. Se si preferisce usare l'interfaccia locale, è necessaria la versione 2.0.27 o successiva. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure][azure-cli-install].

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Le Istanze di contenitore di Azure, analogamente a tutte le risorse di Azure, devono essere distribuite in un gruppo di risorse. I gruppi di risorse consentono di organizzare e gestire le risorse di Azure correlate.

Per iniziare, creare un gruppo di risorse denominato *myResourceGroup* nell'area *eastus* con il comando [az group create][az-group-create] seguente:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Creare un contenitore

Dopo aver creato il gruppo di risorse, è possibile eseguire un contenitore in Azure. Per creare un'istanza di contenitore con l'interfaccia della riga di comando di Azure, specificare il nome del gruppo di risorse, il nome dell'istanza di contenitore e l'immagine del contenitore Docker nel comando [az container create][az-container-create]. È possibile esporre i contenitori in Internet specificando una o più porte da aprire, un'etichetta del nome DNS o entrambi. In questa guida introduttiva si distribuisce un contenitore con un'etichetta del nome DNS che ospita un'app Web di piccole dimensioni scritta in Node.js.

Eseguire il comando seguente per avviare un'istanza di contenitore. Il valore `--dns-name-label` deve essere univoco all'interno dell'area di Azure in cui si crea l'istanza. Se si riceve il messaggio di errore "L'etichetta del nome DNS non è disponibile", provare con un'etichetta del nome DNS diversa.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

Entro pochi secondi si dovrebbe ricevere una risposta dall'interfaccia della riga di comando di Azure che indica che è stata completata la distribuzione. Controllarne lo stato usando il comando [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Quando si esegue il comando, vengono visualizzati il nome di dominio completo (FQDN) del contenitore e il relativo lo stato di provisioning.

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Se il valore `ProvisioningState` del contenitore è **Operazione completata**, passare al relativo indirizzo FQDN nel browser. Se viene visualizzata una pagina Web simile all'immagine seguente, congratulazioni! È stata completata la distribuzione di un'applicazione in esecuzione in un contenitore Docker in Azure.

![Screenshot del browser che mostra l'applicazione in esecuzione in un'istanza di contenitore di Azure][aci-app-browser]

Se inizialmente l'applicazione non viene visualizzata, può essere necessario attendere alcuni secondi il completamento della propagazione DNS, quindi provare ad aggiornare il browser.

## <a name="pull-the-container-logs"></a>Effettuare il pull dei log del contenitore

Quando è necessario risolvere i problemi di un contenitore o dell'applicazione in esecuzione (o semplicemente visualizzarne l'output), iniziare visualizzando i log dell'istanza del contenitore.

Effettuare il pull dei log dell'istanza del contenitore con il comando [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

L'output visualizza i log per il contenitore e dovrebbe mostrare le richieste HTTP GET generate quando l'applicazione è stata visualizzata nel browser.

```console
$ az container logs --resource-group myResourceGroup --name mycontainer
listening on port 80
::ffff:10.240.255.105 - - [01/Oct/2018:18:25:51 +0000] "GET / HTTP/1.0" 200 1663 "-" "-"
::ffff:10.240.255.106 - - [01/Oct/2018:18:31:04 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
::ffff:10.240.255.106 - - [01/Oct/2018:18:31:04 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36"
```

## <a name="attach-output-streams"></a>Collegare i flussi di output

Oltre a visualizzare i log, è possibile collegare i flussi di errore e di output standard locali a quello del contenitore.

Per prima cosa, eseguire il comando [az container attach][az-container-attach] per collegare la console locale ai flussi di output del contenitore:

```azurecli-interactive
az container attach --resource-group myResourceGroup -n mycontainer
```

Al termine, aggiornare il browser alcune volte per generare output aggiuntivo. Scollegare infine la console con `Control+C`. L'output dovrebbe essere simile al seguente:

```console
$ az container attach --resource-group myResourceGroup -n mycontainer
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-15 21:17:59+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Created container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45
(count: 1) (last timestamp: 2018-03-15 21:18:06+00:00) Started container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45

Start streaming logs:
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:44 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:47 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="clean-up-resources"></a>Pulire le risorse

Quando il contenitore non è più necessario, rimuoverlo usando il comando [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Per verificare che il contenitore sia stato eliminato, eseguire il comando [az container list](/cli/azure/container#az-container-list):

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

Il contenitore **mycontainer** non deve essere visualizzato nell'output del comando. Se nel gruppo di risorse non sono presenti altri contenitori, non viene visualizzato alcun output.

Se il gruppo di risorse *myResourceGroup* e tutte le risorse contenute non sono più necessari, eliminarli con il comando [az group delete][az-group-delete]:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva è stata creata un'istanza di contenitore di Azure tramite un'immagine nel registro nell'hub Docker pubblico. Per provare a creare un'immagine del contenitore e a distribuirla da un registro contenitori di Azure privato, passare all'esercitazione su Istanze di contenitore di Azure.

> [!div class="nextstepaction"]
> [Esercitazione su Istanze di contenitore di Azure](./container-instances-tutorial-prepare-app.md)

Per provare le opzioni per l'esecuzione di contenitori in un sistema di orchestrazione in Azure, vedere la guida introduttiva per [Service Fabric][service-fabric] o [Azure Kubernetes Service (AKS)][container-service].

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-container-list]: /cli/azure/container#az-container-list
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
