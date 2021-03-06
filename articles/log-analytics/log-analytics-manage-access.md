---
title: Gestire aree di lavoro in Azure Log Analytics e nel portale di OMS | Microsoft Docs
description: È possibile gestire le aree di lavoro in Log Analytics di Azure e nel portale di OMS usando svariate attività amministrative per utenti, account, aree di lavoro e account di Azure.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: de464cfeca01e492139e8bf9679d8f9876eedda6
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51625624"
---
# <a name="manage-workspaces"></a>Gestire le aree di lavoro

Per gestire l'accesso a Log Analytics, vengono eseguite diverse attività amministrative relative alle aree di lavoro. Questo articolo contiene suggerimenti e procedure per gestire le aree di lavoro. Un'area di lavoro è sostanzialmente un contenitore che include informazioni sull'account e semplici informazioni di configurazione per l'account stesso. Nell'organizzazione è possibile usare più aree di lavoro per gestire diversi set di dati raccolti dall'intera infrastruttura IT o da una parte di essa.

Per creare un'area di lavoro, è necessario:

1. Disporre di una sottoscrizione di Azure
2. Scegliere un nome per l'area di lavoro
3. Associare l'area di lavoro con uno degli abbonamenti e dei gruppi di risorse.
4. Scegliere una località geografica

## <a name="determine-the-number-of-workspaces-you-need"></a>Determinare il numero di aree di lavoro necessarie
Un'area di lavoro è una risorsa di Azure e rappresenta un contenitore in cui i dati vengono, aggregati, analizzati e presentati nel portale di Azure.

È possibile avere più aree di lavoro per ogni sottoscrizione di Azure e avere accesso a più di un'area di lavoro, con la possibilità di eseguire facilmente query al loro interno. Questa sezione descrive quando può essere utile creare più aree di lavoro.

Oggi, un'area di lavoro fornisce:

* Una posizione geografica per archiviare i dati
* Isolamento dei dati per definire diritti di accesso utente diversi
* Ambito per la configurazione delle impostazioni, ad esempio conservazione ed estremità di chiusura dei dati

Dal punto di vista del consumo, è consigliabile creare meno aree di lavoro possibile. In questo modo le operazioni di amministrazione e di esecuzione di query sono più semplici e più rapide. Tuttavia, in base alle caratteristiche precedenti, si possono creare più aree di lavoro se:

* Si opera in un'azienda globale ed è necessario che i dati siano archiviati in aree specifiche per motivi di sovranità o conformità.
* Si usa Azure e si intendono evitare costi di trasferimento dei dati in uscita tramite un'area di lavoro nella stessa area delle risorse di Azure da essa gestite.
* Si intende allocare le spese a reparti o gruppi aziendali diversi in base all'uso tramite la creazione di un'area di lavoro per ogni reparto o gruppo aziendale nella relativa sottoscrizione di Azure.
* Si opera come provider di servizi gestiti e per ogni cliente gestito è necessario mantenere i dati di Log Analytics isolati da altri dati del cliente.
* Si gestiscono più clienti e si vuole che ogni cliente, reparto o gruppo aziendale visualizzi i propri dati, ma non quelli di altri.

Quando si usano agenti Windows per la raccolta dei dati, è possibile [configurare ogni agente in modo che faccia riferimento a una o più aree di lavoro](log-analytics-agent-windows.md).

Se si usa System Center Operations Manager, ogni gruppo di gestione di Operations Manager può essere connesso con una sola area di lavoro. È possibile installare Microsoft Monitoring Agent nei computer gestiti da Operations Manager e fare sì che l’agente faccia riferimento sia a Operations Manager che a un'altra area di lavoro di Log Analytics.

## <a name="workspace-information"></a>Informazioni sull'area di lavoro

È possibile visualizzare i dettagli sull'area di lavoro nel portale di Azure. 

1. Accedere al [portale di Azure](https://portal.azure.com), se questa operazione non è già stata eseguita.

2. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Log Analytics**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analytics**.  

    ![Portale di Azure](media/log-analytics-manage-access/azure-portal-01.png)  

3. Nella pagina delle sottoscrizioni di Log Analytics selezionare un'area di lavoro.

4. In tale pagina vengono visualizzati i dettagli su come iniziare, sulla configurazione e sui collegamenti per ottenere altre informazioni.  

    ![Dettagli dell'area di lavoro](./media/log-analytics-manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Gestire utenti e account
A ogni area di lavoro possono essere associati più account, ognuno dei quali può avere accesso a diverse aree di lavoro. L'accesso viene gestito tramite [accesso basato sui ruoli Azure](../role-based-access-control/role-assignments-portal.md). Questi diritti di accesso si applicano sia nel portale di Azure sia sull'accesso all'API.


Le attività seguenti richiedono anche le autorizzazioni di Azure:

| Azione                                                          | Autorizzazioni di Azure necessarie | Note |
|-----------------------------------------------------------------|--------------------------|-------|
| Aggiunta e rimozione di soluzioni di gestione                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Queste autorizzazioni devono essere concesse a livello di gruppo di risorse o di sottoscrizione. |
| Modifica del piano tariffario                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Visualizzazione dei dati nei riquadri delle soluzioni *Backup* e *Site Recovery* | Amministratore/Coamministratore | Risorse di accesso distribuite usando il modello di distribuzione classica |
| Creare un'area di lavoro nel portale di Azure                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Gestione dell'accesso a Log Analytics con le autorizzazioni di Azure
Per concedere l'accesso all'area di lavoro di Log Analytics usando le autorizzazioni di Azure, seguire i passaggi indicati in [Usare le assegnazioni di ruolo per gestire l'accesso alle risorse della sottoscrizione di Azure](../role-based-access-control/role-assignments-portal.md).

Azure offre due ruoli utente predefiniti per Log Analytics:
- Lettore di Log Analytics
- Collaboratore di Log Analytics

I membri del ruolo *Lettore di Log Analytics* possono eseguire queste operazioni:
- Visualizzare e cercare tutti i dati di monitoraggio 
- Visualizzare le impostazioni di monitoraggio, inclusa la configurazione della diagnostica di Azure in tutte le risorse di Azure.

Il ruolo di lettore di Log Analytics include le azioni di Azure seguenti:

| type    | Autorizzazione | DESCRIZIONE |
| ------- | ---------- | ----------- |
| Azione | `*/read`   | Consente di visualizzare tutte le risorse di Azure e la configurazione delle risorse. Include la visualizzazione di: <br> Stato dell'estensione macchina virtuale <br> Configurazione della diagnostica di Azure nelle risorse <br> Tutte le proprietà e le impostazioni di tutte le risorse |
| Azione | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Consente di eseguire query di ricerca log versione 2 |
| Azione | `Microsoft.OperationalInsights/workspaces/search/action` | Consente di eseguire query di ricerca log versione 1 |
| Azione | `Microsoft.Support/*` | Consente di aprire casi di supporto |
|Non azione | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Impedisce la lettura della chiave dell'area di lavoro, necessaria per l'uso dell'API di raccolta dati e per l'installazione degli agenti. Ciò impedisce all'utente di aggiungere nuove risorse all'area di lavoro |


I membri del ruolo *Collaboratore di Log Analytics* possono eseguire queste operazioni:
- Leggere tutti i dati di monitoraggio come Lettore di Log Analytics può  
- Creare e configurare account di automazione  
- Aggiunta e rimozione di soluzioni di gestione    
    > [!NOTE] 
    > Per eseguire correttamente le ultime due azioni, questa autorizzazione deve essere concessa a livello di gruppo di risorse o di abbonamento.  

- Leggere le chiavi degli account di archiviazione   
- Configurare la raccolta di log da Archiviazione di Azure  
- Modificare le impostazioni di monitoraggio per le risorse di Azure, tra cui
  - Aggiunta dell'estensione macchina virtuale alle VM
  - Configurazione della diagnostica di Azure in tutte le risorse di Azure

> [!NOTE] 
> È possibile usare la possibilità di aggiungere un'estensione macchina virtuale a una VM per ottenere il controllo completo su di essa.

Il ruolo di collaboratore di Log Analytics include le azioni di Azure seguenti:

| Autorizzazione | DESCRIZIONE |
| ---------- | ----------- |
| `*/read`     | Consente di visualizzare tutte le risorse e le configurazioni delle risorse. Include la visualizzazione di: <br> Stato dell'estensione macchina virtuale <br> Configurazione della diagnostica di Azure nelle risorse <br> Tutte le proprietà e le impostazioni di tutte le risorse |
| `Microsoft.Automation/automationAccounts/*` | Consente di creare e configurare account di automazione di Azure, inclusa l'aggiunta e modifica di runbook |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Consente di aggiungere, aggiornare e rimuovere estensioni macchina virtuale, inclusa l'estensione Microsoft Monitoring Agent e l'estensione agente OMS per Linux |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Consente di visualizzare la chiave dell'account di archiviazione, è necessaria per configurare Log Analytics per la lettura dei log dagli account di archiviazione di Azure |
| `Microsoft.Insights/alertRules/*` | Consente di aggiungere, aggiornare e rimuovere regole di avviso |
| `Microsoft.Insights/diagnosticSettings/*` | Consente di aggiungere, aggiornare e rimuovere impostazioni di diagnostica dalle risorse di Azure |
| `Microsoft.OperationalInsights/*` | Consente di aggiungere, aggiornare e rimuovere configurazioni per le aree di lavoro di Log Analytics |
| `Microsoft.OperationsManagement/*` | Consente di aggiungere e rimuovere soluzioni di gestione |
| `Microsoft.Resources/deployments/*` | Consente di creare ed eliminare distribuzioni, è necessaria per aggiungere e rimuovere soluzioni, aree di lavoro e account di automazione |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Consente di creare ed eliminare distribuzioni, è necessaria per aggiungere e rimuovere soluzioni, aree di lavoro e account di automazione |

Per aggiungere e rimuovere utenti da un ruolo utente, è necessario avere le autorizzazioni `Microsoft.Authorization/*/Delete` e `Microsoft.Authorization/*/Write`.

Usare questi ruoli per concedere agli utenti l'accesso ad ambiti diversi:
- Sottoscrizione: accesso a tutte le aree di lavoro nella sottoscrizione
- Gruppo di risorse: accesso a tutte le aree di lavoro nel gruppo di risorse
- Risorsa: accesso alla sola area di lavoro specificata

È consigliabile eseguire le assegnazioni solo a livello di risorsa (area di lavoro) per assicurare un controllo di accesso accurato.  Usare i [ruoli personalizzati](../role-based-access-control/custom-roles.md) per creare ruoli con le autorizzazioni specifiche necessarie.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Collegare un'area di lavoro esistente a una sottoscrizione di Azure
Tutte le aree di lavoro create dopo il 26 settembre 2016 devono essere collegate a una sottoscrizione di Azure al momento della creazione. All'accesso, le aree di lavoro create prima di tale data devono essere collegate a un'area di lavoro. Quando si crea l'area di lavoro dal portale di Azure o si collega l'area di lavoro a una sottoscrizione di Azure, Azure Active Directory viene collegato come account aziendale.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Per collegare un'area di lavoro a una sottoscrizione di Azure nel portale di Azure
1. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Log Analytics**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analytics**.  

2. Nel riquadro delle sottoscrizioni di Log Analytics fare clic si **Aggiungi**.  

    ![Elenco di aree di lavoro](./media/log-analytics-manage-access/workspace-link-existing-01.png)

3. Nel riquadro **Area di lavoro di Log Analytics** fare clic su **Collega esistente**.  

4. Fare clic su **Configura le impostazioni necessarie**.  

5. Viene visualizzato l'elenco delle aree di lavoro non ancora collegate all'account Azure. Selezionare l'area di lavoro.  
   
6. Se necessario, è possibile modificare i valori per gli elementi seguenti:
   * Sottoscrizione
   * Gruppo di risorse
   * Località
   * Piano tariffario  

7. Fare clic su **OK**. L'area di lavoro ora è collegata all'account Azure.

> [!NOTE]
> Se non viene visualizzata l'area di lavoro a cui ci si vuole collegare, la sottoscrizione di Azure non ha accesso all'area di lavoro creata usando il portale di OMS.  Per concedere l'accesso a questo account dal portale di OMS, vedere [Aggiungere un utente a un'area di lavoro esistente](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Aggiornare l'area di lavoro a un piano a pagamento
In OMS sono disponibili tre tipi di piani per l'area di lavoro: **Gratuito**, **Standalone** e **OMS**.  Se si usa il piano *gratuito*, è previsto un limite di 500 MB di dati inviati ogni giorno a Log Analytics.  Se si supera questo limite, è necessario modificare l'area di lavoro in un piano a pagamento per evitare che non vengano raccolti dati oltre questo limite. Il tipo di piano può essere cambiato in qualsiasi momento.  Per altre informazioni sui prezzi di OMS, vedere i [dettagli sui prezzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Uso dei diritti derivanti da una sottoscrizione di OMS
Per usare i diritti che derivano dall'acquisto di OMS E1, OMS E2 o un componente aggiuntivo di OMS per System Center, scegliere il piano *OMS* di Log Analytics.

Quando si acquista una sottoscrizione di OMS, i diritti vengono aggiunti al contratto Enterprise Agreement. I diritti possono essere usati da ogni sottoscrizione di Azure creata in base a tale contratto. Tutte le aree di lavoro in queste sottoscrizioni usano i diritti di OMS.

Per assicurarsi che l'uso di un'area di lavoro venga applicato ai diritti derivanti dalla sottoscrizione di OMS, è necessario:

1. Creare un'area di lavoro in una sottoscrizione di Azure che fa parte del contratto Enterprise Agreement che include sia la sottoscrizione di OMS

2. Selezionare il piano *OMS* per l'area di lavoro

> [!NOTE]
> Se l'area di lavoro è stata creata prima del 26 settembre 2016 e il piano tariffario di Log Analytics è *Premium*, l'area di lavoro usa i diritti derivanti dal componente aggiuntivo OMS per System Center. È possibile anche usare i diritti passando al piano tariffario *OMS*.
>
>

I diritti della sottoscrizione di OMS non sono visibili nel portale di Azure. È possibile visualizzare i diritti e l'uso in Enterprise Portal.  

Se è necessario modificare la sottoscrizione di Azure a cui è collegata la propria area di lavoro, è possibile usare il cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) di Azure PowerShell.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Uso dell'impegno di Azure da un contratto Enterprise
Se non si dispone di una sottoscrizione di OMS, è necessario pagare separatamente per ogni componente di OMS e l'uso viene visualizzato nella fattura di Azure.

Se si ha un impegno monetario di Azure nell'iscrizione Enterprise a cui sono collegate le sottoscrizioni di Azure, l'utilizzo di Log Analytics verrà automaticamente addebitato a fronte dell'impegno monetario rimanente.

Se è necessario modificare la sottoscrizione di Azure a cui è collegata l'area di lavoro, è possibile usare il cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) di Azure PowerShell.  

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-azure-portal"></a>Passare a un piano tariffario a pagamento per l'area di lavoro nel portale di Azure
1. Nel riquadro delle sottoscrizioni di Log Analytics del portale di Azure selezionare un'area di lavoro.

2. Nel riquadro dell'area di lavoro selezionare **Piano tariffario** in **Generale**.  

3. In **Piano tariffario** selezionare un piano tariffario e quindi fare clic su **Seleziona**.  
    ![Piano tariffario selezionato](./media/log-analytics-manage-access/workspace-pricing-tier-info.png)

> [!NOTE]
> Se l'area di lavoro è collegata a un account di Automazione, prima di poter selezionare il piano tariffario *Standalone (Per GB)* (Autonomo - per GB), è necessario eliminare eventuali soluzioni di **Automazione e controllo** e scollegare l'account di Automazione. Nel pannello dell'area di lavoro in **Generale** fare clic su **Soluzioni** per visualizzare ed eliminare le soluzioni. Per scollegare l'account di Automazione, fare clic sul nome dell'account di Automazione nel pannello **Piano tariffario**.
>
>

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-oms-portal"></a>Passare a un piano tariffario a pagamento per l'area di lavoro nel portale di OMS

Per modificare il piano tariffario usando il portale di OMS, è necessaria una sottoscrizione di Azure.

1. Nel portale di OMS fare clic sul riquadro **Impostazioni**.

2. Fare clic sulla scheda **Account** e quindi sulla scheda **Azure Subscription & Data Plan** (Sottoscrizione di Azure e piano dati).

3. Fare clic sul piano tariffario che si vuole usare.

4. Fare clic su **Save**.  

    ![Sottoscrizione e piani di dati](./media/log-analytics-manage-access/subscription-tab.png)

Il nuovo piano dati viene visualizzato nella barra multifunzione del portale di OMS, nella parte superiore della pagina Web.

![Barra multifunzione di OMS](./media/log-analytics-manage-access/data-plan-changed.png)

## <a name="next-steps"></a>Passaggi successivi
* Vedere [Log Analytics agent overview](log-analytics-agent-overview.md) (Panoramica dell'agente di Log Analytics) per raccogliere dati dai computer nel data center o in un altro ambiente cloud.
* Vedere [Raccogliere dati sulle macchine virtuali di Azure](log-analytics-quick-collect-azurevm.md) per configurare la raccolta di dati dalle macchine virtuali di Azure.  
* [Aggiungere soluzioni di Log Analytics dalla raccolta soluzioni](../monitoring/monitoring-solutions.md) per aggiungere funzionalità e raccogliere i dati.

