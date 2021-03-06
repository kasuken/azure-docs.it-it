---
title: Problemi comuni degli URL di firma di accesso condiviso e relative correzioni per Azure Marketplace | Microsoft Docs
description: Elenco dei problemi più comuni relativi all'uso di URI per la firma di accesso condiviso e possibili soluzioni.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/27/2018
ms.author: pbutlerm
ms.openlocfilehash: b20b1506dfcd32ea7d5bfca0847393d1652afb78
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639180"
---
# <a name="common-sas-url-issues-and-fixes"></a>Problemi comuni degli URL di firma di accesso condiviso e relative correzioni

Nella tabella seguente sono elencati alcuni dei problemi più comuni riscontrati durante l'utilizzo di firme di accesso condiviso, usate per identificare e condividere i dischi rigidi virtuali caricati per la soluzione, e le soluzioni suggerite.

| **Problema** | **Messaggio di errore** | **Correzione** | 
| --------- | ------------------- | ------- | 
| &emsp; *Errore durante la copia di immagini* |  |  |  |
| "?" non trovato nell'URL di firma di accesso condiviso | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Aggiornare l'URL di firma di accesso condiviso con gli strumenti consigliati. |
| Parametri "st" e "se" non presenti nell'URL di firma di accesso condiviso | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Aggiornare l'URL di firma di accesso condiviso con i valori **Data di inizio** e **Data di fine** corretti. | 
| "sp = rl" non presente nell'URL di firma di accesso condiviso | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | Aggiornare l'URL di firma di accesso condiviso con autorizzazioni `Read` e `List`. | 
| L'URL di firma di accesso condiviso contiene spazi nel nome del disco rigido virtuale | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Aggiornare l'URL di firma di accesso condiviso eliminando gli spazi vuoti. |
| Errore di autorizzazione dell'URL di firma di accesso condiviso | `Failure: Copying Images. Not able to download blob due to authorization error` | Rivedere e correggere il formato dell'URI di firma di accesso condiviso. Se necessario, rigenerarlo. |
| L'indicazione di data e ora dei parametri "st" e "se" dell'URL di firma di accesso condiviso non è completa | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | I parametri **Data inizio** e **Data fine** dell'URL di firma di accesso condiviso (substring `st` e ` se`) richiedono il formato datetime completo, ad esempio `11-02-2017T00:00:00Z`. Le versioni abbreviate non sono valide. Alcuni comandi dell'interfaccia della riga di comando di Azure generano valori abbreviati per impostazione predefinita. | 
|  |  |  |

Per altre informazioni, vedere [Uso delle firme di accesso condiviso](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
