# Panoramica
## [Informazioni sulla rete di Azure](networking-overview.md)
## Architettura
### [Data center virtuali](/azure/architecture/vdc/networking-virtual-datacenter)
### [Routing asimmetrico con più percorsi di rete](../expressroute/expressroute-asymmetric-routing.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Progettazioni di reti protette](../best-practices-network-security.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Topologia hub-spoke](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
### [Procedure consigliate per la sicurezza di rete](../security/azure-security-network-security-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Appliance virtuali di rete a disponibilità elevata](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha )
### [Combinare i metodi di bilanciamento del carico](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Ripristino di emergenza con DNS di Azure e Gestione traffico](disaster-recovery-dns-traffic-manager.md)
## Pianificare e progettare
### [Reti virtuali](../virtual-network/virtual-network-vnet-plan-design-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Connettività cross-premise: VPN](../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Connettività cross-premise: privata dedicata](../expressroute/expressroute-workflows.md?toc=%2fazure%2fnetworking%2ftoc.json)
### Interoperabilità della connettività di back-end
#### [Introduzione e installazione test](connectivty-interoperability-preface.md?toc=%2fazure%2fnetworking%2ftoc.json)
#### [Configurazione dell'installazione test](connectivty-interoperability-configuration.md?toc=%2fazure%2fnetworking%2ftoc.json)
#### [Analisi del piano di controllo](connectivty-interoperability-control-plane.md?toc=%2fazure%2fnetworking%2ftoc.json)
#### [Analisi del piano dati](connectivty-interoperability-data-plane.md?toc=%2fazure%2fnetworking%2ftoc.json)

##  Concetti
### [Reti virtuali](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Bilanciamento carico di rete](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Bilanciamento del carico dell'applicazione](../application-gateway/overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [DNS](../dns/dns-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Distribuzione del traffico basato su DNS](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Connettersi all'ambiente locale: VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Connettersi all'ambiente locale: rete dedicata](../expressroute/expressroute-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json)


# Attività iniziali
## [Creare la prima rete virtuale](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Procedure
## Connettività Internet
### [Server pubblici per il bilanciamento del carico di rete](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Server pubblici per il bilanciamento del carico dell'applicazione](../application-gateway/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Proteggere le applicazioni Web](../application-gateway/application-gateway-web-application-firewall-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Distribuire il traffico in diverse località](../traffic-manager/traffic-manager-configure-geographic-routing-method.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Connettività interna
### [Server privati per il bilanciamento del carico di rete](../load-balancer/load-balancer-get-started-ilb-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Server privati per il bilanciamento del carico dell'applicazione](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Connettere reti virtuali: stessa località](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Connettere reti virtuali: località diverse](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Connettività cross-premise
### [Creare una connessione VPN da sito a sito (IPsec/IKE)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Creare una connessione VPN da punto a sito (SSTP con certificati)](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Creare una connessione privata dedicata (ExpressRoute)](../expressroute/expressroute-howto-circuit-portal-resource-manager.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Gestione
### [Panoramica del monitoraggio della rete](network-monitoring-overview.md)
### [Verificare l'utilizzo delle risorse rispetto ai limiti di Azure](check-usage-against-limits.md)
### [Visualizzare la topologia di rete](../network-watcher/network-watcher-topology-powershell.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Script di esempio
### [Interfaccia della riga di comando di Azure](cli-samples.md)
### [Azure PowerShell](powershell-samples.md)

## Esercitazioni
### [Bilanciare il carico delle VM](../virtual-machines/linux/tutorial-load-balancer.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Connettere un computer a una rete virtuale](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

# riferimento
## [Interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/network)
## [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/?view=azurermps-3.8.0)
## [.Net](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.network?view=azuremgmtnetwork-9.1.0-preview)
## [Node.js](https://azure.microsoft.com/develop/nodejs/#azure-sdk)
## [REST](https://msdn.microsoft.com/library/mt163658.aspx)

# Risorse
## [Creare modelli](/azure/azure-resource-manager/resource-group-authoring-templates?toc=%2fazure%2fnetworking%2ftoc.json)
## [Roadmap per Azure](https://azure.microsoft.com/roadmap/?category=networking)
## [Modelli della community](https://azure.microsoft.com/resources/templates/)
## [Blog sulle reti](https://azure.microsoft.com/blog/topics/networking)
## [Prezzi](https://azure.microsoft.com/pricing)
## [Calcolatore prezzi](https://azure.microsoft.com/pricing/calculator/)
## [Disponibilità internazionale](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Video](https://azure.microsoft.com/resources/videos/index/?services=virtual-network)

