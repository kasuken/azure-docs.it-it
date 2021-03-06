---
title: Come configurare il firewall IP per l'account Azure Cosmos
description: Informazioni su come configurare i criteri di controllo di accesso agli indirizzi IP per il supporto del firewall negli account del database di Azure Cosmos DB.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: d9a00bccb83fc60c96594ffacc5abde98c0f8470
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632581"
---
# <a name="how-to-configure-ip-firewall-for-your-azure-cosmos-account"></a>Come configurare il firewall IP per l'account Azure Cosmos

È possibile proteggere i dati archiviati nell'account Azure Cosmos tramite i firewall IP. Azure Cosmos DB supporta i controlli di accesso IP per il supporto del firewall in ingresso. È possibile impostare un firewall IP per l'account Azure Cosmos in uno dei modi seguenti:

1. Dal portale di Azure
2. Creando in modo dichiarativo un modello di Azure Resource Manager
3. A livello programmatico tramite l'interfaccia della riga di comando di Azure o Azure Powershell, aggiornando la proprietà **ipRangeFilter**.

## <a id="configure-ip-policy"></a> Configurare il firewall IP usando il portale di Azure

Per impostare i criteri di controllo di accesso IP nel portale di Azure, passare alla pagina dell'account Azure Cosmos DB, fare clic su **Firewall e reti virtuali** nel menu di spostamento e quindi modificare il valore di **Consenti l'accesso da** impostandolo su **Reti selezionate** e infine fare clic su **Salva**. 

![Screenshot che mostra come aprire la pagina Firewall nel portale di Azure](./media/how-to-configure-firewall/azure-portal-firewall.png)

Una volta attivato il controllo di accesso IP, il portale di Azure offre la possibilità di specificare indirizzi e intervalli IP, nonché opzioni per abilitare l'accesso ad altri servizi di Azure e al portale di Azure. Nelle sezioni seguenti vengono fornite informazioni dettagliate su queste opzioni.

> [!NOTE]
> Dopo aver abilitato i criteri di controllo dell'accesso agli indirizzi IP per l'account di Azure Cosmos DB, qualsiasi richiesta all'account di Azure Cosmos DB da computer non inclusi nell'elenco degli intervalli di indirizzi IP consentiti viene respinto. Anche l'esplorazione delle risorse di Azure Cosmos DB dal portale viene bloccata per garantire l'integrità del controllo di accesso.

### <a name="allow-requests-from-the-azure-portal"></a>Consentire le richieste dal portale di Azure 

Quando si abilitano i criteri di controllo di accesso IP a livello di codice, è necessario aggiungere l'indirizzo IP per il portale di Azure alla proprietà **ipRangeFilter** per mantenere l'accesso. Gli indirizzi IP del portale sono i seguenti:

|Region|Indirizzo IP|
|------|----------|
|Germania|51.4.229.218|
|Cina|139.217.8.252|
|US Gov|52.244.48.71|
|Tutte le aree tranne le tre versioni precedenti|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

È possibile abilitare l'accesso al portale di Azure scegliendo le opzioni **Consenti l'accesso dal portale di Azure** come illustrato nello screenshot seguente: 

![Screenshot che mostra come abilitare l'accesso al portale di Azure](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Consentire le richieste dai Datacenter globali di Azure o da altre origini all'interno di Azure

Se si accede all'account Azure Cosmos dai servizi che non forniscono un indirizzo IP statico, ad esempio Analisi di flusso di Azure, Funzioni di Azure, ecc. è comunque possibile usare il firewall IP per limitare l'accesso. Per consentire l'accesso all'account Azure Cosmos da tali servizi, deve essere abilitata un'impostazione del firewall. Per abilitare questa impostazione del firewall, aggiungere l'indirizzo IP: 0.0.0.0 all'elenco degli indirizzi IP consentiti. 0.0.0.0 limita le richieste all'account Azure Cosmos dall'intervallo IP di datacenter di Azure. Questa impostazione non consente l'accesso ad altri intervalli IP all'account Azure Cosmos.

> [!NOTE]
> Questa opzione permette di configurare il firewall in maniera tale da consentire tutte le richieste da Azure, incluse le richieste dalle sottoscrizioni di altri clienti distribuite in Azure. L'elenco di indirizzi IP consentiti da questa opzione è ampia perciò limita l'efficacia dei criteri firewall. Questa opzione deve essere utilizzata solo se le richieste non hanno origine da indirizzi IP statici o subnet nelle reti virtuali. Scegliendo automaticamente questa opzione viene consentito l'accesso dal portale di Azure dal momento che il portale di Azure viene distribuito in Azure.

È possibile abilitare l'accesso al portale di Azure scegliendo l'opzione **Accetta connessioni da datacenter pubblici di Azure** come illustrato nello screenshot seguente: 

![Screenshot che mostra come aprire la pagina Firewall nel portale di Azure](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Richieste dall'IP corrente

Per semplificare lo sviluppo, il portale di Azure consente di identificare e aggiungere l'indirizzo IP del computer client all'elenco di indirizzi consentiti, in modo che le app in esecuzione nel computer possano accedere all'account Azure Cosmos. L'indirizzo IP del client viene rilevato automaticamente dal portale. Potrebbe trattarsi dell'indirizzo IP del client del computer oppure dell'indirizzo IP del gateway di rete. Assicurarsi di rimuovere questo indirizzo IP prima di eseguire i carichi di lavoro nell'ambiente di produzione. 

Per abilitare il proprio indirizzo IP corrente, selezionare **Add my current IP** (Aggiungi indirizzo IP corrente), in modo da aggiungere l'indirizzo all'elenco, e quindi fare clic su **Salva**.

![Screenshot che mostra come configurare le impostazioni del firewall per l'IP corrente](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Richieste da servizi cloud

In Azure, i servizi cloud sono uno strumento comune per ospitare la logica del servizio di livello intermedio con Azure Cosmos DB. Per consentire l'accesso a un account di Azure Cosmos da un servizio cloud, l'indirizzo IP pubblico del servizio cloud deve essere aggiunto all'elenco di indirizzi IP consentiti associato all'account di Azure Cosmos [configurando il criterio di controllo dell'accesso agli indirizzi IP](#configure-ip-policy). In questo modo, tutte le istanze del ruolo dei servizi cloud hanno accesso all'account di Azure Cosmos. È possibile recuperare gli indirizzi IP per i servizi cloud nel portale di Azure, come mostrato nello screenshot seguente:

![Screenshot che mostra l'indirizzo IP pubblico per un servizio cloud visualizzato nel portale di Azure](./media/how-to-configure-firewall/public-ip-addresses.png)

Quando si aumenta il numero di istanze del servizio cloud aggiungendo altre istanze del ruolo, le nuove istanze hanno automaticamente accesso all'account di Azure Cosmos perché fanno parte dello stesso servizio cloud.

### <a name="requests-from-virtual-machines"></a>Richieste da macchine virtuali

Per ospitare servizi di livello intermedio con Azure Cosmos DB è possibile usare anche [macchine virtuali](https://azure.microsoft.com/services/virtual-machines/) o [set di scalabilità di macchine virtuali](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Per configurare l'account Cosmos in modo da consentire l'accesso da macchine virtuali, gli indirizzi IP pubblici della macchina virtuale e/o del set di scalabilità di macchine virtuali devono essere configurati come uno degli indirizzi IP consentiti per l'account di Azure Cosmos [configurando il criterio di controllo dell'accesso agli indirizzi IP](#configure-ip-policy). È possibile recuperare gli indirizzi IP per le macchine virtuali nel portale di Azure, come mostrato nello screenshot seguente:

![Screenshot che mostra un indirizzo IP pubblico per una macchina virtuale visualizzata nel portale di Azure](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Quando si aggiungono istanze di macchina virtuale al gruppo, a queste viene automaticamente fornito l'accesso all'account di Azure Cosmos.

### <a name="requests-from-the-internet"></a>Richieste da Internet

Quando si accede a un account di Azure Cosmos da un computer in Internet, l'indirizzo IP o l'intervallo di indirizzi IP client del computer deve essere aggiunto all'elenco di indirizzi IP consentiti per l'account di Azure Cosmos.

## <a id="configure-ip-firewall-arm"></a>Configurare il firewall IP usando il modello di Resource Manager

Per configurare il controllo di accesso al proprio account Azure Cosmos, il modello di Resource Manager deve specificare l'attributo **ipRangeFilter** con un elenco di intervalli IP, che deve essere inserito nell'elenco elementi consentiti. Ad esempio, aggiungere il codice JSON seguente al modello:

```json
   {
     "apiVersion": "2015-04-08",
     "type": "Microsoft.DocumentDB/databaseAccounts",
     "kind": "GlobalDocumentDB",
     "name": "[parameters('databaseAccountName')]",
     "location": "[resourceGroup().location]",
     "properties": {
     "databaseAccountOfferType": "Standard",
     "name": "[parameters('databaseAccountName')]",
     "ipRangeFilter":"183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
   }
```

## <a id="configure-ip-firewall-cli"></a>Configurazione dei criteri di controllo di accesso IP usando l'interfaccia della riga di comando di Azure

Il comando seguente illustra come creare un account Azure Cosmos con controllo di accesso IP: 

```azurecli-interactive

name="<Azure Cosmos account name>"
resourceGroupName="<Resource group name>"

az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

Per aggiornare le impostazioni del firewall per un account esistente, eseguire il comando seguente:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="troubleshoot-ip-firewall"></a>Come risolvere i problemi relativi ai criteri di controllo di accesso IP

È possibile risolvere i problemi relativi ai criteri di controllo accesso IP usando le opzioni seguenti: 

### <a name="azure-portal"></a>Portale di Azure 
Abilitando i criteri di controllo dell'accesso agli indirizzi IP per l'account di Azure Cosmos, qualsiasi richiesta all'account da computer non inclusi nell'elenco degli intervalli di indirizzi IP consentiti viene bloccato. Se si vogliono abilitare operazioni sul piano dati del portale, ad esempio l'esplorazione di contenitori e le query nei documenti, è quindi necessario consentire esplicitamente l'accesso al portale di Azure usando il riquadro **Firewall** nel portale.

### <a name="sdks"></a>SDK 
Quando si accede a risorse di Azure Cosmos usando gli SDK da computer che non sono presenti nell'elenco consentito, **viene restituito una risposta generica di tipo 404 Non trovato senza altri dettagli**. Verificare l'elenco di IP consentiti per l'account e assicurarsi che la configurazione corretta dei criteri venga applicata all'account Cosmos. 

### <a name="check-source-ips-in-blocked-requests"></a>Verificare gli IP di origine nelle richieste bloccate
Abilitare la funzionalità di registrazione diagnostica nell'account Azure Cosmos, questi log mostreranno ogni richiesta e risposta. **I messaggi relativi al firewall vengono registrati internamente con un codice di errore 403**. Filtrando questi messaggi, è possibile visualizzare gli IP di origine per le richieste bloccate. Vedere [Registrazione diagnostica di Azure Cosmos DB](logging.md).

### <a name="requests-from-subnet-with-service-endpoint-for-azure-cosmos-database-enabled"></a>Richieste provenienti da una subnet con endpoint di servizio per Azure Cosmos database abilitato
Le richieste provenienti da una subnet nella rete virtuale che dispone di un endpoint del servizio per Azure Cosmos DB abilitato invia la rete virtuale e l'identità del subnet per gli account Azure Cosmos. Queste richieste non dispongono dell'indirizzo IP pubblico dell'origine perciò vengono rifiutati dai filtri IP. Per consentire l'accesso da subnet specifiche nelle reti virtuali, aggiungere l'elenco di controllo di accesso della rete virtuale descritta in [Come configurare la rete virtuale e l'accesso basato su subnet per l'account Azure Cosmos](how-to-configure-vnet-service-endpoint.md). L'applicazione delle regole del firewall può richiedere fino a 15 minuti.


## <a name="next-steps"></a>Passaggi successivi

Per configurare l'endpoint del servizio della rete virtuale per l'account Azure Cosmos DB, vedere gli articoli seguenti:

* [Controllo di accesso di rete virtuale e subnet per l'account Azure Cosmos](vnet-service-endpoint.md)
* [Come configurare l'accesso basato su rete virtuale e subnet per l'account Azure Cosmos](how-to-configure-vnet-service-endpoint.md)


