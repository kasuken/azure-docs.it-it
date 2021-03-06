---
title: Protezione del servizio SQL di Azure e dei dati nel Centro sicurezza di Azure | Microsoft Docs
description: Questo documento illustra le raccomandazioni presenti nel Centro sicurezza di Azure che facilitano la protezione dei dati e del servizio SQL di Azure e garantiscano la conformità ai criteri di sicurezza.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: terrylan
ms.openlocfilehash: 177deb779ca3e3e9575a41ab9a37bb51d5e79df8
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51008080"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Protezione del servizio SQL di Azure e dei dati nel Centro sicurezza di Azure
Il Centro sicurezza di Azure analizza lo stato di sicurezza delle risorse di Azure. Quando il Centro sicurezza identifica potenziali vulnerabilità della sicurezza, crea raccomandazioni utili per definire il processo di configurazione dei controlli necessari.  Le raccomandazioni sono applicabili a diversi tipi di risorse di Azure, ovvero macchine virtuali, risorse di rete, SQL, dati e applicazioni.

Questo articolo illustra le raccomandazioni applicabili al servizio SQL di Azure e ai dati e relative all'abilitazione del controllo per i database SQL Azure e della crittografia per i database SQL e dell'account di archiviazione di Azure.  Usare la tabella seguente come riferimento per conoscere le raccomandazioni disponibili per il servizio SQL e per i dati e i relativi effetti se vengono applicate.
### <a name="monitor-data-security"></a>Monitorare la sicurezza dei dati

Quando si fa clic su **Sicurezza dei dati** nella sezione **Prevenzione**, si apre il pannello **Risorse dati** con consigli relativi a SQL e all'archiviazione. Il pannello include anche [raccomandazioni](security-center-sql-service-recommendations.md) sullo stato di integrità generale del database. Per altre informazioni sulla crittografia per l'archiviazione, vedere [Enable encryption for Azure storage account in Azure Security Center](security-center-enable-encryption-for-storage-account.md) (Abilitare la crittografia per l'account di archiviazione di Azure nel Centro sicurezza di Azure).

![Risorse dati](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

In **Raccomandazioni su SQL** è possibile fare clic su qualsiasi raccomandazione e ottenere maggiori dettagli sulle ulteriori azioni per risolvere un problema. L'esempio seguente illustra l'espansione della raccomandazione **Database Auditing & Threat detection on SQL databases** (Controllo database e rilevamento minacce nei database SQL).

![Informazioni dettagliate su una raccomandazione SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

Il pannello **Abilita il controllo e il rilevamento minacce nei database SQL** contiene le informazioni seguenti:

* Elenco di database SQL.
* Server in cui si trovano.
* Informazioni sull'impostazione, ovvero se è stata ereditata dal server o se è univoca in questo database.
* Stato corrente.
* Gravità del problema.

Quando si fa clic sul database per applicare la raccomandazione, viene aperto il pannello **Controllo e rilevamento minacce** come illustrato nella schermata seguente.

![Controllo e rilevamento minacce](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Per abilitare il controllo, selezionare semplicemente **SÌ** nell'opzione **Controllo**.

## <a name="data-and-storage-recommendations"></a>Raccomandazioni per i dati e l'archiviazione

|Tipo di risorsa|Punteggio di sicurezza|Raccomandazione|Descrizione|
|----|----|----|----|
|Account di archiviazione|20|Richiedi il trasferimento sicuro nell'account di archiviazione|Il trasferimento sicuro è un'opzione che impone all'account di archiviazione di accettare richieste solo da connessioni sicure (HTTPS). L'uso di HTTPS garantisce l'autenticazione tra il server e il servizio e protegge i dati in transito dagli attacchi a livello rete, come attacchi man-in-the-middle, eavesdropping e hijack della sessione.|
|Redis|20|Consenti solo connessioni sicure alla Cache Redis|Abilitare solo le connessioni al servizio Cache Redis tramite SSL. L'uso di connessioni sicure garantisce l'autenticazione tra il server e il servizio e protegge i dati in transito dagli attacchi a livello rete, come attacchi man-in-the-middle, eavesdropping e hijack della sessione.|
|SQL|15|Abilita Transparent Data Encryption nei database SQL|Abilitare Transparent Data Encryption per proteggere i dati inattivi e rispettare i requisiti di conformità.|
|SQL|15|Abilita il controllo sui server SQL|Abilitare il controllo per i server SQL. (Solo servizio Azure SQL. Non include istanze di SQL in esecuzione nelle macchine virtuali.)|
|SQL|15|Abilita il controllo sui database SQL|Abilitare il controllo per i database SQL di Azure. (Solo servizio Azure SQL. Non include istanze di SQL in esecuzione nelle macchine virtuali.)|
|Data Lake Analytics|15|Abilita la crittografia dei dati inattivi di Data Lake Analytics|Abilitare Transparent Data Encryption per proteggere i dati inattivi in Data Lake Analytics. La crittografia dei dati inattivi è trasparente. In altre parole, Data Lake Analytics crittografa automaticamente i dati prima di renderli persistenti e li decrittografa prima di recuperarli. Non sono necessarie modifiche nelle applicazioni e nei servizi che interagiscono con Data Lake Analytics a causa della crittografia. La crittografia dei dati inattivi riduce al minimo il rischio di perdita di dati in caso di furto ed è anche utile per rispettare i requisiti di conformità alle normative.|
|Data Lake Store|15|Abilita la crittografia dei dati inattivi per Data Lake Store|Abilitare Transparent Data Encryption per proteggere i dati inattivi in Data Lake Store. La crittografia dei dati inattivi è trasparente. In altre parole, Data Lake Store crittografa automaticamente i dati prima di renderli persistenti e li decrittografa prima di recuperarli. Per supportare la crittografia non è necessario apportare modifiche nelle applicazioni e nei servizi che interagiscono con Data Lake Store. La crittografia dei dati inattivi riduce al minimo il rischio di perdita di dati in caso di furto ed è anche utile per rispettare i requisiti di conformità alle normative.|
|Account di archiviazione|15|Abilita la crittografia per l'account di archiviazione di Azure|Abilitare la crittografia del servizio di archiviazione di Azure per i dati inattivi che applica la crittografia ai dati quando vengono scritti nell'archiviazione di Azure e li decrittografa prima del recupero. La crittografia del servizio Archiviazione di Azure è attualmente disponibile solo per il servizio BLOB di Azure e può essere usata per BLOB in blocchi, BLOB di pagine e BLOB di aggiunta.|
|Data Lake Analytics|5|Abilita i log di diagnostica in Data Lake Analytics|Abilitare i log e conservarli per un periodo massimo di un anno. Ciò consente di ricreare la traccia delle attività per scopi di analisi quando si verifica un evento imprevisto della sicurezza o la rete viene compromessa. |
|Data Lake Store|5|Abilita i log di diagnostica in Azure Data Lake Store|Abilitare i log e conservarli per un periodo massimo di un anno. Ciò consente di ricreare la traccia delle attività per scopi di analisi quando si verifica un evento imprevisto della sicurezza o la rete viene compromessa. |
|SQL|30|Risolvi le vulnerabilità nei database SQL|La funzionalità di valutazione della vulnerabilità di SQL analizza il database per individuare vulnerabilità a livello di sicurezza ed espone eventuali scostamenti dalle procedure consigliate, ad esempio configurazioni errate, autorizzazioni eccessive e dati sensibili non protetti. La risoluzione delle vulnerabilità rilevate può migliorare significativamente il livello di sicurezza del database.|
|SQL|20|Effettua il provisioning di un amministratore di Azure AD per SQL Server|Effettuare il provisioning di un amministratore di Azure AD per il server SQL per abilitare l'autenticazione di Azure AD. Questo tipo di autenticazione consente una gestione semplificata delle autorizzazioni e una gestione centralizzata delle identità degli utenti di database e di altri servizi Microsoft.|
|Account di archiviazione|15|Disabilita l'accesso alla rete senza restrizioni per gli account di archiviazione|Controllare l'accesso alla rete senza restrizioni nelle impostazioni del firewall dell'account di archiviazione. Configurare invece regole di rete in modo che l'account di archiviazione sia accessibile solo alle applicazioni provenienti da reti consentite. Per consentire connessioni da specifici client Internet o locali, è possibile concedere l'accesso al traffico proveniente da determinate reti virtuali di Azure o diretto verso intervalli di indirizzi IP Internet pubblici.|
|Account di archiviazione|1||Esegui la migrazione degli account di archiviazione alle nuove risorse di Azure Resource Manager|Usare la nuova versione di Azure Resource Manager (v2) per gli account di archiviazione per fornire funzionalità di sicurezza migliorate quali controllo degli accessi (RBAC) più avanzato, controllo migliore, distribuzione e governance basate su Azure Resource Manager, accesso alle identità gestite, accesso all'insieme di credenziali delle chiavi per i segreti, autenticazione basata su Azure AD e supporto di tag e gruppi di risorse per una gestione della sicurezza semplificata.|



## <a name="see-also"></a>Vedere anche 
Per altre informazioni sulle raccomandazioni applicabili ad altri tipi di risorse di Azure, vedere gli argomenti seguenti:

* [Protezione delle macchine virtuali nel Centro sicurezza di Azure](security-center-virtual-machine-recommendations.md)
* [Protezione delle applicazioni nel Centro sicurezza di Azure](security-center-application-recommendations.md)
* [Protezione della rete nel Centro sicurezza di Azure](security-center-network-recommendations.md)

Per altre informazioni sul Centro sicurezza, vedere gli argomenti seguenti:

* [Impostazione dei criteri di sicurezza nel Centro sicurezza di Azure](security-center-policies.md) : informazioni su come configurare i criteri di sicurezza per le sottoscrizioni e i gruppi di risorse di Azure.
* [Gestione e risposta agli avvisi di sicurezza nel Centro sicurezza di Azure](security-center-managing-and-responding-alerts.md) : informazioni su come gestire e rispondere agli avvisi di sicurezza.
* [Domande frequenti sul Centro sicurezza di Azure](security-center-faq.md) : domande frequenti sull'uso del servizio.
