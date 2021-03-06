---
title: Rendere disponibile contenuto di Archiviazione di Azure nel servizio app in Linux
description: Come configurare e rendere disponibile contenuto di Archiviazione di Azure nel servizio app di Azure in Linux.
author: msangapu
manager: jeconnoc
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 11/01/2018
ms.author: msangapu
ms.openlocfilehash: 8d4444aac7f84753f55c434d0a3f5ef0edcfb1c4
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980241"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Rendere disponibile contenuto di Archiviazione di Azure nel servizio app in Linux

Questa guida illustra come rendere disponibile contenuto statico nel servizio app in Linux usando [Archiviazione di Azure](/azure/storage/common/storage-introduction). I vantaggi includono la protezione e la portabilità del contenuto, l'accesso a più app e molteplici metodi di trasferimento. In questa guida si apprende come rendere disponibile contenuto di Archiviazione di Azure [configurando l'archiviazione personalizzata](https://blogs.msdn.microsoft.com/appserviceteam/2018/09/24/announcing-bring-your-own-storage-to-app-service/).

## <a name="prerequisites"></a>Prerequisiti

- Un'app Web esistente (servizio app in Linux o app Web per contenitori)
- [Interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli) (2.0.46 o successiva)

## <a name="create-azure-storage"></a>Creare una risorsa di archiviazione di Azure

> [!NOTE]
> Archiviazione di Azure è una risorsa di archiviazione non predefinita e fatturata separatamente, non inclusa nell'app Web.
>

Creare un [account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli).

```azurecli
#Create Storage Account
az storage account create --name <storage_account_name> --resource-group myResourceGroup

#Create Storage Container
az storage container create --name <storage_container_name> --account-name <storage_account_name>
```

## <a name="upload-files-to-azure-storage"></a>Caricare file in Archiviazione di Azure

Per caricare una directory locale nell'account di archiviazione, usare il comando [ `az storage blob upload-batch` ](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest#az-storage-blob-upload-batch) come nell'esempio seguente:

```azurecli
az storage blob upload-batch -d <full_path_to_local_directory> --account-name <storage_account_name> --account-key "<access_key>" -s <source_location_name>
```

## <a name="link-storage-to-your-web-app"></a>Collegare la risorsa di archiviazione all'app Web

> [!CAUTION]
> Collegando una directory esistente in un'app Web a un account di archiviazione il contenuto della directory verrà eliminato. Se si esegue la migrazione dei file di un'app esistente, creare un copia di backup dell'app e del relativo contenuto prima di iniziare.
>

Per montare un account di archiviazione in una directory dell'app servizio app, usare il comando [ `az webapp config storage-account add` ](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add). Il tipo di archiviazione può essere AzureBlob o AzureFiles. Per questo contenitore usare AzureBlob.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureBlob --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

È consigliabile eseguire questa operazione per tutte le altre directory che si vogliono collegare a un account di archiviazione.

## <a name="verify"></a>Verificare

Dopo aver collegato un contenitore di archiviazione a un'app Web, è possibile verificare il collegamento eseguendo il comando seguente:

```azurecli
az webapp conf storage-account list --resource-group <group_name> --name <app_name>
```

## <a name="next-steps"></a>Passaggi successivi

- [Configurazione delle app Web in Servizio app di Azure](https://docs.microsoft.com/azure/app-service/web-sites-configure)
