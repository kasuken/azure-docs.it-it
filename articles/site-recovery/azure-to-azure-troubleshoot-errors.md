---
title: Risoluzione dei problemi di Azure Site Recovery per gli errori di replica da Azure ad Azure | Microsoft Docs
description: Risoluzione dei problemi e degli errori che si verificano quando si esegue la replica di macchine virtuali di Azure per il ripristino di emergenza
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: sujayt
ms.openlocfilehash: 040ace1eab4062c011ed82a59e7f5bfb789c256b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945740"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Risolvere i problemi di replica delle VM da Azure ad Azure

Questo articolo descrive i problemi comuni in Azure Site Recovery quando si eseguono la replica e il ripristino di macchine virtuali di Azure da un'area a un'altra e illustra come risolverli. Per altre informazioni sulle configurazioni supportate, vedere la [matrice di supporto per la replica delle VM di Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problemi di quota delle risorse di Azure (codice errore 150097)
È necessario che la sottoscrizione sia abilitata per la creazione di VM di Azure nell'area di destinazione che si prevede di usare come area per il ripristino di emergenza. Per la sottoscrizione deve inoltre essere abilitata una quota sufficiente per la creazione di VM di dimensioni specifiche. Per impostazione predefinita, Site Recovery usa, per la VM di destinazione, le stesse dimensioni della VM di origine. Se le dimensioni corrispondenti non sono disponibili, vengono selezionate automaticamente le dimensioni più vicine possibili. Se non ci sono dimensioni corrispondenti che supportano la configurazione della VM di origine, viene visualizzato il messaggio di errore seguente:

**Codice errore** | **Possibili cause** | **Consiglio**
--- | --- | ---
150097<br></br>**Messaggio**: Replication couldn't be enabled for the virtual machine VmName (Non è stato possibile abilitare la replica per la macchina virtuale NomeVM). | - L'ID sottoscrizione potrebbe non essere abilitato per la creazione di VM nel percorso dell'area di destinazione.</br></br>- L'ID sottoscrizione potrebbe non essere abilitato o non avere una quota sufficiente per la creazione di VM di dimensioni specifiche nel percorso dell'area di destinazione.</br></br>- Non è stata trovata una VM di destinazione adatta con un numero di schede di interfaccia di rete (2) corrispondente alla VM di origine per l'ID sottoscrizione nel percorso dell'area di destinazione.| Contattare il [servizio di supporto per la fatturazione di Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) per abilitare la creazione di VM delle dimensioni richieste nel percorso di destinazione per la sottoscrizione. Dopo l'abilitazione, provare a eseguire di nuovo l'operazione che ha causato l'errore.

### <a name="fix-the-problem"></a>Risolvere il problema
È possibile contattare il [servizio di supporto per la fatturazione di Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) per abilitare la sottoscrizione per la creazione di VM delle dimensioni richieste nel percorso di destinazione.

Se il percorso di destinazione ha un vincolo di capacità, disabilitare la replica e abilitarla in un percorso diverso dove la sottoscrizione ha una quota sufficiente per la creazione di VM delle dimensioni richieste.

## <a name="trusted-root-certificates-error-code-151066"></a>Certificati radice trusted (codice errore 151066)

Se nella VM non sono presenti tutti i certificati radice trusted più recenti, il processo di abilitazione della replica potrebbe non riuscire. Senza i certificati, le chiamate di autenticazione e autorizzazione del servizio Site Recovery dalla VM non riescono. Viene visualizzato il messaggio di errore del processo di abilitazione della replica di Site Recovery:

**Codice errore** | **Causa possibile** | **Raccomandazioni**
--- | --- | ---
151066<br></br>**Messaggio**: La configurazione di Site Recovery non è riuscita. | I certificati radice trusted necessari usati per l'autorizzazione e l'autenticazione non sono presenti nel computer. | - Per una VM che esegue il sistema operativo Windows, assicurarsi che i certificati radice trusted siano presenti nel computer. Per informazioni, vedere [Configurare radici attendibili e certificati non consentiti](https://technet.microsoft.com/library/dn265983.aspx).<br></br>- Per una VM che esegue il sistema operativo Linux, seguire le indicazioni per i certificati radice trusted pubblicate dal distributore della versione del sistema operativo Linux.

### <a name="fix-the-problem"></a>Risolvere il problema
**Windows**

Installare tutti gli aggiornamenti di Windows più recenti nella VM in modo che tutti i certificati radice trusted siano presenti nel computer. Se si lavora in un ambiente non connesso, seguire il processo di aggiornamento di Windows standard dell'organizzazione per ottenere i certificati. Se i certificati richiesti non sono presenti nella VM, le chiamate al servizio Site Recovery non riescono per motivi di sicurezza.

Seguire il processo tipico di gestione degli aggiornamenti di Windows o dei certificati in uso nell'organizzazione per ottenere tutti i certificati radice più recenti e l'elenco di revoche di certificati aggiornato nelle VM.

Per verificare che il problema sia stato risolto, passare a login.microsoftonline.com da un browser nella VM.

**Linux**

Seguire le indicazioni fornite dal distributore di Linux per ottenere i certificati radice trusted più recenti e l'elenco di revoche di certificati aggiornato nella VM.

Poiché SuSE Linux usa i collegamenti simbolici per gestire un elenco di certificati, eseguire questa procedura:

1.  Accedere come utente ROOT.

2.  Eseguire questo comando per cambiare la directory.

      ``# cd /etc/ssl/certs``

3. Controllare se è presente il certificato CA della radice di Symantec.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. Se il certificato CA della radice di Symantec non è reperibile, eseguire il comando seguente per scaricare il file. Verificare la presenza di eventuali errori e seguire le azioni consigliate per gli errori di rete.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. Controllare se è presente il certificato CA della radice Baltimore.

      ``# ls Baltimore_CyberTrust_Root.pem``

6. Se il certificato CA della radice Baltimore non è reperibile, scaricarlo.  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. Verificare che il certificato DigiCert_Global_Root_CA sia presente.

    ``# ls DigiCert_Global_Root_CA.pem``

8. Se il certificato DigiCert_Global_Root_CA non è reperibile, eseguire i comandi seguenti per scaricarlo.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. Eseguire lo script rehash per aggiornare i codici hash dell'oggetto per i certificati appena scaricati.

    ``# c_rehash``

10. Controllare che gli hash dell'oggetto come collegamenti simbolici vengano creati per i certificati.

    - Comando

      ``# ls -l | grep Baltimore``

    - Output

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Comando

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Output

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Comando

      ``# ls -l | grep DigiCert_Global_Root``

    - Output

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

11. Creare una copia del file VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem con nome file b204d74a.0

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

12. Creare una copia del file Baltimore_CyberTrust_Root.pem con nome file 653b494a.0

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Creare una copia del file DigiCert_Global_Root_CA.pem con nome file 3513523f.0

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Controllare che i file siano presenti.  

    - Comando

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Output

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Connettività in uscita per gli intervalli IP o gli URL di Site Recovery (codice errore 151037 o 151072)

Per il funzionamento della replica di Site Recovery, è necessaria la connettività in uscita dalla VM a intervalli IP o URL specifici. Se la macchina virtuale è protetta da un firewall o usa regole di gruppi di sicurezza di rete (NGS) per controllare la connettività in uscita, potrebbe verificarsi uno di questi problemi.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151037-br"></a>Problema 1: Non è stato possibile registrare la macchina virtuale di Azure con Site Recovery (151037) </br>
- **Causa possibile** </br>
  - Si usa un gruppo di sicurezza di rete (NSG) per controllare l'accesso in uscita nella macchina virtuale e gli intervalli di indirizzi IP necessari non sono inclusi nell'elenco elementi consentiti per l'accesso in uscita.
  - Si usano strumenti firewall di terze parti e gli intervalli di indirizzi IP/URL necessari non sono inclusi nell'elenco di elementi consentiti.


- **Risoluzione**
   - Se si usa un proxy firewall per controllare la connettività di rete in uscita nella macchina virtuale, assicurarsi che gli URL o gli intervalli di indirizzi IP dei data center prerequisiti siano inclusi negli elenchi elementi consentiti. Per informazioni, vedere le [indicazioni per i proxy firewall](https://aka.ms/a2a-firewall-proxy-guidance).
   - Se si usano regole di gruppi di sicurezza di rete (NSG) per controllare la connettività di rete in uscita nella macchina virtuale, assicurarsi che gli intervalli di indirizzi IP dei data center prerequisiti siano inclusi nell'elenco elementi consentiti. Per altre informazioni, vedere le [indicazioni per i gruppi di sicurezza di rete](azure-to-azure-about-networking.md).
   - Per includere nell'elenco elementi consentiti gli [URL necessari](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) o gli [intervalli IP necessari](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), seguire i passaggi nel [documento di indicazioni sulle reti](azure-to-azure-about-networking.md).

### <a name="issue-2-site-recovery-configuration-failed-151072"></a>Problema 2: La configurazione di Site Recovery non è riuscita (151072)
- **Causa possibile** </br>
  - Non è possibile stabilire la connessione agli endpoint di servizio di Site Recovery


- **Risoluzione**
   - Se si usa un proxy firewall per controllare la connettività di rete in uscita nella macchina virtuale, assicurarsi che gli URL o gli intervalli di indirizzi IP dei data center prerequisiti siano inclusi negli elenchi elementi consentiti. Per informazioni, vedere le [indicazioni per i proxy firewall](https://aka.ms/a2a-firewall-proxy-guidance).
   - Se si usano regole di gruppi di sicurezza di rete (NSG) per controllare la connettività di rete in uscita nella macchina virtuale, assicurarsi che gli intervalli di indirizzi IP dei data center prerequisiti siano inclusi nell'elenco elementi consentiti. Per altre informazioni, vedere le [indicazioni per i gruppi di sicurezza di rete](https://aka.ms/a2a-nsg-guidance).
   - Per includere nell'elenco elementi consentiti gli [URL necessari](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) o gli [intervalli IP necessari](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), seguire i passaggi nel [documento di indicazioni sulle reti](site-recovery-azure-to-azure-networking-guidance.md).

### <a name="issue-3-a2a-replication-failed-when-the-network-traffic-goes-through-on-premise-proxy-server-151072"></a>Problema 3: La replica da Azure ad Azure non riesce quando il traffico di rete attraversa un server proxy locale (151072)
 - **Causa possibile** </br>
   - Le impostazioni proxy personalizzate e l'agente del servizio Mobility di Azure Site Recovery non hanno rilevato automaticamente le impostazioni proxy da Internet Explorer


 - **Risoluzione**
  1.    L'agente del servizio Mobility rileva le impostazioni proxy da Internet Explorer in Windows e da /etc/environment in Linux.
  2.  Se si preferisce impostare il proxy solo per il servizio Mobility di Azure Site Recovery, è possibile fornire i dettagli del proxy in ProxyInfo.conf, disponibile in:</br>
      - ``/usr/local/InMage/config/`` su ***Linux***
      - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` su ***Windows***
  3.    Il file ProxyInfo.conf deve includere le impostazioni proxy nel formato INI seguente. </br>
                   *[proxy]*</br>
                   *Address=http://1.2.3.4*</br>
                   *Port=567*</br>
  4. L'agente del servizio Mobility di Azure Site Recovery supporta solo ***proxy non autenticati***.

### <a name="fix-the-problem"></a>Risolvere il problema
Per includere nell'elenco elementi consentiti gli [URL necessari](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) o gli [intervalli IP necessari](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), seguire i passaggi nel [documento di indicazioni sulle reti](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disco non trovato nel computer (codice errore 150039)

È necessario inizializzare un nuovo disco collegato alla VM.

**Codice errore** | **Possibili cause** | **Raccomandazioni**
--- | --- | ---
150039<br></br>**Messaggio**: Il disco dati di Azure (NomeDisco) (%DiskUri) con numero di unità logica (LUN) (ValoreLUN) non è stato mappato a un disco corrispondente presente all'interno della macchina virtuale con lo stesso valore LUN. | - Alla VM è stato collegato un nuovo disco dati che però non è stato inizializzato.</br></br>- Il disco dati all'interno della VM non segnala il valore LUN corretto con il quale il disco è stato collegato alla VM.| Assicurarsi che i dischi dati siano inizializzati e quindi provare a eseguire di nuovo l'operazione.</br></br>Per Windows: [Connettere e inizializzare un nuovo disco](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Per Linux: [Inizializzare un nuovo disco dati in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Risolvere il problema
Assicurarsi che i dischi dati siano stati inizializzati e quindi provare a eseguire di nuovo l'operazione:

- Per Windows: [Connettere e inizializzare un nuovo disco](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Per Linux: [aggiungere un nuovo disco dati in Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Se il problema persiste, contattare il supporto tecnico.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Impossibile vedere la VM di Azure tra le opzioni per l'abilitazione della replica

 **Causa 1: Il gruppo di risorse e la macchina virtuale di origine si trovano in una posizione diversa** <br>
Attualmente, Azure Site Recovery richiede che il gruppo di risorse dell'area di origine e le macchine virtuali si trovino obbligatoriamente nella stessa posizione. Se non è questo il caso, non sarà possibile trovare la macchina virtuale durante il periodo di protezione.

**Causa 2: Il gruppo di risorse non appartiene alla sottoscrizione selezionata** <br>
Potrebbe non essere possibile trovare il gruppo di risorse durante il periodo di protezione se il gruppo non fa parte della sottoscrizione specificata. Assicurarsi che il gruppo di risorse appartenga alla sottoscrizione in uso.

 **Causa 3: Configurazione non aggiornata** <br>
Se la macchina virtuale che si desidera abilitare per la replica non viene visualizzata, potrebbe essere a causa di una configurazione di Site Recovery non aggiornata nella macchina virtuale di Azure. La configurazione non aggiornata potrebbe rimanere in una VM di Azure nei casi seguenti:

- È stata abilitata la replica per la VM di Azure usando Site Recovery e quindi è stato eliminato l'insieme di credenziali di Site Recovery senza disabilitare in modo esplicito la replica nella VM.
- È stata abilitata la replica per la VM di Azure usando Site Recovery e quindi è stato eliminato il gruppo di risorse contenente l'insieme di credenziali di Site Recovery senza disabilitare in modo esplicito la replica nella VM.

### <a name="fix-the-problem"></a>Risolvere il problema

È possibile usare lo [script per la rimozione della configurazione non aggiornata di Azure Site Recovery](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) e rimuovere la configurazione dalla VM di Azure. Dopo la rimozione della configurazione non aggiornata dovrebbe essere possibile visualizzare la macchina virtuale.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Impossibile selezionare la macchina virtuale per eseguire la procedura di protezione 
 **Causa 1: Un'estensione installata nella macchina virtuale si trova in stato di errore o non risponde** <br>
 Passare a Macchine virtuali > Impostazioni > Estensioni e verificare se esistono eventuali estensioni che si trovano in uno stato di errore. Disinstallare l'estensione in stato di errore e ripetere la procedura di protezione della macchina virtuale.<br>
 **Causa 2: [Lo stato di provisioning della VM non è valido](#vms-provisioning-state-is-not-valid-error-code-150019)**

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>Lo stato di provisioning della VM non è valido (codice errore 150019)

Per abilitare la replica sulla VM, lo stato di provisioning deve essere **Riuscito**. Per verificare lo stato della macchina virtuale, seguire questa procedura.

1.  Selezionare **Esplora risorse** da **Tutti i servizi** nel portale di Azure.
2.  Espandere l'elenco **Sottoscrizioni** e selezionare la sottoscrizione.
3.  Espandere l'elenco **Gruppi di risorse** e selezionare il gruppo di risorse della VM.
4.  Espandere l'elenco **Risorse** e selezionare la macchina virtuale
5.  Controllare il campo **provisioningState** nella visualizzazione Istanza a destra.

### <a name="fix-the-problem"></a>Risolvere il problema

- Se **provisioningState** è **Non riuscito**, contattare il supporto tecnico specificando i dettagli necessari per risolvere il problema.
- Se **provisioningState** è **Aggiornamento**, potrebbe essere in corso la distribuzione di un'altra estensione. Controllare se sono in corso operazioni nella VM, attenderne il completamento e provare a eseguire di nuovo il processo **Abilita replica** di Site Recovery non riuscito.

## <a name="unable-to-select-target-virtual-network---network-selection-tab-is-grayed-out"></a>Non è possibile selezionare la rete virtuale di destinazione: la scheda di selezione della rete è inattiva.

**Causa 1: la macchina virtuale è collegata a una rete che è già mappata a una "rete di destinazione".**
- Se la macchina virtuale di origine fa parte di una rete virtuale e un'altra macchina virtuale della stessa rete virtuale è già mappata a una rete nel gruppo di risorse di destinazione, l'elenco a discesa di selezione della rete è disabilitato per impostazione predefinita.

![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

**Causa 2: in precedenza la macchina virtuale è stata protetta tramite Azure Site Recovery disabilitando la replica.**
 - La disabilitazione della replica di una macchina virtuale non elimina il mapping di rete. Questo deve essere eliminato dall'insieme di credenziali di Servizi di ripristino in cui è stata protetta la macchina virtuale. </br>
 Passare a Insieme di credenziali di Servizi di ripristino > Infrastruttura di Site Recovery > Mapping di rete. </br>
 ![Delete_NW_Mapping](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)
 - La rete di destinazione impostata durante la configurazione del ripristino di emergenza può essere modificata dopo la configurazione iniziale, in seguito alla protezione della macchina virtuale. </br>
 ![Modify_NW_mapping](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)
 - La modifica del mapping di rete interessa tutte le macchine virtuali protette che usano il mapping specifico.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>Errore del servizio Copia Shadow del volume/COM+ (codice di errore 151025)
**Codice errore** | **Possibili cause** | **Raccomandazioni**
--- | --- | ---
151025<br></br>**Messaggio**: Site recovery extension failed to install (Impossibile installare l'estensione di Site Recovery) | - Il servizio "Applicazione di sistema COM+" è disabilitato.</br></br>- Il servizio "Copia Shadow del volume" è disabilitato.| Impostare i servizi "Applicazione di sistema COM+" e "Copia Shadow del volume" sulla modalità di avvio automatica o manuale.

### <a name="fix-the-problem"></a>Risolvere il problema

È possibile aprire la console "Servizi" e verificare che i servizi "Applicazione di sistema COM+" e "Copia Shadow del volume" non siano impostati su "Disabilitato" per "Tipo di avvio".
  ![Errore COM](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="next-steps"></a>Passaggi successivi
[Replicare le macchine virtuali di Azure](site-recovery-replicate-azure-to-azure.md)
