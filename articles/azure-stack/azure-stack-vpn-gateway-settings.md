---
title: Impostazioni del gateway VPN di Azure Stack | Microsoft Docs
description: Informazioni sulle impostazioni per i gateway VPN che si usa con Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2018
ms.author: sethm
ms.openlocfilehash: 2fa062621e551ce7182facc45ec84b39d4c2dad7
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078750"
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>Impostazioni di configurazione di gateway VPN di Azure Stack

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

Un gateway VPN è un tipo di gateway di rete virtuale che invia traffico crittografato tra la rete virtuale in Azure Stack e un gateway VPN remoto. Il gateway VPN remoto può essere in Azure, un dispositivo nel Data Center o un dispositivo in un altro sito.  Se c'è connettività di rete tra due endpoint, è possibile stabilire una connessione VPN Site-to-Site (S2S) sicura tra le due reti.

Una connessione di gateway VPN si basa sulla configurazione di più risorse, ognuna delle quali contiene impostazioni configurabili. Questo articolo illustra le risorse e le impostazioni correlate a un gateway VPN per una rete virtuale creata nel modello di distribuzione Resource Manager. È possibile trovare i diagrammi di topologia e le descrizioni per ogni soluzione di connessione in [informazioni sul Gateway VPN di Azure Stack](azure-stack-vpn-gateway-about-vpn-gateways.md).

## <a name="vpn-gateway-settings"></a>Impostazioni del gateway VPN

### <a name="gateway-types"></a>Tipi di gateway

Ogni rete virtuale di Azure Stack supporta un gateway di rete virtuale singola, che deve essere di tipo **Vpn**.  Questo supporto è diverso da Azure, che supporta i tipi aggiuntivi.  

Quando si crea un gateway di rete virtuale, è necessario assicurarsi che il tipo di gateway sia corretto per la configurazione. È necessario un gateway VPN di `-GatewayType Vpn`, ad esempio:

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>SKU del gateway

Quando si crea un gateway di rete virtuale è necessario specificare il codice SKU del gateway da usare. Selezionare gli SKU che soddisfano i requisiti inerenti ai tipi di carichi di lavoro, alle velocità effettive, alle funzionalità e ai contratti di servizio.

Azure Stack offre il gateway VPN SKU illustrati nella tabella seguente.

|   | Velocità effettiva del Gateway VPN |Tunnel IPsec massimi del Gateway VPN |
|-------|-------|-------|
|**SKU Basic**  | 100 Mbps  | 10    |
|**SKU standard**           | 100 Mbps  | 10    |
|**SKU con prestazioni elevate** | 200 Mbps    | 5 |

### <a name="resizing-gateway-skus"></a>Ridimensionamento di SKU del gateway

Azure Stack non supporta un ridimensionamento degli SKU tra gli SKU legacy supportati.

Analogamente, Azure Stack non supporta un ridimensionamento da uno SKU legacy supportato (Basic, Standard e HighPerformance) a uno SKU più recente supportato da Azure (VpnGw1, VpnGw2 e VpnGw3).

### <a name="configure-the-gateway-sku"></a>Configurare lo SKU del gateway

#### <a name="azure-stack-portal"></a>Portale di Azure Stack

Se si usa il portale di Azure Stack per creare un gateway di rete virtuale di Resource Manager, è possibile selezionare lo SKU del gateway usando l'elenco a discesa. Le opzioni che vengono presentati con corrispondono al tipo di Gateway e al tipo di VPN selezionato.

#### <a name="powershell"></a>PowerShell

L'esempio di PowerShell seguente specifica i **- GatewaySku** come VpnGw1.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```

### <a name="connection-types"></a>Tipi di connessione

Nel modello di distribuzione Resource Manager ogni configurazione richiede un tipo di connessione di gateway di rete virtuale specifico. I valori di PowerShell per Resource Manager disponibili per **- ConnectionType** sono:

* IPsec

Nell'esempio di PowerShell seguente, viene creata una connessione S2S che richiede il tipo di connessione IPsec.

```PowerShell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

### <a name="vpn-types"></a>Tipi di VPN

Quando si crea il gateway di rete virtuale per una configurazione di gateway VPN, è necessario specificare un tipo di VPN. Il tipo di VPN scelto dipende dalla topologia di connessione che si desidera creare.  Un tipo di VPN può anche dipendere l'hardware in uso. Le configurazioni S2S richiedono un dispositivo VPN. Alcuni dispositivi VPN supportano solo un determinato tipo di VPN.

> [!IMPORTANT]  
> Attualmente, Azure Stack supporta solo il tipo di VPN basato su Route. Se il dispositivo supporta solo le VPN basato su criteri, non sono supportate le connessioni ai dispositivi da Azure Stack.  
>
> Inoltre, Azure Stack non supporta l'uso selettori di traffico basati su criteri per i gateway basati su Route a questo punto, poiché non sono supportate le configurazioni di criteri IPSec/IKE personalizzate.

* **PolicyBased**: VPN basata su criteri crittografano e reindirizzano i pacchetti tramite tunnel IPsec basati sui criteri IPsec configurati con le combinazioni di prefissi di indirizzo tra la rete locale e la rete virtuale di Azure Stack. I criteri o selettori di traffico, in genere sono un elenco di accesso nella configurazione del dispositivo VPN.

  >[!NOTE]
  >Basato su criteri è supportato in Azure, ma non in Azure Stack.

* **RouteBased**: le VPN RouteBased usare route configurate nell'inoltro IP o tabella di routing per indirizzare i pacchetti a interfacce tunnel corrispondenti. Le interfacce tunnel consentono quindi di crittografare o decrittografare i pacchetti all'interno e all'esterno dei tunnel. I criteri o selettori di traffico, per le VPN RouteBased vengono configurati come any-to-any (o usare caratteri jolly). Per impostazione predefinita, non possono essere modificati. Il valore per un tipo di VPN RouteBased è di tipo RouteBased.

L'esempio di PowerShell seguente specifica i **- VpnType** tipo routebased. Quando si crea un gateway, è necessario assicurarsi che il **- VpnType** sia corretto per la configurazione.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```

### <a name="gateway-requirements"></a>Requisiti del gateway

Nella tabella seguente elenca i requisiti per i gateway VPN.

| |Gateway VPN PolicyBased Basic | Gateway VPN RouteBased Basic | Gateway VPN Standard RouteBased | Gateway VPN RouteBased High Performance|
|--|--|--|--|--|
| **Site-to-Site connectivity (connettività S2S)** | Non supportato | Configurazione VPN RouteBased | Configurazione VPN RouteBased | Configurazione VPN RouteBased |
| **Metodo di autenticazione**  | Non supportato | Chiave precondivisa per la connettività S2S  | Chiave precondivisa per la connettività S2S  | Chiave precondivisa per la connettività S2S  |   
| **Numero massimo di connessioni Site-to-Site**  | Non supportato | 10 | 10| 5|
|**Supporto routing attivo (BGP)** | Non supportate | Non supportate | Supportato | Supportato |

### <a name="gateway-subnet"></a>Subnet gateway

Prima di creare un gateway VPN, è necessario creare una subnet del gateway. La subnet del gateway contiene gli indirizzi IP che usano il gateway di rete virtuale le macchine virtuali e servizi. Quando si crea il gateway di rete virtuale, le VM del gateway vengono distribuite nella subnet del gateway e configurate con le impostazioni del gateway VPN necessarie. **Non** distribuire altro (ad esempio, VM aggiuntive) alla subnet del gateway.

>[!IMPORTANT]
>Per poter funzionare correttamente, la subnet del gateway deve essere denominata **GatewaySubnet** . Stack di Azure Usa questo nome per identificare la subnet in cui distribuire il gateway di rete virtuale le macchine virtuali e servizi.

Quando si crea la subnet del gateway, si specifica il numero di indirizzi IP inclusi nella subnet. Gli indirizzi IP inclusi nella subnet del gateway sono allocati alle VM del gateway e ai servizi del gateway. Alcune configurazioni richiedono più indirizzi IP di altre. Esaminare le istruzioni per la configurazione da creare e verificare che la subnet del gateway che si vuole creare soddisfi tali requisiti.

Inoltre, assicurarsi che la subnet del gateway ha un numero di indirizzi IP sufficiente per gestire altre configurazioni future. Sebbene sia possibile creare una subnet del gateway pari a/29, è consigliabile che creare una subnet del gateway/28 o più grande (/ 28, / 27, / 26 e così via.) In questo modo, se si aggiungono funzionalità in futuro, non è necessario eliminare il gateway, quindi eliminare e ricreare la subnet del gateway per consentire altri indirizzi IP.

L'esempio seguente di PowerShell Resource Manager illustra una subnet del gateway denominata GatewaySubnet. La notazione CIDR specifica /27. Questa dimensione ammette un numero di indirizzi IP sufficiente per la maggior parte delle configurazioni attualmente esistenti.

```PowerShell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Quando si usano le subnet del gateway, evitare di associare un gruppo di sicurezza di rete (NSG) alla subnet del gateway. Se si associa un gruppo di sicurezza di rete a tale subnet, il gateway VPN potrebbe smettere di funzionare come previsto. Per altre informazioni sui gruppi di sicurezza di rete, vedere [che cos'è un gruppo di sicurezza di rete?](/azure/virtual-network/virtual-networks-nsg).

### <a name="local-network-gateways"></a>Gateway di rete locali

Quando si crea una configurazione di gateway VPN di Azure, il gateway di rete locale rappresenta spesso la posizione locale. In Azure Stack rappresenta qualsiasi dispositivo VPN remoto che si trova all'esterno di Azure Stack. Potrebbe trattarsi di un dispositivo VPN nel tuo Data Center (o un Data Center remoto), o un Gateway VPN in Azure.

Assegnare un nome, l'indirizzo IP pubblico del dispositivo VPN, gateway di rete locale e specificare i prefissi degli indirizzi che si trovano nel percorso locale. Azure esamina i prefissi di indirizzo di destinazione per il traffico di rete, consulta la configurazione specificata per il gateway di rete locale e indirizza i pacchetti di conseguenza.

L'esempio di PowerShell seguente crea un nuovo gateway di rete locale:

```PowerShell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Talvolta è necessario modificare le impostazioni di gateway di rete locale. Ad esempio, quando si aggiunge o si modifica l'intervallo di indirizzi oppure se viene modificato l'indirizzo IP del dispositivo VPN. Vedere [Modificare le impostazioni del gateway di rete locale usando PowerShell](/azure/vpn-gateway/vpn-gateway-modify-local-network-gateway).

## <a name="ipsecike-parameters"></a>Parametri IPsec/IKE

Quando si configura una connessione VPN in Azure Stack, è necessario configurare la connessione a entrambe le estremità.  Se si sta configurando una connessione VPN tra Azure Stack e un dispositivo hardware, ad esempio un commutatore o un router che agisce come un Gateway VPN, tale dispositivo potrebbe essere richiesta per le impostazioni aggiuntive.

A differenza di Azure, che supporta più offerte come un iniziatore e risponditore, Azure Stack supporta sola offerta.

### <a name="ike-phase-1-main-mode-parameters"></a>Parametri della Fase 1 di IKE (Modalità principale)

| Proprietà              | Valore|
|-|-|
| Versione IKE           | IKEv2 |
|Diffie-Hellman Group   | Gruppo 2 (1024 bit) |
| Metodo di autenticazione | Chiave precondivisa |
|Algoritmi di crittografia e di hash | AES256, SHA256 |
|Durata dell'associazione di sicurezza (tempo)     | 28.800 secondi|

### <a name="ike-phase-2-quick-mode-parameters"></a>Parametri della Fase 2 di IKE (Modalità rapida)

| Proprietà| Valore|
|-|-|
|Versione IKE |IKEv2 |
|Crittografia e hash algoritmi (crittografia)     | GCMAES256|
|Crittografia e hash algoritmi (autenticazione) | GCMAES256|
|Durata dell'associazione di sicurezza (tempo)  | 27.000 secondi  |
|Durata dell'associazione di sicurezza (byte) | 33,553,408     |
|Perfect Forward Secrecy (PFS) |Nessuno<sup>vedere la nota 1</sup> |
|Rilevamento peer inattivo | Supportato|  

* *Nota 1:* prima della versione 1807, Stack di Azure Usa il valore PFS2048 per il segreto PFS (Perfect Forward).

## <a name="next-steps"></a>Passaggi successivi

[Connessione tramite ExpressRoute](azure-stack-connect-expressroute.md)
