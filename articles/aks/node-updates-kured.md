---
title: Aggiornare e riavviare i nodi con kured nel servizio Kubernetes di Azure (AKS)
description: Informazioni su come aggiornare e riavviare automaticamente i nodi con kured in nel servizio Kubernetes di Azure (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 11/06/2018
ms.author: iainfou
ms.openlocfilehash: 0bcc49df6540b73b8feb5bb1ec4312e680572797
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617809"
---
# <a name="apply-security-and-kernel-updates-to-nodes-in-azure-kubernetes-service-aks"></a>Applicare aggiornamenti di sicurezza e del kernel ai nodi nel servizio Kubernetes di Azure (AKS)

Per proteggere i cluster, gli aggiornamenti di sicurezza vengono applicati automaticamente ai nodi nel servizio Kubernetes di Azure. Questi aggiornamenti includono correzioni della sicurezza del sistema operativo o gli aggiornamenti del kernel. Alcuni di questi aggiornamenti richiedono il riavvio di un nodo per completare il processo. Il servizio Kubernetes di Azure non riavvia automaticamente i nodi per completare il processo di aggiornamento.

Questo articolo illustra come usare [kured (KUbernetes REboot Daemon)][kured] per controllare i nodi che richiedono un riavvio e gestire automaticamente la ripianificazione dei pod in esecuzione e il processo di riavvio del nodo.

> [!NOTE]
> `Kured` è un progetto open source di Weaveworks. L'assistenza per questo progetto in AKS è fornita con il massimo impegno possibile. È possibile trovare assistenza aggiuntiva nel canale #weave-community di Slack,

## <a name="before-you-begin"></a>Prima di iniziare

Questo articolo presuppone che si disponga di un cluster AKS esistente. Se è necessario un cluster AKS, vedere la Guida introduttiva su AKS [Uso dell'interfaccia della riga di comando di Azure][aks-quickstart-cli] oppure [Uso del portale di Azure][aks-quickstart-portal].

È necessario anche che sia installata e configurata l'interfaccia della riga di comando di Azure versione 2.0.49 o successiva. Eseguire  `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere  [Installare l'interfaccia della riga di comando di Azure][install-azure-cli].

## <a name="understand-the-aks-node-update-experience"></a>Comprendere l'esperienza di aggiornamento del nodo AKS

In un cluster AKS, i nodi Kubernetes vengono eseguiti come macchine virtuali di Azure (VM). Queste macchine virtuali, basate su Linux, usano un'immagine di Ubuntu con il sistema operativo configurato per cercare automaticamente gli aggiornamenti tutte le notti. Se sono disponibili aggiornamenti di sicurezza o del kernel, questi vengono scaricati e installati automaticamente.

![Processo di aggiornamento e riavvio del nodo AKS con kured](media/node-updates-kured/node-reboot-process.png)

Alcuni aggiornamenti di sicurezza, ad esempio gli aggiornamenti del kernel, richiedono un riavvio del nodo per finalizzare il processo. Un nodo che richiede il riavvio crea un file denominato */var/run/reboot-required*. Questo processo di riavvio non avviene automaticamente.

È possibile usare i propri flussi di lavoro e processi per gestire i riavvii del nodo, oppure utilizzare `kured` per dirigere il processo. Con `kured`, viene distribuito un [DaemonSet][DaemonSet] che esegue un pod su ciascun nodo del cluster. Questi pod nel DaemonSet verificano la presenza del file */var/run/reboot-required*, quindi avviano un processo di riavvio dei nodi.

### <a name="node-upgrades"></a>Aggiornamenti del nodo

Esiste un processo aggiuntivo nel servizio Kubernetes di Azure che permette di *aggiornare* un cluster. In genere, un aggiornamento è un passaggio a una versione più recente di Kubernetes, non solo l'applicazione di aggiornamenti di sicurezza del nodo. Un aggiornamento di AKS esegue le azioni seguenti:

* Un nuovo nodo viene distribuito con gli aggiornamenti di sicurezza e la versione di Kubernetes applicati più recenti.
* Un nodo precedente viene chiuso e svuotato.
* I pod vengono pianificati sul nuovo nodo.
* Il nodo precedente viene eliminato.

Non è possibile mantenere la stessa versione di Kubernetes durante un evento di aggiornamento. È necessario specificare una versione più recente di Kubernetes. Per eseguire l'aggiornamento alla versione più recente di Kubernetes, è possibile [aggiornare il cluster AKS][aks-upgrade].

## <a name="deploy-kured-in-an-aks-cluster"></a>Distribuire kured in un cluster AKS

Per distribuire il DaemonSet `kured`, applicare l'esempio di manifesto YAML seguente dalla loro pagina dei progetti di GitHub. Questo manifesto crea un ruolo e un ruolo del cluster, associazioni e un account del servizio, quindi distribuisce il DaemonSet usando la versione 1.1.1.0 di `kured` che supporta i cluster di AKS versione 1.9 o successiva.

```console
kubectl apply -f https://github.com/weaveworks/kured/releases/download/1.1.0/kured-1.1.0.yaml
```

È anche possibile configurare i parametri aggiuntivi per `kured`, ad esempio l'integrazione con Prometheus o Slack. Per altre informazioni sui parametri di configurazione aggiuntivi, consultare la [documentazione di installazione di kured][kured-install].

## <a name="update-cluster-nodes"></a>Aggiornare i nodi del cluster

Per impostazione predefinita, i nodi AKS verificano ogni sera se ci sono aggiornamenti disponibili. Se non si vuole attendere, è possibile eseguire manualmente un aggiornamento per verificare che l'esecuzione di `kured` avvenga correttamente. In primo luogo, seguire la procedura per creare una [connessione SSH a uno dei nodi AKS][aks-ssh]. Dopo aver creato una connessione SSH al nodo, verificare la presenza di aggiornamenti e applicarli come indicato di seguito:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Se sono stati applicati gli aggiornamenti che richiedono un riavvio del nodo, viene scritto un file in */var/run/reboot-required*. `Kured` verifica la presenza di nodi che richiedono un riavvio ogni 60 minuti per impostazione predefinita.

## <a name="monitor-and-review-reboot-process"></a>Monitorare e controllare il processo di riavvio

Quando una delle repliche nel DaemonSet ha rilevato che è necessario un riavvio del nodo, viene inserito un blocco sul nodo tramite l'API di Kubernetes. Questo blocco impedisce l'aggiunta al nodo di altri pod pianificati. Inoltre, il blocco indica che deve essere riavviato solo un nodo alla volta. Con il nodo bloccato, i pod in esecuzione vengono rimossi dal nodo, che viene quindi riavviato.

È possibile monitorare lo stato dei nodi usando il comando [kubectl get nodes][kubectl-get-nodes]. L'output di esempio seguente mostra un nodo con lo stato *SchedulingDisabled* mentre si prepara per il processo di riavvio:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-79590246-2   Ready,SchedulingDisabled   agent     1h        v1.9.11
```

Una volta completato il processo di aggiornamento, è possibile visualizzare lo stato dei nodi usando il comando [kubectl get nodes][kubectl-get-nodes] con il parametro `--output wide`. Questo output aggiuntivo consente di visualizzare una differenza in *KERNEL-VERSION* dei nodi sottostanti, come illustrato nell'output di esempio seguente. *aks-nodepool1-79590246-2* è stato aggiornato in un passaggio precedente e mostra la versione del kernel *4.15.0-1025-azure*. Il nodo *aks-nodepool1-79590246-1* che non è stato aggiornato mostra la versione del kernel *4.15.0-1023-azure*.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-79590246-1   Ready     agent     1h        v1.9.11   10.240.0.6    <none>        Ubuntu 16.04.5 LTS   4.15.0-1023-azure   docker://1.13.1
aks-nodepool1-79590246-2   Ready     agent     1h        v1.9.11   10.240.0.4    <none>        Ubuntu 16.04.5 LTS   4.15.0-1025-azure   docker://1.13.1
```

## <a name="next-steps"></a>Passaggi successivi

Questo articolo ha illustrato come usare `kured` per riavviare automaticamente i nodi come parte del processo di aggiornamento di sicurezza. Per eseguire l'aggiornamento alla versione più recente di Kubernetes, è possibile [aggiornare il cluster AKS][aks-upgrade].

<!-- LINKS - external -->
[kured]: https://github.com/weaveworks/kured
[kured-install]: https://github.com/weaveworks/kured#installation
[kubectl-get-nodes]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[DaemonSet]: concepts-clusters-workloads.md#statefulsets-and-daemonsets
[aks-ssh]: ssh.md
[aks-upgrade]: upgrade-cluster.md
