---
title: Scaricare gli strumenti di Azure Stack da GitHub | Microsoft Docs
description: Informazioni su come scaricare gli strumenti necessari per l'uso di Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 09/28/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 5b90ebc554738c89816cf88f8984ffab01c87d4d
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451320"
---
# <a name="download-azure-stack-tools-from-github"></a>Scaricare gli strumenti di Azure Stack da GitHub

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

**Gli strumenti di AzureStack** è un [repository di GitHub](https://github.com/Azure/AzureStack-Tools) che ospita i moduli di PowerShell per la gestione e distribuzione delle risorse in Azure Stack. Se si prevede di stabilire la connettività VPN, è possibile scaricare questi moduli di PowerShell per Azure Stack Development Kit o per un client esterno basato su Windows. Per ottenere questi strumenti, clonare il repository GitHub o scaricare il **AzureStack-Tools** cartella eseguendo lo script seguente:

```PowerShell
# Change directory to the root directory. 
cd \

# Download the tools archive.
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>Funzionalità fornite dai moduli

Il **AzureStack-Tools** repository contiene i moduli di PowerShell che supportano le funzionalità seguenti per Azure Stack:  

| Funzionalità | DESCRIZIONE | Chi può usare questo modulo? |
| --- | --- | --- |
| [Funzionalità del cloud](user/azure-stack-validate-templates.md) | Usare questo modulo per ottenere le funzionalità cloud di un cloud. Con questo modulo, ad esempio, è possibile ottenere le funzionalità cloud come versione dell'API e risorse di Azure Resource Manager. È anche possibile ottenere le estensioni di macchina virtuale per Azure Stack e cloud di Azure con questo modulo. | Gli utenti e agli operatori cloud |
| [Criteri di Resource Manager per Azure Stack](user/azure-stack-policy-module.md) | Usare questo modulo per configurare una sottoscrizione di Azure o un gruppo di risorse di Azure con la stessa disponibilità di servizio e controllo delle versioni di Azure Stack. | Gli utenti e agli operatori cloud |
| [Registrazione in Azure](azure-stack-register.md) | Usare questo modulo per registrare l'istanza del kit di sviluppo con Azure. Dopo la registrazione, è possibile scaricare gli elementi di marketplace di Azure e usarle in Azure Stack. | Operatori cloud |
| [Distribuzione di Azure Stack](azure-stack-run-powershell-script.md) | Usare questo modulo per preparare il computer host Azure Stack per distribuire e ridistribuire con l'immagine di disco rigido virtuale (VHD) di Azure Stack. | Operatori cloud|
| [La connessione ad Azure Stack](azure-stack-connect-powershell.md) | Usare questo modulo per configurare la connettività VPN ad Azure Stack. | Gli utenti e agli operatori cloud |
| [Validator del modello](user/azure-stack-validate-templates.md) | Usare questo modulo per verificare se un modello nuovo o esistente può essere distribuito in Azure Stack. | Gli utenti e agli operatori cloud|


## <a name="next-steps"></a>Passaggi successivi
* [Configurare l'ambiente di PowerShell dell'utente Azure Stack](user/azure-stack-powershell-configure-user.md)   
* [Connettersi a Azure Stack Development Kit su una rete VPN](azure-stack-connect-azure-stack.md)  
