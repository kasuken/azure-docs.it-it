---
title: Creare e gestire gruppi di azione nel portale di Azure
description: Informazioni su come creare e gestire gruppi di azione nel portale di Azure.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 3ce7c5111fa176bb7fa734f54084b9e14e7afbef
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016047"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Creare e gestire gruppi di azione nel portale di Azure
## <a name="overview"></a>Panoramica ##
Un gruppo di azioni è una raccolta delle preferenze di notifica definite dall'utente. Gli avvisi di Monitoraggio di Azure e di integrità dei servizi sono configurati per l'uso di un gruppo di azioni specifico quando viene generato l'avviso. I vari avvisi possono usare lo stesso gruppo di azioni o gruppi di azioni diversi, a seconda delle esigenze dell'utente.

Questo articolo illustra come creare e gestire gruppi di azione nel portale di Azure.

Ogni azione è composta dalle seguenti proprietà:

* **Nome:** un identificatore univoco all'interno del gruppo di azione.  
* **Tipo di azione**: l'azione da eseguire. Gli esempi includono l'invio di una chiamata vocale, un SMS o un messaggio di posta elettronica oppure l'attivazione di vari tipi di azioni automatizzate. Vedere i tipi più avanti in questo articolo. 
* **Dettagli**: i corrispondenti dettagli che variano in base al *tipo di azione*. 

Per informazioni sull'uso dei modelli di Azure Resource Manager per configurare i gruppi di azione: [Modelli di Resource Manager per il gruppo di azione](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Creare un gruppo di azione usando il portale di Azure ##
1. Nel [portale](https://portal.azure.com)selezionare **Monitoraggio**. Il pannello **Monitoraggio** consolida tutte le impostazioni e i dati di monitoraggio in una vista.

    ![Servizio "Monitoraggio"](./media/monitoring-action-groups/home-monitor.png)
1. Selezionare **Avvisi** e quindi **Gestisci gruppo di azioni**.

    ![Pulsante Gestisci gruppo di azioni](./media/monitoring-action-groups/manage-action-groups.png)
1. Selezionare **Aggiungi gruppo di azione** e compilare i campi.

    ![Comando "Aggiungi gruppo di azione"](./media/monitoring-action-groups/add-action-group.png)
1. Immettere un nome nella casella **Nome gruppo di azione** e un nome nella casella **Nome breve gruppo di azione**. Il nome breve viene usato al posto del nome completo di un gruppo di azione quando le notifiche vengono inviate usando questo gruppo.

      ![Finestra di dialogo "Aggiungi gruppo di azione"](./media/monitoring-action-groups/action-group-define.png)

1. Nella casella **Sottoscrizione** viene inserita automaticamente la sottoscrizione corrente. Il gruppo di azione verrà salvato in questa sottoscrizione.

1. Selezionare il **gruppo di risorse** in cui verrà salvato il gruppo di azione.

1. Definire un elenco di azioni fornendo i dati di ogni azione:

    a. **Nome**: immettere un identificatore univoco per questa azione.

    b. **Tipo di azione**: selezionare Massaggio di posta elettronica/SMS/Push/Voce, App per la logica, Webhook, ITSM o Runbook di Automazione.

    c. **Dettagli**: in base al tipo di azione, immettere un numero di telefono, un indirizzo di posta elettronica, l'URI del webhook, l'app Azure, la connessione ITSM o il runbook di Automazione. Per l'azione ITSM, specificare anche **Elemento di lavoro** e altri campi richiesti dallo strumento ITSM.

1. Fare clic su **OK** per creare il gruppo di azione.

## <a name="manage-your-action-groups"></a>Gestire i gruppi di azione ##
Dopo la creazione, il gruppo di azione sarà visibile nella sezione **Gruppi di azione** del pannello **Monitoraggio**. Selezionare il gruppo di azione da gestire per:

* Aggiungere, modificare o rimuovere azioni.
* Eliminare il gruppo di azione.

## <a name="action-specific-information"></a>Informazioni specifiche delle azioni
**Push dell'app Azure**: un gruppo di azioni può contenere fino a 10 azioni dell'app Azure. In questo momento l'azione dell'app Azure supporta solo gli avvisi ServiceHealth. Qualsiasi altro tipo di avviso verrà ignorato. Fare riferimento alle informazioni su come [configurare gli avvisi ogni volta che viene inviata una notifica di integrità del servizio](monitoring-activity-log-alerts-on-service-notifications.md).

**Posta elettronica**: i messaggi di posta elettronica verranno inviati dagli indirizzi di posta elettronica seguenti. Verificare che il filtro della posta elettronica sia configurato correttamente
   - azure-noreply@microsoft.com
   - azureemail-noreply@microsoft.com
   - alerts-noreply@mail.windowsazure.com

Un gruppo di azioni può contenere fino a 1000 azioni di posta elettronica. Vedere l'articolo relativo alle [informazioni sulla limitazione della frequenza](./monitoring-alerts-rate-limiting.md).

**ITSM**: un gruppo di azioni può contenere fino a 10 azioni di tipo ITSM. Per un'azione ITSM è necessaria una connessione ITSM. Informazioni su come creare una [connessione ITSM](../log-analytics/log-analytics-itsmc-overview.md).

**App per la logica**: un gruppo di azioni può contenere fino a 10 azioni di tipo App per la logica.

**Runbook**: un gruppo di azioni può contenere fino a 10 azioni di tipo Runbook. Per informazioni sui limiti per i payload di Runbook, vedere i [limiti dei servizi di sottoscrizione di Azure](../azure-subscription-service-limits.md).

**SMS**: un gruppo di azioni può contenere fino 10 azioni di tipo SMS. Vedere l'articolo contenente le [informazioni sulla limitazione della frequenza](./monitoring-alerts-rate-limiting.md) e quello relativo al [comportamento degli avvisi SMS](monitoring-sms-alert-behavior.md).

**Voce**: un gruppo di azioni può contenere fino a 10 azioni di tipo Voce.</dd>
Vedere l'articolo relativo alle [informazioni sulla limitazione della frequenza](./monitoring-alerts-rate-limiting.md).</dd>

**Webhook**: un gruppo di azioni può contenere un massimo di 10 azioni di tipo Webhook. Logica di ripetizione dei tentativi: il periodo di timeout per una risposta è 10 secondi. Verrà eseguito un massimo di 2 nuovi tentativi di chiamata webhook quando vengono restituiti i codici di stato HTTP 408, 429, 503, 504 o l'endpoint HTTP non risponde. La prima ripetizione del tentativo avviene dopo 10 secondi. La seconda e ultima ripetizione avviene dopo 100 secondi.

Intervalli di indirizzi IP di origine
    - 13.106.57.181
    - 13.106.54.3
    - 13.106.54.19
    - 13.106.38.142
    - 13.106.38.148
    - 13.106.57.196

Per ricevere aggiornamenti sulle modifiche apportate a questi indirizzi IP, è consigliabile configurare un [avviso di Integrità dei servizi](./monitoring-service-notifications.md) che consente di monitorare l'eventuale presenza di notifiche informative sul servizio Gruppi di azioni.


## <a name="next-steps"></a>Passaggi successivi ##
* Altre informazioni sul [Comportamento degli avvisi SMS](monitoring-sms-alert-behavior.md).  
* Leggere le [informazioni sullo schema webhook degli avvisi del log attività](monitoring-activity-log-alerts-webhook.md).  
* Altre informazioni sul [connettore ITSM](../log-analytics/log-analytics-itsmc-overview.md)
* Altre informazioni sulla [limitazione della frequenza](monitoring-alerts-rate-limiting.md) degli avvisi.
* Leggere una [panoramica degli avvisi del log attività](monitoring-overview-alerts.md) e informazioni su come ricevere gli avvisi.  
* Informazioni su come [configurare gli avvisi ogni volta che viene inviata una notifica sull'integrità del servizio](monitoring-activity-log-alerts-on-service-notifications.md).
