---
title: Guida introduttiva di Azure - Creare un BLOB nell'archivio oggetti con il portale di Azure | Microsoft Docs
description: In questa guida introduttiva si usa il portale di Azure con l'archivio oggetti (BLOB). Si usa quindi il portale di Azure per caricare un BLOB in Archiviazione di Azure, scaricare un BLOB ed elencare i BLOB presenti in un contenitore.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: tamram
ms.openlocfilehash: 84753f2c3ab19a0cc9d72ef8ce5011dfc8e5a8da
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51711889"
---
# <a name="quickstart-upload-download-and-list-blobs-with-the-azure-portal"></a>Guida introduttiva: Caricare, scaricare ed elencare BLOB con il portale di Azure

In questa guida introduttiva si apprenderà come usare il [portale di Azure](https://portal.azure.com/) per creare un contenitore in Archiviazione di Azure e per caricare e scaricare BLOB in blocchi in tale contenitore.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

## <a name="create-a-container"></a>Creare un contenitore

Per creare un contenitore nel portale di Azure, seguire questa procedura:

1. Passare al nuovo account di archiviazione nel portale di Azure.
2. Nel menu a sinistra per l'account di archiviazione scorrere fino alla sezione **Servizio BLOB** e quindi selezionare **BLOB**.
3. Selezionare il pulsante **+ Contenitore**.
4. Immettere un nome per il nuovo contenitore. Il nome del contenitore deve essere composto da lettere minuscole, deve iniziare con una lettera o un numero e può contenere solo lettere, numeri e trattino (-). Per altre informazioni sui nomi di contenitori e BLOB, vedere [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Denominazione e riferimento a contenitori, BLOB e metadati).
5. Impostare il livello di accesso pubblico del contenitore. Il livello predefinito è **Private (no anonymous access)** (Privato - nessun accesso anonimo).
6. Fare clic su **OK** per creare il contenitore.

    ![Screenshot che mostra come creare un contenitore nel portale di Azure](media/storage-quickstart-blobs-portal/create-container.png)

## <a name="upload-a-block-blob"></a>Caricare un BLOB in blocchi

I BLOB in blocchi sono costituiti da blocchi di dati combinati per creare un BLOB. La maggior parte degli scenari di archiviazione BLOB usa BLOB in blocchi. I BLOB in blocchi sono ideali per l'archiviazione di dati di testo e binari nel cloud, ad esempio file, immagini e video. Questa guida introduttiva spiega come usare i BLOB in blocchi. 

Per caricare un BLOB in blocchi nel nuovo contenitore nel portale di Azure, seguire questa procedura:

1. Nel portale di Azure passare al contenitore creato nella sezione precedente.
2. Selezionare il contenitore per visualizzare un elenco dei BLOB contenuti. Poiché questo contenitore è nuovo, non conterrà ancora alcun BLOB.
3. Selezionare il pulsante **Carica** per caricare un BLOB nel contenitore.
4. Cercare nel file system locale un file da caricare come BLOB in blocchi e selezionare **Carica**.
     
    ![Screenshot che mostra come caricare un BLOB dall'unità locale](media/storage-quickstart-blobs-portal/upload-blob.png)

5. Selezionare il **Tipo di autenticazione**. Il valore predefinito è **SAS**.
6. Caricare tutti i BLOB desiderati in questo modo. Si noterà che i nuovi BLOB sono ora elencati all'interno del contenitore.

## <a name="download-a-block-blob"></a>Scaricare un BLOB in blocchi

È possibile scaricare un BLOB in blocchi per visualizzarlo nel browser o salvarlo nel file system locale. Per scaricare un BLOB in blocchi, seguire questa procedura:

1. Passare all'elenco di BLOB caricati nella sezione precedente. 
2. Fare clic con il pulsante destro del mouse sul BLOB che si vuole scaricare e selezionare **Scarica**. 

## <a name="clean-up-resources"></a>Pulire le risorse

Per rimuovere le risorse create in questa guida introduttiva, è possibile eliminare il contenitore. Verranno eliminati anche tutti i BLOB presenti nel contenitore.

Per eliminare il contenitore:

1. Nel portale di Azure passare all'elenco dei contenitori presenti nell'account di archiviazione.
2. Selezionare il contenitore da eliminare.
3. Selezionare il pulsante **Altro** (**...**) e scegliere **Elimina**.
4. Confermare di voler eliminare il contenitore.

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva è stato descritto il trasferimento di file tra un disco locale e Archiviazione BLOB di Azure con il portale di Azure. Per altre informazioni sull'uso dell'archiviazione BLOB, continuare la procedura relativa all'archiviazione BLOB.

> [!div class="nextstepaction"]
> [Procedura relativa alle operazioni di archiviazione BLOB](storage-dotnet-how-to-use-blobs.md)

