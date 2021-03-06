---
title: "Esercitazione: Esaminare l'utilizzo e i costi di Azure con Cloudyn | Microsoft Docs"
description: In questa esercitazione si esamineranno l'utilizzo e i costi per tenere traccia delle tendenze, rilevare le inefficienze e creare avvisi.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/31/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: ''
manager: benshy
ms.openlocfilehash: 7b9c9a600d105d4b7fbbeb4f52ee42b5eb2bcaaa
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275871"
---
<!-- Intent: As a cloud-consuming user, I need to view usage and costs for my cloud resources and services.
-->

# <a name="tutorial-review-usage-and-costs"></a>Esercitazione: Esaminare l'utilizzo e i costi

Cloudyn mostra l'utilizzo e i costi consentendo di tenere traccia delle tendenze, rilevare le inefficienze e creare avvisi. Tutti i dati di utilizzo e i dati sui costi sono visualizzati nei dashboard e nei report di Cloudyn. Gli esempi di questa esercitazione illustrano come esaminare l'utilizzo e i costi tramite i dashboard e i report.

Gestione costi di Azure offre funzionalità simili a quelle di Cloudyn. Gestione costi di Azure è una soluzione di gestione costi nativa di Azure. È un modo per analizzare i costi, creare e gestire budget, esportare i dati, esaminare e implementare gli elementi consigliati per l'ottimizzazione e di conseguenza risparmiare. Per altre informazioni, vedere [Gestione costi di Azure](overview-cost-mgt.md).

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Tenere traccia delle tendenze di utilizzo e dei costi
> * Rilevare le inefficienze dell'utilizzo
> * Creare avvisi per spese inusuali o eccessive
> * Esportazione dei dati

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>prerequisiti

- È necessario disporre di un account Azure.
- È necessario disporre di una registrazione di valutazione o una sottoscrizione a pagamento per Cloudyn.

## <a name="open-the-cloudyn-portal"></a>Aprire il portale Cloudyn

È possibile esaminare tutte le informazioni sull'utilizzo e sui costi nel portale Cloudyn. Aprire il portale di Cloudyn dal portale di Azure oppure passare a https://azure.cloudyn.com ed eseguire l'accesso.

## <a name="track-usage-and-cost-trends"></a>Tenere traccia delle tendenze di utilizzo e dei costi

È possibile tenere traccia del denaro effettivamente speso per l'utilizzo e i costi con i report nel tempo per identificare le tendenze. Per iniziare a osservare le tendenze, usare il report Actual Cost Over Time (Costi effettivi nel tempo). Nella parte superiore sinistra del portale, fare clic su **Costo** > **Analisi dei costi** > **Actual Cost Over Time** (Costo effettivo nel tempo). Quando viene aperto per la prima volta, al report non sono applicati gruppi o filtri.

Di seguito è illustrato un report di esempio:

![report di esempio](./media/tutorial-review-usage/actual-cost01.png)

Il report visualizza tutte le spese degli ultimi 30 giorni. Per visualizzare solo le spese per i servizi di Azure, applicare il gruppo Servizio e quindi applicare un filtro per tutti i servizi di Azure. L'immagine seguente mostra i servizi filtrati.

![servizi filtrati](./media/tutorial-review-usage/actual-cost02.png)

Nell'esempio precedente a partire dal 29-10-2018 la spesa è stata inferiore rispetto al periodo precedente. Un numero eccessivo di colonne può però rendere meno evidente una tendenza ovvia. È possibile modificare la visualizzazione del report in un grafico a linee o ad area per visualizzare i dati visibili in altre visualizzazioni. La figura seguente illustra la tendenza più chiaramente.

![tendenza nel report](./media/tutorial-review-usage/actual-cost03.png)

Continuando con l'esempio, è possibile vedere che il costo per macchina virtuale di Azure è diminuito. Anche i costi relativi ad altri servizi di Azure hanno iniziato a diminuire nello stesso giorno. Che cosa ha causato il calo della spesa? In questo esempio è stato completato un progetto di lavoro di grandi dimensioni, di conseguenza è diminuito il consumo di numerosi servizi di Azure.

Per guardare un video di esercitazione su come tenere traccia delle tendenze di utilizzo e dei costi, vedere [Analyzing your cloud billing data vs. time with Cloudyn](https://youtu.be/7LsVPHglM0g) (Analisi del rapporto fatturazione/tempo per il cloud con Cloudyn).

## <a name="detect-usage-inefficiencies"></a>Rilevare le inefficienze dell'utilizzo

I report dell'utilità di ottimizzazione migliorano l'efficienza, ottimizzano l'utilizzo e identificano i modi in cui è possibile risparmiare il denaro speso nelle risorse cloud. I report sono particolarmente utili con consigli di ridimensionamento che hanno come obiettivo la riduzione del numero di macchine virtuali inattive o costose.

Un problema comune alle organizzazioni che spostano per la prima volta le risorse nel cloud è rappresentato dalla strategia di virtualizzazione. Le organizzazioni usano spesso un approccio simile a quello usato per la creazione di macchine virtuali per l'ambiente di virtualizzazione locale. Presuppongono che per ridurre i costi sia sufficiente spostare le macchine virtuali locali nel cloud, senza apportare altre modifiche. Tuttavia, è probabile che questo approccio non riduca i costi.

Il problema è che l'infrastruttura esistente è già stata pagata. Gli utenti potevano creare e mantenere in esecuzione macchine virtuali di grandi dimensioni, inattive o meno, senza alcuna conseguenza. Lo spostamento di macchine virtuali di grandi dimensioni o inattive nel cloud può *aumentare* i costi. L'allocazione dei costi per le risorse è importante quando si sottoscrivono contratti con i provider di servizi cloud. È necessario pagare per quanto sottoscritto, indipendentemente dal fatto che la risorsa venga usata interamente o meno.

Il report Cost Effective Sizing Recommendations (Consigli di ridimensionamento per la riduzione dei costi) identifica i risparmi annuali potenziali confrontando la capacità dei tipi di istanza delle macchine virtuali con i dati cronologici di utilizzo della CPU e della memoria.  

Nel menu nella parte superiore del portale fare clic su **Optimizer** (Utilità di ottimizzazione)  > **Pricing Optimization** (Ottimizzazione dimensioni)  > **Cost Effective Sizing Recommendations** (Consigli di ridimensionamento per la riduzione dei costi). Se utile, applicare un filtro per ridurre il numero di risultati. Di seguito è riportata una figura di esempio.

![Macchine virtuali di Azure](./media/tutorial-review-usage/sizing01.png)

In questo esempio è stato possibile risparmiare $ 2.382 seguendo i consigli per modificare i tipi di istanza di macchina virtuale. Fare clic sul segno più (+) in **Details** (Dettagli) per il primo consiglio. Di seguito sono illustrati i dettagli del primo consiglio.

![dettagli del consiglio](./media/tutorial-review-usage/sizing02.png)

Fare clic sul segno più accanto a **List of Candidates** (Elenco dei candidati) per visualizzare gli ID delle istanze della macchina virtuale.

![Elenco dei candidati](./media/tutorial-review-usage/sizing03.png)

Per guardare un video di esercitazione per il rilevamento delle inefficienze di utilizzo, vedere [Optimizing VM Size in Cloudyn](https://youtu.be/1xaZBNmV704) (Ottimizzazione delle dimensioni delle macchine virtuali in Cloudyn).

Gestione costi di Azure fornisce anche gli elementi consigliati di ottimizzazione dei costi per i servizi di Azure. Per altre informazioni, vedere [Esercitazione: Ottimizzare i costi grazie agli elementi consigliati](tutorial-acm-opt-recommendations.md).

## <a name="create-alerts-for-unusual-spending"></a>Creare avvisi per spese inusuali

È possibile inviare automaticamente avvisi agli stakeholder relativi ad anomalie nelle spese e rischi di spese eccessive. È possibile creare avvisi in modo semplice e rapido usando report che supportano gli avvisi basati su soglie di budget e costo.

È possibile creare un avviso per qualsiasi spesa utilizzando un report Cost (Costo). In questo esempio usare il report Actual Cost Over Time (Costo effettivo nel tempo) per ricevere un avviso quando la spesa della macchina virtuale di Azure si avvicina al budget totale. Tutti i passaggi seguenti sono necessari per la creazione dell'avviso. Nel menu nella parte superiore del portale fare clic su **Costo** > **Analisi dei costi** > **Actual Cost Over Time** (Costo effettivo nel tempo). Impostare **Groups** (Gruppi) su **Service** (Servizio) e impostare **Filter on the service** (Filtro sul servizio) su **Azure/VM** (Azure/macchina virtuale). Nella parte superiore destra del report fare clic su **Actions** (Azioni) e quindi selezionare **Schedule report** (Pianifica report).

Nella casella Save or Schedule this report (Salva o pianifica report), usare la scheda **Scheduling** (Pianificazione) per inviare a se stessi un messaggio di posta elettronica con il report con la frequenza desiderata. Assicurarsi di selezionare **Send via email** (Invia tramite posta elettronica). Nel report inviato tramite posta elettronica sono inclusi tutti i tag, i raggruppamenti e i filtri usati. Fare clic sulla scheda **Threshold** (Soglia) e selezionare **Actual Cost vs. Threshold** (Costo effettivo/soglia). Se si ha a disposizione un budget totale di $ 20.000 e si vuole ricevere una notifica quando il costo si avvicina alla metà del budget, creare un **Red alert** (Avviso rosso) a $ 10.000 e un **Yellow alert** (Avviso giallo) a $ 9.000. Non includere le virgole nei valori immessi. Scegliere quindi il numero di avvisi consecutivi. Dopo aver ricevuto il numero totale di avvisi specificato, non vengono inviati altri avvisi. Salvare il report pianificato.

![report di esempio](./media/tutorial-review-usage/schedule-alert01.png)

È anche possibile scegliere Cost Percentage vs. Budget (Percentuale costo/budget) per la creazione degli avvisi. Con questa metrica è possibile usare le percentuali del budget anziché i valori di valuta.

## <a name="export-data"></a>Esportazione dei dati

Analogamente al modo in cui si creano gli avvisi per i report, è possibile esportare dati da tutti i report. Ad esempio, è possibile esportare un elenco di account Cloudyn o altri dati utente. Per esportare i report, aprire il report e fare clic su **Azioni** nella parte superiore destra del report. Tra le azioni disponibili, è possibile **esportare tutti i dati del report** in modo da poter scaricare o stampare le informazioni. In alternativa, è possibile selezionare **Pianifica report** per pianificare l'invio del report come messaggio di posta elettronica.

## <a name="next-steps"></a>Passaggi successivi

Questa esercitazione illustra come:

> [!div class="checklist"]
> * Tenere traccia delle tendenze di utilizzo e dei costi
> * Rilevare le inefficienze dell'utilizzo
> * Creare avvisi per spese inusuali o eccessive
> * Esportazione dei dati


Passare alla prossima esercitazione per imparare come prevedere la spesa usando i dati cronologici.

> [!div class="nextstepaction"]
> [Prevedere la spesa futura](tutorial-forecast-spending.md)
