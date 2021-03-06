---
title: Note sulla versione di Azure Stack SQL resource provider 1.1.30.0 | Microsoft Docs
description: Informazioni sulle novità nell'aggiornamento più recente SQL di Azure Stack resource provider, inclusi i problemi noti e posizione di download.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: d2dda25c63a6e4a73205b9e4a830a211d025b3ed
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688205"
---
# <a name="sql-resource-provider-11300-release-notes"></a>Note sulla versione SQL resource provider 1.1.30.0

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

Queste note sulla versione vengono descritti i miglioramenti e problemi noti nella versione del provider di risorse 1.1.30.0 SQL.

## <a name="build-reference"></a>Riferimento alla compilazione
Scaricare il provider di risorse SQL binario e quindi eseguire il programma di autoestrazione per estrarre il contenuto in una directory temporanea. Il provider di risorse dispone di uno Stack di Azure corrispondente minimo di compilazione. La versione minima dello Stack di Azure necessaria per installare questa versione del provider di risorse SQL è elencata di seguito:

> |Versione minima di Azure Stack|Versione del provider di risorse SQL|
> |-----|-----|
> |Versione 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
> |     |     |

> [!IMPORTANT]
> Applicare l'aggiornamento di Azure Stack supportato minimo per il sistema integrato Azure Stack o distribuire più recente Azure Stack Development Kit (ASDK) prima di distribuire la versione più recente del provider di risorse SQL.

## <a name="new-features-and-fixes"></a>Nuove funzionalità e correzioni
Questa versione del provider di risorse SQL di Azure Stack include le correzioni e i miglioramenti seguenti:

- **I dati di telemetria abilitata per le distribuzioni del provider di risorse SQL**. Raccolta dati di telemetria è stata abilitata per le distribuzioni del provider di risorse SQL. Dati di telemetria raccolti includono distribuzione del provider di risorse, avviare e arrestare i tempi, uscire da stato, messaggi di chiusura e i dettagli dell'errore (se applicabile).

- **Aggiornamento di crittografia TLS 1.2**. TLS 1.2 di solo supporto abilitato per la comunicazione di provider di risorse con i componenti interni di Azure Stack. 

### <a name="fixes"></a>Correzioni

- **SQL resource provider PowerShell per Azure Stack compatibilità**. Il provider di risorse SQL è stato aggiornato a con il profilo di Stack 2018-03-01-ibrida di Azure PowerShell e per garantire la compatibilità con AzureRM 1.3.0 aziendali e versioni successive.

- **Pannello SQL login change password**. Risolto un problema in cui la password non può essere modificata nel pannello Modifica password. Le notifiche di modifica collegamenti rimossi dalla password.

- **Aggiornamento del pannello delle impostazioni server di hosting SQL**. Risolto un problema in cui il pannello delle impostazioni è stato denominato in modo non corretto come "Password".

## <a name="known-issues"></a>Problemi noti 

- **SKU SQL può richiedere fino a un'ora siano visibili nel portale di**. Possono trascorrere a un'ora per i nuovi SKU creata sia visibile per l'uso durante la creazione di nuovi database SQL. 

    **Soluzione alternativa**: Nessuno.

- **Riutilizzare gli account di accesso SQL**. Tentativo di creare un nuovo SQL l'accesso con lo stesso nome utente come un account di accesso esistente nella stessa sottoscrizione comporterà riutilizzo all'account di accesso e la password esistente. 

    **Soluzione alternativa**: utilizzare nomi utente diversi durante la creazione di nuovi account di accesso nella stessa sottoscrizione o creare gli account di accesso con lo stesso nome utente in diverse sottoscrizioni.

- **Gli account di accesso condiviso di SQL causare un'incoerenza dei dati**. Se un account di accesso SQL condiviso per più database SQL nella stessa sottoscrizione, la modifica della password di account di accesso causerà un'incoerenza dei dati.

    **Soluzione alternativa**: usare sempre gli account di accesso diversi per i database diversi nella stessa sottoscrizione.

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Problemi noti per gli amministratori Cloud Azure Stack operativo
Vedere la documentazione nel [note sulla versione di Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Passaggi successivi
[Altre informazioni sul provider di risorse SQL](azure-stack-sql-resource-provider.md).

[Preparare la distribuzione il provider di risorse SQL](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[Aggiornare il provider di risorse SQL da una versione precedente](azure-stack-sql-resource-provider-update.md). 