---
title: Connettersi ad Azure Analysis Services con Excel | Microsoft Docs
description: Informazioni su come connettersi a un server di Azure Analysis Services usando Excel.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2a9646ad2daed5ebc224d6d0b97f1f42c7da101e
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49425853"
---
# <a name="connect-with-excel"></a>Connettersi con Excel

Dopo aver creato un server e aver distribuito un modello tabulare nel server, i client possono connettersi e iniziare l'esplorazione dei dati. 

## <a name="before-you-begin"></a>Prima di iniziare
L'account con cui si esegue l'accesso deve appartenere a un ruolo del database modello con almeno le autorizzazioni di lettura. Per altre informazioni, vedere [Autenticazione e autorizzazioni utente](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Connettersi in Excel

La connessione a un server in Excel è supportata tramite Dati in Excel 2016. La connessione tramite l'Importazione guidata tabella in Power Pivot non è supportata. 

**Per connettersi in Excel 2016**

1. In Excel 2016, sulla barra multifunzione **Dati**, fare clic su **Recupera dati esterni** > **Da altre origini** > **Da Analysis Services**.

2. Nella Connessione guidata dati, in **Nome Server**, immettere il nome del server, inclusi il protocollo e l'URI. Ad esempio, asazure://westcentralus.asazure.windows.net/advworks. In **Credenziali di accesso** selezionare **Usa nome utente e password seguenti** e quindi digitare il nome utente dell'organizzazione, ad esempio nancy@adventureworks.com, e la password.

    > [!IMPORTANT]
    > Se si accede con un account Microsoft, con un Live ID o con Yahoo, Gmail e così via, o se si deve accedere tramite autenticazione a più fattori, lasciare vuoto il campo della password. Dopo aver fatto clic su Avanti, è necessario specificare la password. 

    ![Connettersi dall'accesso a Excel](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. In **Seleziona database e tabella** selezionare il database e il modello o la prospettiva e quindi fare clic su **Fine**.
   
    ![Connettersi dal modello di selezione in Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Vedere anche 
[Librerie client](analysis-services-data-providers.md)   
[Gestire il server](analysis-services-manage.md)     


