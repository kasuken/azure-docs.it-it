---
title: Usare i propri certificati TLS per l'ingresso con cluster del servizio Kubernetes di Azure (AKS)
description: Informazioni su come installare e configurare un controller di ingresso NGINX che usa i propri certificati in un cluster del servizio Kubernetes di Azure (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/23/2018
ms.author: iainfou
ms.openlocfilehash: 4e3f2f33cfffeacbcbeccc4f17f55b7d0e1a985c
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50129166"
---
# <a name="create-an-https-ingress-controller-and-use-your-own-tls-certificates-on-azure-kubernetes-service-aks"></a>Creare un controller di ingresso HTTPS e usare i propri certificati TLS in servizio Kubernetes di Azure (AKS)

Un controller di ingresso è un componente software che fornisce proxy inverso, routing del traffico configurabile e terminazione TLS per i servizi Kubernetes. Le risorse di ingresso Kubernetes vengono usate per configurare le regole di ingresso e le route per i singoli servizi Kubernetes. Usando un controller di ingresso e regole di ingresso è possibile servirsi di un singolo indirizzo IP per instradare il traffico a più servizi in un cluster Kubernetes.

Questo articolo illustra come distribuire il [controller di ingresso NGINX][nginx-ingress] in un cluster del servizio Kubernetes di Azure (AKS). Si generano i propri certificati e si crea un segreto Kubernetes per l'utilizzo con la route in ingresso. Infine, due applicazioni vengono eseguite nel cluster AKS, ognuna delle quali è accessibile tramite un singolo indirizzo IP.

È anche possibile:

- [Creare un controller di ingresso di base con connettività di rete esterna][aks-ingress-basic]
- [Abilitare il componente aggiuntivo di routing dell'applicazione HTTP][aks-http-app-routing]
- [Creare un controller di ingresso che usa una rete privata interna e l'indirizzo IP][aks-ingress-internal]
- Creare un controller di ingresso che usa Let's Encrypt per generare automaticamente certificati TLS [con un indirizzo IP pubblico dinamico][aks-ingress-tls] o [con un indirizzo IP pubblico statico][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Prima di iniziare

Questo articolo usa Helm per installare il controller di ingresso NGINX e un'app Web di esempio. È necessario disporre di Helm inizializzato nel cluster AKS e usare un account del servizio per Tiller. Assicurarsi di usare l'ultima versione di Helm. Per istruzioni sull'aggiornamento, consultare [Installare i documenti Helm][helm-install]. Per altre informazioni sulla configurazione e l'uso di Helm, vedere [Installare le applicazioni con Helm nel servizio Kubernetes di Azure (AKS)][use-helm].

Questo articolo richiede anche che sia in esecuzione l'interfaccia della riga di comando di Azure versione 2.0.41 o successive. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Creare un controller di ingresso

Per creare il controller di ingresso, usare `Helm` per installare *Ingresso nginx*. Per maggiore ridondanza, vengono distribuite due repliche dei controller di ingresso NGINX con il parametro `--set controller.replicaCount`. Per sfruttare appieno le repliche del controller di ingresso in esecuzione, assicurarsi che nel cluster AKS siano presenti più nodi.

> [!TIP]
> L'esempio seguente illustra come installare il controller di ingresso nello spazio dei nomi `kube-system`. Se lo si desidera, è possibile specificare uno spazio dei nomi diverso per il proprio ambiente. Se il cluster AKS non dispone dell'abilitazione RBAC, aggiungere `--set rbac.create=false` ai comandi.

```console
helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2
```

Durante l'installazione viene creato un indirizzo IP pubblico di Azure per il controller di ingresso. Questo indirizzo IP pubblico è statico per la durata di vita del controller di ingresso. Se si elimina il controller di ingresso, l'assegnazione dell'indirizzo IP pubblico viene persa. Se si crea quindi un controller di ingresso aggiuntivo, viene assegnato un nuovo indirizzo IP pubblico. Se si vuole mantenere l'uso dell'indirizzo IP pubblico, è possibile [creare un controller di ingresso con un indirizzo IP pubblico statico][aks-ingress-static-tls].

Per ottenere l'indirizzo IP pubblico, usare il comando `kubectl get service`. Per l'assegnazione dell'indirizzo IP al servizio, sono necessari pochi minuti.

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                          TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
virulent-seal-nginx-ingress-controller        LoadBalancer   10.0.48.240   40.87.46.190   80:31159/TCP,443:30657/TCP   7m
virulent-seal-nginx-ingress-default-backend   ClusterIP      10.0.50.5     <none>         80/TCP                       7m
```

Prendere nota di questo indirizzo IP pubblico, perché è usato nell'ultimo passaggio per testare la distribuzione.

Non è ancora stata creata alcuna regola di ingresso. Se si passa all'indirizzo IP pubblico, viene visualizzata la pagina predefinita 404 del controller di ingresso NGINX.

## <a name="generate-tls-certificates"></a>Generare i certificati TLS

Per questo articolo, è necessario generare un certificato autofirmato con `openssl`. Per l'uso in produzione è necessario richiedere un certificato attendibile firmato mediante un provider o la propria autorità di certificazione (CA). Nel passaggio successivo, si genera un *segreto* Kubernetes usando il certificato TLS e la chiave privata generati da OpenSSL.

L'esempio seguente genera un certificato RSA X509 a 2048 bit valido per 365 giorni denominato *aks-ingress-tls.crt*. Il file della chiave privata è denominato *aks-ingress-tls.key*. Un segreto Kubernetes TLS richiede entrambi i file.

Questo articolo funziona con il nome comune soggetto *demo.azure.com* e non deve necessariamente essere modificato. Per l'uso in produzione, specificare i propri valori organizzativi per il `-subj` parametro:

```console
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -out aks-ingress-tls.crt \
    -keyout aks-ingress-tls.key \
    -subj "/CN=demo.azure.com/O=aks-ingress-tls"
```

## <a name="create-kubernetes-secret-for-the-tls-certificate"></a>Creare un segreto Kubernetes per il certificato TLS

Per consentire a Kubernetes di usare il certificato TLS e la chiave privata per il controller di ingresso, creare e usare un segreto. Il segreto viene definito una sola volta e usa il certificato e il file di chiave creati nel passaggio precedente. Si fa quindi riferimento a questo segreto quando si definiscono le route in ingresso.

L'esempio seguente crea un nome segreto *aks-ingress-tls*:

```console
kubectl create secret tls aks-ingress-tls \
    --key aks-ingress-tls.key \
    --cert aks-ingress-tls.crt
```

## <a name="run-demo-applications"></a>Eseguire applicazioni demo

Sono stati configurati un controller di ingresso e un segreto con il certificato dell'utente. A questo punto è possibile eseguire due applicazioni demo nel cluster AKS. In questo esempio Helm viene usato per distribuire due istanze di una semplice applicazione "Hello world".

Prima di installare i grafici Helm di esempio, aggiungere il repository degli esempi di Azure all'ambiente Helm come indicato di seguito:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Creare la prima applicazione demo da un grafico Helm con il comando seguente:

```console
helm install azure-samples/aks-helloworld
```

Installare ora una seconda istanza dell'applicazione demo. Specificare un nuovo titolo per la seconda istanza in modo che le due applicazioni siano visivamente distinte. È anche possibile specificare un nome di servizio univoco:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Creare una route in ingresso

Entrambe le applicazioni sono ora in esecuzione nel cluster Kubernetes, anche se sono configurate con un servizio di tipo `ClusterIP`. Di conseguenza, le applicazioni non sono accessibili da Internet. Per renderle disponibili pubblicamente, creare una risorsa di ingresso Kubernetes. La risorsa di ingresso configura le regole che instradano il traffico a una delle due applicazioni.

Nell'esempio seguente il traffico verso l'indirizzo `https://demo.azure.com/` viene instradato al servizio denominato `aks-helloworld`. Il traffico verso l'indirizzo `https://demo.azure.com/hello-world-two` viene instradato al servizio `ingress-demo`. In questo articolo non è necessario modificare i nomi host di demo. Per l'uso in produzione, fornire i nomi specificati come parte del processo di richiesta e generazione del certificato.

> [!TIP]
> Se il nome host specificato durante il processo di richiesta del certificato, il nome CN, non corrisponde all'host definito nella route di ingresso, il controller di ingresso visualizza un *Certificato finto del controller di ingresso Kubernetes*. Assicurarsi che i nomi host del certificato e della route di ingresso corrispondano.

La sezione *tls* indica la route di ingresso per la quale usare il segreto denominato *aks-ingress-tls* per l'host *demo.azure.com*. Anche in questo caso, per l'uso in produzione specificare il proprio indirizzo host.

Creare un file denominato `hello-world-ingress.yaml` e copiarlo nell'esempio YAML seguente.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo.azure.com
    secretName: aks-ingress-tls
  rules:
  - host: demo.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Creare la risorsa di ingresso con il comando `kubectl apply -f hello-world-ingress.yaml`.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-configuration"></a>Testare la configurazione di ingresso

Per testare i certificati con il nostro host finto *demo.azure.com*, usare `curl` e specificare il parametro *--risolvere*. Questo parametro consente di eseguire il mapping del nome *demo.azure.com* per l'indirizzo IP pubblico del controller di ingresso. Specificare l'indirizzo IP pubblico del proprio controller di ingresso, come illustrato nell'esempio seguente:

```
curl -v -k --resolve demo.azure.com:443:40.87.46.190 https://demo.azure.com
```

Non è stato fornito nessun percorso aggiuntivo con l'indirizzo, pertanto, il controller di ingresso per impostazione predefinita viene instradato verso la route */*. Viene restituita la prima applicazione demo, come illustrato nell'output di esempio condensato seguente:

```
$ curl -v -k --resolve demo.azure.com:443:40.87.46.190 https://demo.azure.com

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

Il parametro *- v* nel nostro comando `curl` restituisce informazioni dettagliate, tra cui il certificato TLS ricevuto. A metà dell'output di curl, è possibile verificare che sia stato usato il proprio certificato TLS. Il parametro *-k* continua il caricamento della pagina anche se viene usato un certificato autofirmato. L'esempio seguente mostra che è stato usato il certificato *autorità di certificazione: CN=demo.azure.com; O=aks-ingress-tls*:

```
[...]
* Server certificate:
*  subject: CN=demo.azure.com; O=aks-ingress-tls
*  start date: Oct 22 22:13:54 2018 GMT
*  expire date: Oct 22 22:13:54 2019 GMT
*  issuer: CN=demo.azure.com; O=aks-ingress-tls
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
[...]
```

A questo punto aggiungere il percorso */hello-world-two* all'indirizzo, ad esempio *https://demo.azure.com/hello-world-two*. Viene restituita la seconda applicazione demo con titolo personalizzato, come illustrato nell'output di esempio condensato seguente:

```
$ curl -v -k --resolve demo.azure.com:443:137.117.36.18 https://demo.azure.com/hello-world-two

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>AKS Ingress Demo</title>
[...]
```

## <a name="clean-up-resources"></a>Pulire le risorse

Questo articolo ha usato Helm per installare i componenti di ingresso e le app di esempio. Quando si distribuisce un grafico Helm, viene creato un certo numero di risorse di Kubernetes. Queste risorse includono pod, distribuzioni e servizi. Per eseguire la pulizia, elencare prima le versioni di Helm con il comando `helm list`. Cercare i grafici denominati *nginx-ingress* e *aks-helloworld*, come illustrato nell'output di esempio seguente:

```
$ helm list

NAME            REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
virulent-seal   1           Tue Oct 23 16:37:24 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
billowing-guppy 1           Tue Oct 23 16:41:38 2018    DEPLOYED    aks-helloworld-0.1.0                default
listless-quokka 1           Tue Oct 23 16:41:30 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Eliminare le versioni con il comando `helm delete`. L'esempio seguente elimina la distribuzione di ingresso NGINX e le due app AKS hello world di esempio.

```
$ helm delete virulent-seal billowing-guppy listless-quokka

release "virulent-seal" deleted
release "billowing-guppy" deleted
release "listless-quokka" deleted
```

Rimuovere quindi il repository Helm per le app AKS hello world:

```console
helm repo remove azure-samples
```

Rimuovere la route in ingresso che ha indirizzato il traffico verso le app di esempio:

```console
kubectl delete -f hello-world-ingress.yaml
```

Infine, rimuovere il certificato segreto:

```console
kubectl delete secret aks-ingress-tls
```

## <a name="next-steps"></a>Passaggi successivi

In questo articolo sono stati inclusi alcuni componenti esterni ad AKS. Per altre informazioni su questi componenti, vedere le pagine di progetto seguenti:

- [Interfaccia della riga di comando di Helm][helm-cli]
- [Controller di ingresso NGINX][nginx-ingress]

È anche possibile:

- [Creare un controller di ingresso di base con connettività di rete esterna][aks-ingress-basic]
- [Abilitare il componente aggiuntivo di routing dell'applicazione HTTP][aks-http-app-routing]
- [Creare un controller di ingresso che usa una rete privata interna e l'indirizzo IP][aks-ingress-internal]
- Creare un controller di ingresso che usa Let's Encrypt per generare automaticamente certificati TLS [con un indirizzo IP pubblico dinamico][aks-ingress-tls] o [con un indirizzo IP pubblico statico][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-tls]: ingress-tls.md
