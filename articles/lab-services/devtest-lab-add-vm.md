---
title: Aggiungere una VM in un lab in Azure DevTest Labs | Microsoft Docs
description: Informazioni su come aggiungere una macchina virtuale in un lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: e86568da7f3d607c90e42e09a61ced9993c4d744
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254044"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Aggiungere una VM in un lab in Azure DevTest Labs
Se è già stata [creata la prima VM](devtest-lab-create-first-vm.md), l'operazione è stata eseguita con un'[immagine di marketplace](devtest-lab-configure-marketplace-images.md) precaricata. Se ora si vuole aggiungere le macchine virtuali successive al lab, è anche possibile scegliere una *base* che sia un'[immagine personalizzata](devtest-lab-create-template.md) o una [formula](devtest-lab-manage-formulas.md). Questa esercitazione illustra l'uso del portale di Azure per aggiungere una VM in un lab in DevTest Labs.

Questo articolo descrive anche come gestire gli elementi per una VM nel lab.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Passaggi per aggiungere una VM a un lab in Azure DevTest Labs
1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selezionare **Tutti i servizi** e quindi **DevTest Labs** nell'elenco.
1. Nell'elenco di lab selezionare il lab in cui si vuole creare la nuova VM.  
1. Nel riquadro **Panoramica** del lab selezionare **+ Aggiungi**.  

    ![Pulsante Aggiungi VM](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Nel riquadro **Scegli una base** selezionare una base per la macchina virtuale.
1. Nel riquadro **Macchina virtuale** il **nome della macchina virtuale** risulta precompilato con un nome univoco generato automaticamente, costituito dal nome utente incluso nell'indirizzo di posta elettronica seguito da un numero univoco a tre cifre. Ciò consente di evitare di perdere tempo a definire un nome per la macchina virtuale e digitarlo ogni volta che se ne crea una. Se si vuole, è possibile sostituire il nome compilato automaticamente con un nome di propria scelta. Per eseguire questa operazione, immettere un nome nella casella di testo **Nome macchina virtuale**. 

    ![Riquadro lab della macchina virtuale](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Il **nome utente** per la macchina viene precompilato con un nome univoco generato automaticamente, costituito dal nome utente incluso nell'indirizzo di posta elettronica. Ciò consente di evitare di perdere tempo a definire un nome utente ogni volta che si crea una nuova macchina. Anche in questo caso, se si vuole, è possibile sostituire il nome utente compilato automaticamente con un nome di propria scelta. Per eseguire questa operazione, immettere un valore nella casella di testo **Nome utente**. A questo utente vengono concessi i privilegi di **amministratore** sulla macchina virtuale.     
1. Per la **password**:
    
    Se si crea la prima macchina virtuale nel lab, immettere una password nella casella di testo **Digitare un valore**. Per salvare questa password come predefinita nell'insieme di credenziali delle chiavi di Azure associato al lab, selezionare **Salva come password predefinita**. La password predefinita viene salvata nell'insieme di credenziali delle chiavi con il nome **VmPassword**. Quando si prova a creare altre macchine virtuali nel lab, **VmPassword** viene selezionata automaticamente come **password**. Per sostituire il valore, deselezionare la casella di controllo **Usa un segreto salvato** e immettere una password. 

    È anche possibile salvare prima un segreto nell'insieme di credenziali delle chiavi e quindi usarlo per creare una macchina virtuale nel lab. Per altre informazioni, vedere [Archiviare segreti in un insieme di credenziali delle chiavi](devtest-lab-store-secrets-in-key-vault.md). Per usare la password archiviata nell'insieme di credenziali delle chiavi, selezionare **Usa un segreto salvato** e specificare un valore di chiave corrispondente al segreto (password). 
3. Il **tipo di disco della macchina virtuale** determina il tipo di disco di archiviazione consentito per le macchine virtuali nel lab.
4. Selezionare **Dimensioni macchina virtuale** e uno degli elementi predefiniti che specificano le memorie centrali del processore, la dimensione della RAM e le dimensioni dell'unità disco rigido della VM da creare.
5. Selezionare **Elementi** e dall'elenco di elementi selezionare e configurare gli elementi da aggiungere all'immagine di base.
    **Nota:** se non si ha familiarità con DevTest Labs o con la configurazione di elementi, vedere la sezione [Aggiungere un elemento esistente in una macchina virtuale](#add-an-existing-artifact-to-a-vm) e tornare qui al termine dell'operazione.
6. Selezionare **Impostazioni avanzate** per configurare le opzioni di rete e le opzioni relative alla scadenza della VM. 

   Per impostare un'opzione di scadenza, scegliere l'icona del calendario per specificare una data in cui la macchina virtuale verrà eliminata automaticamente.  Per impostazione predefinita, la macchina virtuale non scadrà. 
1. Se si vuole visualizzare o copiare il modello di Azure Resource Manager, vedere la sezione [Salvare il modello di Azure Resource Manager](#save-azure-resource-manager-template) e tornare qui al termine dell'operazione.
1. Selezionare **Crea** per aggiungere la macchina virtuale specificata al lab.
1. Nel riquadro del lab viene visualizzato lo stato di creazione della macchina virtuale, prima come **Creazione** e poi come **Esecuzione** dopo l'avvio della macchina virtuale.

> [!NOTE]
> L'argomento [Add a claimable VM](devtest-lab-add-claimable-vm.md) (Aggiungere una macchina virtuale a disposizione degli utenti) illustra come rendere disponibile una macchina virtuale per l'uso da parte di qualsiasi utente nel laboratorio.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Aggiungere un elemento esistente in una macchina virtuale
Durante la creazione di una macchina virtuale, è possibile aggiungere elementi esistenti. Ogni lab include elementi del repository pubblico degli elementi di DevTest Labs ed elementi creati e aggiunti al repository degli elementi personale.

* Gli *elementi* di Azure DevTest Labs consentono di specificare le *azioni* eseguite quando viene eseguito il provisioning della macchina virtuale, ad esempio, l'esecuzione di script Windows PowerShell, comandi Bash e installazione del software.
* Gli elementi *parametri* consentono di personalizzare l'elemento per un determinato scenario

Per istruzioni sulla creazione di elementi, vedere l'articolo contenente informazioni su [Creare elementi personalizzati per le macchine virtuali di DevTest Labs](devtest-lab-artifact-author.md).

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selezionare **Tutti i servizi** e quindi **DevTest Labs** nell'elenco.
1. Nell'elenco di lab selezionare il lab contenente la VM che si vuole usare.  
1. Selezionare **Macchine virtuali**.
1. Selezionare la VM desiderata.
1. Selezionare **Gestisci gli artefatti**. 
1. Selezionare **Apply artifacts** (Applica elementi).
1. In **Applica artefatti** selezionare l'artefatto da aggiungere alla macchina virtuale.
1. Nel riquadro **Aggiungi artefatto** immettere i valori dei parametri obbligatori e gli eventuali parametri facoltativi necessari.  
1. Selezionare **Aggiungi** per aggiungere l'artefatto e tornare al riquadro **Applica artefatti**.
1. Continuare ad aggiungere gli elementi necessari per la macchina virtuale.
1. Dopo aver aggiunto gli elementi, è possibile [modificare l'ordine in cui vengono eseguiti](#change-the-order-in-which-artifacts-are-run). È anche possibile tornare indietro per [visualizzare o modificare un elemento](#view-or-modify-an-artifact).
1. Una volta aggiunti tutti gli elementi, selezionare **Applica**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Modificare l'ordine di esecuzione degli elementi
Per impostazione predefinita, le azioni degli elementi vengono eseguite nell'ordine in cui vengono aggiunte alla macchina virtuale. La procedura seguente descrive come modificare l'ordine in cui vengono eseguiti gli elementi.

1. Nella parte superiore del riquadro **Applica artefatti** selezionare il collegamento che indica il numero di artefatti aggiunti alla macchina virtuale.
   
    ![Numero di elementi aggiunti alla macchina virtuale](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Nel riquadro **Artefatti selezionati** trascinare e rilasciare gli artefatti nell'ordine desiderato. **Nota:** se si verificano problemi di trascinamento dell'elemento, assicurarsi di trascinare dal lato sinistro dell'elemento. 
1. Selezionare **OK** al termine dell'operazione.  

## <a name="view-or-modify-an-artifact"></a>visualizzare o modificare un elemento
I passaggi seguenti illustrano come visualizzare o modificare i parametri di un elemento:

1. Nella parte superiore del riquadro **Applica artefatti** selezionare il collegamento che indica il numero di artefatti aggiunti alla macchina virtuale.
   
    ![Numero di elementi aggiunti alla macchina virtuale](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Nel riquadro **Artefatti selezionati** selezionare l'artefatto che si vuole visualizzare o modificare.  
1. Nel riquadro **Aggiungi artefatto** apportare le modifiche necessarie e selezionare **OK** per chiudere il riquadro **Aggiungi artefatto**.
1. Selezionare **OK** per chiudere il riquadro **Artefatti selezionati**.

## <a name="save-azure-resource-manager-template"></a>Save Azure Resource Manager template
Un modello di Azure Resource Manager permette di definire una distribuzione ripetibile in modo dichiarativo. I passaggi seguenti illustrano come salvare il modello di Azure Resource Manager per la VM da creare.
Dopo il salvataggio è possibile usare il modello di Azure Resource Manager per [distribuire nuove macchine virtuali con Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Nel riquadro **Macchina virtuale** selezionare **Visualizza il modello di Azure Resource Manager**.
2. Nel riquadro **Visualizza il modello di Azure Resource Manager** selezionare il testo del modello.
3. Copiare il testo selezionato negli Appunti.
4. Selezionare **OK** per chiudere il riquadro **Visualizza il modello di Azure Resource Manager**.
5. Aprire un editor di testo.
6. Incollare il testo del modello dagli Appunti.
7. Salvare il file per usarlo in seguito.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Passaggi successivi
* Dopo avere creato la macchina virtuale, è possibile connettersi ad essa selezionando **Connetti** nel riquadro della macchina virtuale.
* Informazioni su come [creare elementi personalizzati per la VM di DevTest Labs](devtest-lab-artifact-author.md).
* Esplorare la [raccolta dei modelli di avvio rapido di Azure Resource Manager di DevTest Labs](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
