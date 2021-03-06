---
title: Visualizzare i log del controller del servizio Kubernetes di Azure
description: Informazioni su come abilitare e visualizzare i log per il nodo master di Kubernetes nel servizio Kubernetes di Azure
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/25/2018
ms.author: iainfou
ms.openlocfilehash: 5f151db9cb2cf7a5ef242f777cd2ad1820c856a8
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636687"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Abilitare e controllare i log del nodo master di Kubernetes nel servizio Kubernetes di Azure

Con il servizio Kubernetes di Azure, i componenti master, ad esempio *kube-apiserver* e *kube-controller-manager* vengono forniti come servizio gestito. Si creano e si gestiscono i nodi che eseguono *kubelet* e il runtime del contenitore e si distribuiscono le applicazioni attraverso il server API Kubernetes gestito. Per facilitare la risoluzione dei problemi dell'applicazione e dei servizi, potrebbe essere necessario visualizzare i log generati da tali componenti master. Questo articolo illustra come usare Azure Log Analytics per abilitare i log ed eseguire query dai componenti master di Kubernetes.

## <a name="before-you-begin"></a>Prima di iniziare

Questo articolo richiede un cluster del servizio Kubernetes di Azure esistente in esecuzione nel proprio account di Azure. Se non si dispone ancora di un cluster del servizio Kubernetes di Azure, crearne uno usando l'[interfaccia della riga di comando di Azure] [ cli-quickstart] oppure il [portale di Azure][portal-quickstart]. Log Analytics funziona con cluster del servizio Kubernetes di Azure abilitati per RBAC e non RBAC.

## <a name="enable-diagnostics-logs"></a>Abilitare la registrazione diagnostica

Per raccogliere e rivedere i dati da più origini, Log Analytics fornisce un linguaggio di query e un motore di analisi che offre informazioni dettagliate per l'ambiente in uso. Viene usata un'area di lavoro per collazionare e analizzare i dati che possa integrarsi con altri servizi di Azure, ad esempio Application Insights e Centro sicurezza. Per usare una piattaforma diversa per analizzare i log, è possibile scegliere di inviare i log di diagnostica a un account di archiviazione di Azure o a un hub eventi. Per altre informazioni, vedere [Informazioni su Azure Log Analytics][log-analytics-overview].

Log Analytics è abilitato e gestito nel portale di Azure. Per abilitare la raccolta dei log per i componenti master di Kubernetes nel cluster del servizio Kubernetes di Azure (AKS), aprire il portale di Azure in un Web browser e completare i passaggi seguenti:

1. Selezionare il gruppo di risorse per il cluster AKS, ad esempio *myResourceGroup*. Non selezionare il gruppo di risorse che contiene le singole risorse del cluster AKS, ad esempio *MC_myResourceGroup_myAKSCluster_eastus*.
1. Sul lato sinistro, scegliere **Impostazioni di diagnostica**.
1. Selezionare il cluster AKS, ad esempio *myAKSCluster*, quindi scegliere **Abilita diagnostica**.
1. Immettere un nome, ad esempio *myAKSLogs*, quindi selezionare l'opzione **Invia a Log Analytics**.
    * Scegliere *Configura* in Log Analytics, quindi selezionare un'area di lavoro esistente o **creare una nuova area di lavoro**.
    * Se è necessario creare un'area di lavoro, specificare un nome, un gruppo di risorse e un percorso.
1. Nell'elenco dei log disponibili selezionare i log che si vuole abilitare, ad esempio *kube-apiserver*, *kube-controller-manager* e *kube-scheduler*. È possibile restituire e modificare i log raccolti dopo l'abilitazione del Log Analytics.
1. Quando si è pronti, selezionare **Salva** per abilitare la raccolta dei log selezionati.

Lo screenshot di esempio del portale seguente mostra la finestra *Impostazioni di diagnostica* e l'opzione per creare un'area di lavoro di Log Analytics:

![Abilitare l'area di lavoro di Log Analytics per il cluster AKS in Log Analytics](media/view-master-logs/enable-oms-log-analytics.png)

>[!NOTE]
>Le aree di lavoro OMS sono ora denominate aree di lavoro di Log Analytics.

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>Pianificare un pod di test nel cluster AKS

Per generare alcuni log, creare un nuovo pod nel cluster AKS. Il seguente esempio di manifesto YAML può essere usato per creare un'istanza NGINX di base. Creare un file denominato `nginx.yaml` in un editor a scelta e incollare il contenuto seguente:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    ports:
    - containerPort: 80
```

Creare il pod con il comando [kubectl create][kubectl-create] e specificare il file YAML, come illustrato nell'esempio seguente:

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Visualizzare i log raccolti

Potrebbero occorrere alcuni minuti prima che i log di diagnostica siano abilitati e vengano visualizzati nell'area di lavoro di Log Analytics. Nel portale di Azure selezionare il gruppo di risorse per l'area di lavoro di Log Analytics, ad esempio *myResourceGroup*, quindi scegliere la risorsa di Log Analytics, ad esempio *myAKSLogs*.

![Selezionare l'area di lavoro di Log Analytics per il cluster AKS](media/view-master-logs/select-log-analytics-workspace.png)

Sul lato sinistro scegliere **Log**. Per visualizzare *kube-apiserver*, immettere la query seguente nella casella di testo:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| project log_s
```

Probabilmente vengono restituiti molti log per il server API. Per limitare i risultati della query in modo da visualizzare i log sul pod NGINX creato nel passaggio precedente, aggiungere un'altra istruzione *where* per cercare *pods/nginx* come illustrato nella query di esempio seguente:

```
AzureDiagnostics
| where Category == "kube-apiserver"
| where log_s contains "pods/nginx"
| project log_s
```

Vengono visualizzati i log specifici per il pod NGINX, come illustrato nello screenshot di esempio seguente:

![Registrare i risultati della query di Log Analytics per il pod NGINX di esempio](media/view-master-logs/log-analytics-query-results.png)

Per visualizzare altri log, è possibile aggiornare la query per il nome di *Categoria* con *kube-controller-manager* oppure *kube-scheduler*, a seconda dei log aggiuntivi che si vuole abilitare. È possibile usare ulteriori istruzioni *where* per definire gli eventi da cercare.

Per altre informazioni su come eseguire una query e filtrare i dati dei log, vedere [Visualizzare o analizzare i dati raccolti con la ricerca log di Log Analytics][analyze-log-analytics].

## <a name="log-event-schema"></a>Schema di eventi del log

Per semplificare l'analisi dei dati dei log, la tabella seguente illustra lo schema utilizzato per ogni evento:

| Nome campo               | DESCRIZIONE |
|--------------------------|-------------|
| *resourceId*             | Risorsa di Azure che ha generato il log |
| *time*                   | Timestamp di quando è stato caricato il log |
| *category*               | Nome del contenitore o componente che ha generato il log |
| *operationName*          | Sempre *Microsoft.ContainerService/managedClusters/diagnosticLogs/Read* |
| *properties.log*         | Query full-text del log dal componente |
| *properties.stream*      | *stderr* o *stdout* |
| *properties.pod*         | Nome del pod da cui proviene il log |
| *properties.containerID* | ID del contenitore Docker da cui proviene questo log |

## <a name="next-steps"></a>Passaggi successivi

In questo articolo è stato descritto come abilitare e analizzare i log per i componenti master di Kubernetes nel cluster AKS. Per monitorare ulteriormente e risolvere eventuali problemi, è anche possibile [visualizzare i log di Kubelet] [ kubelet-logs] e [abilitare l'accesso ai nodi SSH][aks-ssh].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../log-analytics/log-analytics-overview.md
[analyze-log-analytics]: ../log-analytics/log-analytics-tutorial-viewdata.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
