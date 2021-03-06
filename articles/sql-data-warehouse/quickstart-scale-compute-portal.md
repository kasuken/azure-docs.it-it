---
title: 'Guida introduttiva: aumentare il numero delle risorse di calcolo in Azure SQL Data Warehouse - Portale di Azure| Microsoft Docs'
description: È possibile ridimensionare le risorse di calcolo in Azure SQL Data Warehouse dal portale di Azure, aumentandone il numero per ottenere prestazioni migliori o riducendolo per diminuire i costi.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.component: implement
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 1abafb011a445847757af3efb9e0ea4e6170408d
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285534"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Guida introduttiva: ridimensionare le risorse di calcolo in Azure SQL Data Warehouse dal portale di Azure

È possibile ridimensionare le risorse di calcolo in Azure SQL Data Warehouse dal portale di Azure, [aumentandone il numero](sql-data-warehouse-manage-compute-overview.md) per ottenere prestazioni migliori o riducendolo per diminuire i costi. 

Se non si ha una sottoscrizione di Azure, creare un account [gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="sign-in-to-the-azure-portal"></a>Accedere al portale di Azure

Accedere al [portale di Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Prima di iniziare

È possibile ridimensionare un data warehouse di cui già si dispone o fare riferimento a [Guida introduttiva: Creare e connettere - portale](create-data-warehouse-portal.md) per creare un data warehouse denominato **mySampleDataWarehouse**.  In questa Guida introduttiva viene ridimensionato **mySampleDataWarehouse**.

## <a name="scale-compute"></a>Ridimensionare le risorse di calcolo

In SQL Data Warehouse è possibile aumentare o ridurre le risorse di calcolo agendo sulle unità Data Warehouse. Nella pagina [Creare e connettere - portale](create-data-warehouse-portal.md) **mySampleDataWarehouse** è stato creato e inizializzato con 400 unità Data Warehouse. La procedura seguente modifica le unità Data Warehouse per **mySampleDataWarehouse**.

Per modificare le unità Data Warehouse:

1. Nella pagina di sinistra del portale di Azure fare clic su **SQL Data Warehouse**.
2. Selezionare **mySampleDataWarehouse** nella pagina **SQL Data Warehouse**. Verrà aperto il data warehouse.
3. Fare clic su **Scale**.

    ![Fare clici su Ridimensiona](media/quickstart-scale-compute-portal/click-scale.png)

2. Nel pannello Ridimensiona spostare il dispositivo di scorrimento a sinistra o a destra per modificare l'impostazione delle unità Data Warehouse.

    ![Spostare il dispositivo di scorrimento](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Fare clic su **Save**. Viene visualizzato un messaggio di conferma. Fare clic su **Sì** per confermare o su **No** per annullare.

    ![Fare clic su Salva.](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Passaggi successivi
Si è appreso come ridimensionare le risorse di calcolo per il data warehouse. Per altre informazioni su Azure SQL Data Warehouse, continuare con l'esercitazione per il caricamento dei dati.

> [!div class="nextstepaction"]
>[Caricare i dati in SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
