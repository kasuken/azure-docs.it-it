---
title: "Esempio di script dell'interfaccia della riga di comando di Azure: creare un'istanza di Cache Redis di Azure | Microsoft Docs"
description: "Esempio di script dell'interfaccia della riga di comando di Azure: creare un'istanza di Cache Redis di Azure"
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: afd7f6e0-9297-4c98-a95e-597be939cef7
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: f65ecde40e3b5ba6dc935a6f61c321c14abe4ea9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2018
ms.locfileid: "29849396"
---
# <a name="create-an-azure-redis-cache"></a>Creare una cache Redis di Azure

In questo scenario viene illustrato come creare un'istanza di Cache Redis di Azure.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script di esempio

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-cache/create-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti per creare un gruppo di risorse e un'istanza di Cache Redis. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Consente di creare un gruppo di risorse in cui sono archiviate tutte le risorse. |
| [az redis create](https://docs.microsoft.com/cli/azure/redis#az_redis_create) | Creare un'istanza di Cache Redis. |


## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere la [documentazione sull'interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure).

Altri esempi di script dell'interfaccia della riga di comando della Cache Redis di Azure sono disponibili nella [documentazione della Cache Redis di Azure](../cli-samples.md).