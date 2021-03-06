---
title: Risolvere problemi relativi ad Azure Migrate | Microsoft Docs
description: Questo articolo offre una panoramica dei problemi noti relativi al servizio Azure Migrate e alcuni suggerimenti per la risoluzione degli errori comuni.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: raynew
ms.openlocfilehash: 0b2954ddfda0ab4c94ddf6176d76d8bcd937fa42
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50413334"
---
# <a name="troubleshoot-azure-migrate"></a>Risolvere i problemi relativi ad Azure Migrate

## <a name="troubleshoot-common-errors"></a>Risoluzione dei problemi comuni

[Azure Migrate](migrate-overview.md) valuta i carichi di lavoro locali per la migrazione ad Azure. Attenersi a questo articolo per risolvere i problemi che possono verificarsi durante la distribuzione e l'utilizzo di Azure Migrate.

### <a name="i-am-using-the-continuous-discovery-ova-but-vms-that-are-deleted-in-my-on-premises-environment-are-still-being-shown-in-the-portal"></a>Sto usando un OVA di individuazione continua, ma le macchine virtuali eliminate dal mio ambiente locale vengono ancora visualizzate nel portale.

L'appliance per l'individuazione continua si limita a raccogliere i dati sulle prestazioni in modo continuo, non rileva eventuali modifiche alla configurazione nell'ambiente locale (ad esempio, aggiunta ed eliminazione di macchine virtuali, aggiunta di dischi e così via). Se si esegue una modifica della configurazione nell'ambiente locale, è possibile procedere come segue per riflettere le modifiche nel portale:

- Aggiunta di elementi (macchine virtuali, dischi, core e così via): per riflettere tali modifiche nel portale di Azure, è possibile arrestare l'individuazione dall'appliance e quindi riavviarla. Ciò garantisce che le modifiche vengono aggiornate nel progetto Azure Migrate.

   ![Arresta individuazione](./media/troubleshooting-general/stop-discovery.png)

- Eliminazione di macchine virtuali: a causa della modo in cui è progettata l'appliance, l'eliminazione di macchine virtuali non viene rilevata anche se si arresta e riavvia l'individuazione. I dati acquisiti dalle individuazioni successive vengono infatti aggiunti alle individuazioni precedenti e non sostituiti. In questo caso è possibile semplicemente ignorare la macchina virtuale nel portale, rimuovendola dal gruppo e ricalcolando la valutazione.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>La creazione del progetto di migrazione non è riuscita con errore *Requests must contain user identity headers* (Le richieste devono contenere le intestazioni di identità dell'utente)

Questo problema può verificarsi per gli utenti che non hanno accesso al tenant di Azure Active Directory (Azure AD) dell'organizzazione. Quando un utente viene aggiunto a un tenant di Azure AD per la prima volta, riceve un invito tramite posta elettronica per entrare nel tenant. Gli utenti devono accedere al messaggio di posta elettronica e accettare l'invito per essere aggiunti correttamente al tenant. Se non si riesce a visualizzare il messaggio di posta elettronica, contattare un utente che già dispone dell'accesso al tenant e chiedere di inviare di nuovo l'invito tramite la procedura specificata [qui](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Una volta ricevuto il messaggio di posta elettronica di invito, è necessario aprirlo e fare clic sul collegamento al suo interno per accettare l'invito. Al termine, è necessario disconnettersi dal portale di Azure e accedere nuovamente; l'aggiornamento del browser non funzionerà. È quindi possibile provare a creare il progetto di migrazione.

### <a name="performance-data-for-disks-and-networks-adapters-shows-as-zeros"></a>I dati sulle prestazioni relative ai dischi e alle schede di rete sono pari a zero

Questo problema può verificarsi se il livello delle impostazioni relative alle statistiche nel server vCenter è impostato su un valore inferiore a tre. Se impostato sul livello tre o superiore, vCenter archivia la cronologia delle prestazioni della macchina virtuale per scopi di calcolo, archiviazione e rete. Se impostato su un livello inferiore a tre, vCenter non archivia dati di archiviazione e di rete ma solo dati relativi alla CPU e alla memoria. In questo scenario, i dati sulle prestazioni risultano pari a zero in Azure Migrate, che fornisce indicazioni sulle dimensioni di dischi e reti in base ai metadati raccolti dalle macchine locali.

Per abilitare la raccolta dei dati sulle prestazioni di dischi e reti, impostare il livello relativo alle impostazioni delle statistiche su tre. Attendere quindi almeno un giorno per identificare l'ambiente e valutarlo.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Ho installato gli agenti e usato la visualizzazione delle dipendenze per creare gruppi. Ora, in seguito a un failover, sulle macchine viene visualizzata l'azione "Installa agente" anziché "Visualizza dipendenze"
* In seguito a un failover pianificato o non pianificato, le macchine locali vengono disattivate e le macchine equivalenti vengono riattivate in Azure. Queste macchine acquisiscono un indirizzo MAC diverso ed eventualmente anche un indirizzo IP diverso, in base alla scelta dell'utente di mantenere o meno l'indirizzo IP locale. Se gli indirizzi IP e MAC sono diversi, Azure Migrate non associa le macchine locali ai dati sulle dipendenze di Elenco dei servizi e chiede all'utente di installare gli agenti anziché visualizzare le dipendenze.
* In seguito al failover di test, le macchine locali rimangono attivate come previsto. Le macchine equivalenti riattivate in Azure acquisiscono un indirizzo MAC diverso ed eventualmente anche un indirizzo IP diverso. Se l'utente non blocca il traffico di Log Analytics in uscita da queste macchine, Azure Migrate non associa le macchine locali ai dati sulle dipendenze di Elenco dei servizi e chiede all'utente di installare gli agenti anziché visualizzare le dipendenze.

### <a name="i-specified-an-azure-geography-while-creating-a-migration-project-how-do-i-find-out-the-exact-azure-region-where-the-discovered-metadata-would-be-stored"></a>Durante la creazione di un progetto di migrazione è stata specificata un'area geografica di Azure. Come è possibile individuare l'esatta area di Azure in cui verranno archiviati i metadati individuati?

È possibile passare alla sezione **Informazioni di base** della pagina **Panoramica** del progetto per identificare la posizione esatta in cui sono archiviati i metadati. La posizione viene selezionata in modo casuale all'interno dell'area geografica da Azure Migrate e non è possibile modificarla. Se si vuole creare un progetto solo in una determinata area, è possibile usare le API REST per creare il progetto di migrazione e passare alla regione desiderata.

   ![Posizione del progetto](./media/troubleshooting-general/geography-location.png)

## <a name="collector-errors"></a>Errori dell'agente di raccolta

### <a name="deployment-of-azure-migrate-collector-failed-with-the-error-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry"></a>La distribuzione del servizio Agente di raccolta di Azure Migrate non è riuscita con l'errore: The provided manifest file is invalid: Invalid OVF manifest entry. (Il file manifesto specificato non è valido: voce del manifesto OVF non valida.)

1. Verificare se il file OVA del servizio Agente di raccolta di Azure Migrate viene scaricato correttamente controllando il relativo valore hash. Per verificare il valore hash, vedere questo [articolo](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware#verify-the-collector-appliance). Se il valore hash non corrisponde, scaricare nuovamente il file OVA e ripetere il tentativo di distribuzione.
2. Se ancora non riesce e si usa VMware vSphere Client per distribuire il file OVF, provare a eseguire la distribuzione con vSphere Web Client. Se il problema persiste, provare a usare un Web browser diverso.
3. Se si usa vSphere Web Client e si prova a distribuirlo in vCenter Server 6.5, provare a distribuire il file OVA direttamente nell'host ESXi seguendo questi passaggi:
  - Connettersi direttamente all'host ESXi (anziché vCenter Server) usando il client Web (https://<*indirizzo IP dell'host*>/ui)
  - Passare a Home > Inventory (Pagina iniziale > Inventario)
  - Fare clic sul File > Deploy OVF template (Distribuisci modello OVF) > individuare il file OVA e completare la distribuzione
4. Se l'errore di distribuzione persiste, contattare il supporto di Azure Migrate.


### <a name="collector-is-not-able-to-connect-to-the-internet"></a>L'agente di raccolta non è in grado di connettersi a Internet

Ciò può verificarsi quando il computer in uso è protetto da un proxy. Assicurarsi di fornire le credenziali di autorizzazione, se richiesto dal proxy.
Se si usa qualsiasi proxy firewall basato su URL per controllare la connettività in uscita, assicurarsi di inserire nell'elenco dei consentiti questi URL richiesti:

**URL** | **Scopo**  
--- | ---
*.portal.azure.com | Necessario per controllare la connettività al servizio di Azure e convalidare i problemi relativi alla sincronizzazione dell'ora.
*.oneget.org | Necessario per il download del modulo vCenter PowerCLI basato su PowerShell.

**L'agente di raccolta non è in grado di connettersi a Internet a causa di un errore di convalida del certificato**

Questa situazione può verificarsi se si usa un proxy di intercettazione per connettersi a Internet e non è stato importato il certificato del proxy nella macchina virtuale dell'agente di raccolta. È possibile importare il certificato del proxy eseguendo i passaggi descritti [qui](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity).

**L'agente di raccolta non riesce a connettersi al progetto usando l'ID progetto e la chiave copiata dal portale.**

Verificare di aver copiato e incollato le informazioni corrette. Per risolvere il problema, installare Microsoft Monitoring Agent (MMA) e verificare se è in grado di connettersi al progetto come descritto di seguito:

1. Scaricare [MMA](https://go.microsoft.com/fwlink/?LinkId=828603) nella macchina virtuale dell'agente di raccolta.
2. Per avviare l'installazione, fare doppio clic sul file scaricato.
3. Nella pagina di **benvenuto** della procedura di configurazione fare clic su **Avanti**. Nella pagina **Condizioni di licenza** fare clic su **Accetto** per accettare la licenza.
4. In **Cartella di destinazione** mantenere o modificare la cartella di installazione predefinita e quindi fare clic su **Avanti**.
5. In **Opzioni di installazione dell'agente** selezionare **Azure Log Analytics** > **Avanti**.
6. Fare clic su **Aggiungi** per aggiungere una nuova area di lavoro di Log Analytics. Incollare l'ID progetto e la chiave copiata in precedenza. Quindi fare clic su **Next**.
7. Verificare che l'agente riesca a connettersi al progetto. In caso contrario, verificare le impostazioni. Se l'agente riesce a connettersi, ma non l'agente di raccolta, contattare il supporto tecnico.


### <a name="error-802-date-and-time-synchronization-error"></a>Errore 802: errore di sincronizzazione di data e ora

È possibile che l'orologio del server abbia perso la sincronizzazione con l'ora corrente da più di cinque minuti. Modificare l'ora nella macchina virtuale dell'agente di raccolta in modo che corrisponda all'ora corrente, come illustrato di seguito:

1. Nella macchina virtuale aprire un prompt dei comandi come amministratore.
2. Per controllare il fuso orario, eseguire w32tm /tz.
3. Per sincronizzare l'ora, eseguire w32tm /resync.

### <a name="vmware-powercli-installation-failed"></a>L'installazione di VMware PowerCLI non è riuscita.

L'agente di raccolta di Azure Migrate scarica PowerCLI ed esegue l'installazione nel dispositivo. L'errore di installazione di PowerCLI potrebbe essere a causa di un endpoint non raggiungibile per il repository PowerCLI. Per risolvere il problema, provare a installare PowerCLI manualmente nella macchina virtuale dell'agente di raccolta usando il passaggio seguente:

1. Aprire Windows PowerShell in modalità amministratore.
2. Andare alla directory C:\ProgramFiles\ProfilerService\VMWare\Scripts\
3. Eseguire lo script InstallPowerCLI.ps1

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Si è verificato l'errore interno UnhandledException: System.IO.FileNotFoundException

Questo problema può verificarsi a causa di un problema con l'installazione di VMware PowerCLI. Seguire la procedura seguente per risolvere il problema:

1. Se non si usa la versione più recente dell'appliance dell'agente di raccolta, [eseguire l'aggiornamento dell'agente di raccolta alla versione più recente](https://aka.ms/migrate/col/checkforupdates) e verificare che il problema si sia risolto.
2. Se si dispone già della versione più recente dell'agente di raccolta, installare manualmente [VMware PowerCLI 6.5.2](https://www.powershellgallery.com/packages/VMware.PowerCLI/6.5.2.6268016) e verificare che il problema sia risolto.
3. Se le operazioni precedenti non risolvono il problema, passare alla cartella C:\Program Files\ProfilerService e rimuovere i file VMware.dll e VimService65.dll presenti nella cartella e quindi riavviare il servizio "Agente di raccolta di Azure Migrate" in Service Manager di Windows (Aprire "Esegui" e digitare "Services.msc" per aprire Service Manager di Windows).

### <a name="error-unabletoconnecttoserver"></a>Errore UnableToConnectToServer

Impossibile connettersi al server vCenter "Servername.com:9443" a causa dell'errore: There was no endpoint listening at https://Servername.com:9443/sdk that could accept the message. (Nessun endpoint in ascolto su https://Servername.com:9443/sdk in grado di accettare il messaggio).

Controllare se si sta usando la versione più recente dell'appliance dell'agente di raccolta; in caso contrario, aggiornare l'appliance alla [versione più recente](https://docs.microsoft.com/azure/migrate/concepts-collector#how-to-upgrade-collector).

Se l'errore si verifica anche con la versione più recente, è possibile che computer dell'agente di raccolta non sia in grado di risolvere il nome del server vCenter specificato o la porta specificata non è corretta. Per impostazione predefinita, se la porta non è specificata, l'agente di raccolta tenterà di connettersi alla porta numero 443.

1. Provare a effettuare il ping di Servername.com dal computer dell'agente di raccolta.
2. Se il passaggio 1 ha esito negativo, provare a connettersi al server vCenter sull'indirizzo IP.
3. Identificare il numero di porta corretto per connettersi al server vCenter.
4. Infine controllare che il server vCenter sia attivo e in esecuzione.

## <a name="troubleshoot-dependency-visualization-issues"></a>Risolvere i problemi di visualizzazione delle dipendenze

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>È stato installato Microsoft Monitoring Agent (MMA) e Dependency Agent nelle macchine virtuali locali, ma le dipendenze sono ora visualizzate nel portale di Azure Migrate.

Dopo aver installato gli agenti, Azure Migrate richiede in genere 15-30 minuti per visualizzare le dipendenze nel portale. Se si è atteso per più di 30 minuti, verificare che l'agente MMA sia in grado di comunicare con l'area di lavoro OMS, seguendo questa procedura:

Per la macchina virtuale Windows:
1. Andare al **pannello di controllo** e avviare **Microsoft Monitoring Agent**
2. Andare alla scheda **Azure Log Analytics (OMS)** nella finestra popup delle proprietà MMA
3. Verificare che lo **Stato** dell'area di lavoro sia di colore verde.
4. Se lo stato non è di colore verde, provare a rimuovere l'area di lavoro e aggiungerla nuovamente a MMA.
        ![Stato MMA](./media/troubleshooting-general/mma-status.png)

Per la macchina virtuale Linux, verificare che i comandi di installazione per l'agente MMA e per il Dependency Agent abbiano avuto esito positivo.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>Quali sono i sistemi operativi supportati da MMA?

L'elenco dei sistemi operativi Windows supportati da MMA è reperibile [qui](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
L'elenco dei sistemi operativi Linux supportati da MMA è reperibile [qui](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Quali sono i sistemi operativi supportati dal Dependency Agent?

L'elenco dei sistemi operativi Windows supportati dal Dependency Agent è reperibile [qui](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
L'elenco dei sistemi operativi Linux supportati dal Dependency Agent è reperibile [qui](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Non riesco a visualizzare le dipendenze in Azure Migrate per più di un'ora.
Azure Migrate consente di visualizzare le dipendenze per la durata di un'ora. Azure Migrate tuttavia consente di tornare a una determinata data nella cronologia al massimo nel mese precedente. Il tempo massimo per cui possibile visualizzare le dipendenze è un'ora. Ad esempio, è possibile usare la funzionalità di durata nella mappa delle dipendenze per visualizzare le dipendenze di ieri ma possono essere visualizzate solo per un'ora.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>Non riesco a visualizzare le dipendenze per i gruppi con più di 10 macchine virtuali.
È possibile [visualizzare le dipendenze per i gruppi](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) che hanno fino a 10 macchine virtuali. Se si dispone di un gruppo con più di 10 macchine virtuali, è consigliabile dividere il gruppo in gruppi più piccoli e visualizzarne le dipendenze.

## <a name="troubleshoot-readiness-issues"></a>Risolvere problemi di idoneità

**Problema** | **Correzione**
--- | ---
Il tipo di avvio non è supportato | Azure non supporta le macchine virtuali con tipo di avvio EFI. È consigliabile convertire il tipo di avvio in BIOS prima di eseguire una migrazione. <br/><br/>Per eseguire la migrazione di queste macchine virtuali è possibile usare [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure), che converte il tipo di avvio della macchina virtuale in BIOS durante la migrazione.
Sistema operativo Windows supportato in modo condizionale | Il sistema operativo ha superato la data di fine del supporto e necessita di un contratto di supporto personalizzato per usufruire del [supporto in Azure](https://aka.ms/WSosstatement). Provare ad aggiornare il sistema operativo prima di eseguire la migrazione ad Azure.
Sistema operativo Windows non supportato | Azure supporta solo [determinate versioni del sistema operativo Windows](https://aka.ms/WSosstatement). Provare ad aggiornare il sistema operativo della macchina virtuale prima di eseguire la migrazione ad Azure.
Sistema operativo Linux con approvazione condizionale | Azure approva solo [determinate versioni del sistema operativo Linux](../virtual-machines/linux/endorsed-distros.md). Provare ad aggiornare il sistema operativo della macchina virtuale prima di eseguire la migrazione ad Azure.
Sistema operativo Linux non approvato | La macchina virtuale può essere avviata in Azure, ma Azure non fornisce alcun supporto del sistema operativo. Provare ad aggiornare il sistema operativo a una [versione di Linux approvata](../virtual-machines/linux/endorsed-distros.md) prima di eseguire la migrazione ad Azure.
Sistema operativo sconosciuto | Il sistema operativo della macchina virtuale è stato specificato come "Altro" nel server vCenter, pertanto Azure Migrate non è in grado di identificare l'idoneità per Azure della macchina virtuale. Verificare che il sistema operativo in esecuzione nella macchina sia [supportato](https://aka.ms/azureoslist) da Azure prima di eseguire la migrazione della macchina.
Numero di bit del sistema operativo non supportati | Le macchine virtuali con sistema operativo a 32 bit possono essere avviate in Azure, ma è consigliabile aggiornare il sistema operativo della macchina virtuale da 32 bit a 64 bit prima di eseguire la migrazione ad Azure.
Richiede una sottoscrizione di Visual Studio. | Nelle macchine virtuali è in esecuzione un sistema operativo client Windows supportato solo nella sottoscrizione di Visual Studio.
Non è stata trovata alcuna macchina virtuale per le prestazioni di archiviazione richieste. | Le prestazioni di archiviazione (IOPS/velocità effettiva) richieste per la macchina superano le prestazioni supportate dalla macchina virtuale di Azure. Prima di eseguire la migrazione, ridurre i requisiti di archiviazione per la macchina.
Non è stata trovata alcuna macchina virtuale per le prestazioni di rete richieste. | Le prestazioni di rete (ingresso/uscita) richieste per la macchina superano le prestazioni supportate dalla macchina virtuale di Azure. Ridurre i requisiti di rete per la macchina.
Non è stata trovata alcuna macchina virtuale nel piano tariffario specificato. | Se il piano tariffario è impostato su Standard, è consigliabile ridurre le dimensioni della macchina virtuale prima di eseguire la migrazione ad Azure. Se il piano tariffario è impostato su Base, è consigliabile cambiare il piano tariffario della valutazione su Standard.
La macchina virtuale non è stata trovata nella località specificata. | Usare una località di destinazione diversa prima di eseguire migrazione.
Uno o più dischi non idonei. | Uno o più dischi collegati alla macchina virtuale non soddisfano i requisiti di Azure. Per ogni disco collegato alla macchina virtuale, verificare che le dimensioni disco siano < 4 TB; in caso contrario, ridurre le dimensioni disco prima di eseguire la migrazione ad Azure. Assicurarsi che le prestazioni necessarie per ogni disco (IOPS/velocità effettiva) siano supportata dai [dischi gestiti della macchina virtuale](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) di Azure.   
Una o più schede di rete non idonee. | Prima di eseguire la migrazione, rimuovere le schede di rete non usate dalla macchina.
Il numero di dischi supera il limite | Rimuovere i dischi non usati dalla macchina prima di eseguire la migrazione.
Le dimensioni del disco hanno superato il limite | Azure supporta dischi di dimensioni fino a 4 TB. Ridurre le dimensioni del disco a meno di 4 TB prima di eseguire la migrazione.
Il disco non è disponibile nella località specificata | Assicurarsi che il disco si trovi nella località di destinazione prima di eseguire la migrazione.
Il disco non è disponibile per la ridondanza specificata | Il disco deve usare il tipo di archiviazione di ridondanza definito nelle impostazioni di valutazione (per impostazione predefinita, LRS).
Non è stato possibile determinare l'idoneità del disco a causa di un errore interno | Provare a creare una nuova valutazione per il gruppo.
La macchina virtuale con core e memoria richiesti non è stata trovata | Azure non può trovare un tipo di macchina virtuale idonea. Prima di eseguire la migrazione, quindi, ridurre la memoria e il numero di core della macchina locale.
Non è stato possibile determinare l'idoneità della macchina virtuale a causa di un errore interno. | Provare a creare una nuova valutazione per il gruppo.
Non è stato possibile determinare l'idoneità di uno o più dischi a causa di un errore interno. | Provare a creare una nuova valutazione per il gruppo.
Non è stato possibile determinare l'idoneità di una o più schede di rete a causa di un errore interno. | Provare a creare una nuova valutazione per il gruppo.


## <a name="collect-logs"></a>Raccogliere i log

**Come è possibile raccogliere i log nella macchina virtuale dell'agente di raccolta?**

La funzionalità di creazione di log è abilitata per impostazione predefinita. I log si trovano nei percorsi seguenti:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Per raccogliere Event Trace for Windows, seguire questa procedura:

1. Nella macchina virtuale dell'agente di raccolta aprire una finestra di comando di PowerShell.
2. Eseguire **Get-EventLog -LogName Application | export-csv eventlog.csv**.

**Come è possibile raccogliere i log relativi al traffico di rete del portale?**

1. Aprire il browser e accedere al [portale](https://portal.azure.com).
2. Premere F12 per avviare Strumenti di sviluppo. Se necessario, deselezionare l'impostazione **Cancella voci durante l'esplorazione**.
3. Fare clic sulla scheda **Rete** e avviare l'acquisizione del traffico di rete:
 - In Chrome selezionare **Preserve log** (Mantieni log). La registrazione viene avviata automaticamente. Un cerchio rosso indica che è in corso l'acquisizione del traffico. Se non viene visualizzato, fare clic sul cerchio nero per avviarla.
 - Anche in Edge/Internet Explorer la registrazione viene avviata automaticamente. In caso contrario, fare clic sul pulsante di esecuzione verde.
4. Provare a riprodurre l'errore.
5. Dopo aver riscontrato l'errore durante la registrazione, arrestare la registrazione e salvare una copia dell'attività registrata:
 - In Chrome fare clic con il pulsante destro del mouse su **Save as HAR with content** (Salva come HAR con contenuto). In questo modo i log vengono compressi ed esportati come file con estensione har.
 - In Edge/Internet Explorer fare clic sull'icona **Esporta traffico catturato**. In questo modo il log viene compresso ed esportato.
6. Passare alla scheda **Console** per verificare la presenza di eventuali avvisi o errori. Per salvare il log della console:
 - In Chrome fare clic con il pulsante destro del mouse in un punto qualsiasi del log della console. Selezionare **Salva con nome** per esportare e comprimere il log.
 - In Edge/Internet Explorer fare clic con il pulsante destro del mouse sugli errori e selezionare **Copia tutto**.
7. Chiudere Strumenti di sviluppo.

## <a name="collector-error-codes-and-recommended-actions"></a>Codici di errore dell'agente di raccolta e azioni consigliate

| Codice di errore | Nome errore   | Message   | Possibili cause | Azione consigliata  |
| --- | --- | --- | --- | --- |
| 601       | CollectorExpired               | L'agente di raccolta è scaduto.                                                        | Agente di raccolta scaduto.                                                                                    | Scaricare una nuova versione dell'agente di raccolta e riprovare.                                                                                      |
| 751       | UnableToConnectToServer        | Non è possibile connettersi al server vCenter "%Name;" a causa dell'errore: %ErrorMessage;     | Per altre informazioni, controllare il messaggio di errore.                                                             | Risolvere il problema e riprovare.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | Il server "%Name;" non è un server vCenter.                                  | Specificare i dettagli del server vCenter.                                                                       | Ritentare l'operazione con i dettagli del server vCenter corretti.                                                                                   |
| 753       | InvalidLoginCredentials        | Non è possibile connettersi al server vCenter "%Name;" a causa dell'errore: %ErrorMessage; | La connessione al server vCenter non è riuscita a causa delle credenziali di accesso non valide.                             | Verificare che le credenziali di accesso specificate siano corrette.                                                                                    |
| 754       | NoPerfDataAvaialable           | Non sono disponibili dati sulle prestazioni.                                               | Controllare il livello di statistiche nel server vCenter. Perché i dati sulle prestazioni siano disponibili, questo deve essere impostato su 3. | Modificare il livello di statistiche a 3 (per una durata di 5 minuti, 30 minuti e 2 ore) e riprovare dopo avere atteso almeno un giorno.                   |
| 756       | NullInstanceUUID               | È stato rilevato un computer con valore InstanceUUID null                                  | Il server vCenter può presentare un oggetto non idoneo.                                                      | Risolvere il problema e riprovare.                                                                                                           |
| 757       | VMNotFound                     | Impossibile trovare la macchina virtuale                                                  | La macchina virtuale potrebbe essere stata eliminata: %VMID;                                                                | Verificare che le macchine virtuali selezionate nella definizione dell'ambito dell'inventario vCenter esistano durante l'individuazione                                      |
| 758       | GetPerfDataTimeout             | Timeout della richiesta vCenter. Messaggio %Message;                                  | Le credenziali del server vCenter non sono corrette                                                              | Controllare le credenziali del server vCenter e assicurarsi che questo sia raggiungibile. Ripetere l'operazione. Se il problema persiste, contattare il supporto tecnico. |
| 759       | VmwareDllNotFound              | DLL VMWare.Vim non trovata.                                                     | PowerCLI non è installato correttamente.                                                                   | Accertarsi che PowerCLI sia installato correttamente. Ripetere l'operazione. Se il problema persiste, contattare il supporto tecnico.                               |
| 800       | ServiceError                   | Il servizio Agente di raccolta di Azure Migrate non è in esecuzione.                               | Il servizio Agente di raccolta di Azure Migrate non è in esecuzione.                                                       | Usare il comando services.msc per avviare il servizio e ripetere l'operazione.                                                                             |
| 801       | PowerCLIError                  | L'installazione di VMware PowerCLI non è riuscita.                                          | L'installazione di VMware PowerCLI non è riuscita.                                                                  | Ripetere l'operazione. Se il problema persiste, installarlo manualmente e ritentare l'operazione.                                                   |
| 802       | TimeSyncError                  | L'ora non è sincronizzata con il server di riferimento ora Internet.                            | L'ora non è sincronizzata con il server di riferimento ora Internet.                                                    | Verificare che l'ora del computer sia impostata in modo accurato per il fuso orario del computer e ripetere l'operazione.                                 |
| 702       | OMSInvalidProjectKey           | È stata specificata una chiave del progetto non valida.                                                | È stata specificata una chiave del progetto non valida.                                                                        | Ripetere l'operazione con la chiave del progetto corretta.                                                                                              |
| 703       | OMSHttpRequestException        | Si è verificato un errore durante l'invio della richiesta. Messaggio %Message;                                | Controllare l'ID e la chiave del progetto e assicurarsi che l'endpoint sia raggiungibile.                                       | Ripetere l'operazione. Se il problema persiste, contattare il supporto tecnico Microsoft.                                                                     |
| 704       | OMSHttpRequestTimeoutException | Timeout della richiesta HTTP. Messaggio %Message;                                     | Controllare l'ID e la chiave del progetto e assicurarsi che l'endpoint sia raggiungibile.                                       | Ripetere l'operazione. Se il problema persiste, contattare il supporto tecnico Microsoft.                                                                     |
