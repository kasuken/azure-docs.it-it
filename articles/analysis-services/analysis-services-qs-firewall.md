---
title: Guida introduttiva - Configurare un firewall per un server di Analysis Services in Azure | Microsoft Docs
description: Informazioni su come configurare un firewall per un'istanza del server di Analysis Services in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 96141ddeeb73c7483935d5da49b9341436c10c18
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429705"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Guida introduttiva: Configurare il firewall del server - Portale

Questa guida introduttiva consente di configurare un firewall per il server di Azure Analysis Services. L'abilitazione di un firewall e la configurazione di intervalli di indirizzi IP solo per i computer che accedono al server sono una parte importante della protezione dei dati e del server.

## <a name="prerequisites"></a>Prerequisiti

- Un server di Analysis Services nella sottoscrizione. Per altre informazioni, vedere [Guida introduttiva: Creare un server - Portale](analysis-services-create-server.md) o [Guida introduttiva: Creare un server - PowerShell](analysis-services-create-powershell.md)
- Uno o più intervalli di indirizzi IP per i computer client (se necessario).

## <a name="log-in-to-the-azure-portal"></a>Accedere al Portale di Azure 

[Accedere al portale](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Configurare un firewall

1. Fare clic sul server per aprire la pagina Panoramica. 
2. In **IMPOSTAZIONI** > **Firewall** > **Abilita firewall** fare clic su **Sì**.
3. Per consentire l'accesso DirectQuery dal servizio Power BI, in **Allow access from Power BI** (Consenti accesso da Power BI) fare clic su **Sì**.  
4. (Facoltativo) Specificare uno o più intervalli di indirizzi IP. Immettere un nome e un indirizzo IP iniziale e finale per ogni intervallo. 
5. Fare clic su **Save**.

     ![Impostazioni del firewall](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non sono più necessari, eliminare gli intervalli di indirizzi IP o disabilitare il firewall.

## <a name="next-steps"></a>Passaggi successivi
In questa guida introduttiva è stato illustrato come configurare un firewall per il server. Dopo avere creato il server e averlo protetto con un firewall, è possibile aggiungervi un modello di dati di esempio di base dal portale. Un modello di esempio è utile per avere informazioni sulla configurazione dei ruoli del database modello e sul test delle connessioni client. Per altre informazioni, continuare con l'esercitazione per l'aggiunta di un modello di esempio.

> [!div class="nextstepaction"]
> [Esercitazione: Aggiungere un modello di esempio al server](analysis-services-create-sample-model.md)
