---
title: Controllare l'integrità di un cluster di Esplora dati di Azure
description: Questo articolo descrive i passaggi per determinare se un cluster di Esplora dati di Azure è integro.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: d07873b34a41ff20b5007a88743f6b150d4d8a3d
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212829"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Controllare l'integrità di un cluster di Esplora dati di Azure

Ci sono diversi fattori che influiscono sull'integrità di un cluster di Esplora dati di Azure, tra cui CPU, memoria e sottosistema del disco. Questo articolo illustra alcuni passaggi di base che è possibile eseguire per misurare l'integrità di un cluster.

1. Accedere a [https://dataexplorer.azure.com](https://dataexplorer.azure.com).

1. Nel riquadro sinistro selezionare un cluster ed eseguire il comando seguente.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Un output pari a 1 è integro; un output pari a 0 è non integro.

1. Accedere al [portale di Azure](https://portal.azure.com) e passare al cluster.

1. In **Monitoraggio** selezionare **Metrica**, quindi selezionare **Keep-alive**, come illustrato nell'immagine seguente. Un output vicino a 1 indica un cluster integro.

    ![Metrica keep-alive del cluster](media/check-cluster-health/portal-metrics.png)

1. È possibile aggiungere altre metriche al grafico. Selezionare il grafico quindi **Aggiungi metrica**. Selezionare un'altra metrica: in questo esempio viene mostrato **CPU**.

    ![Aggiungere una metrica](media/check-cluster-health/add-metric.png)

1. Se occorre assistenza per la diagnosi dei problemi con l'integrità di un cluster, aprire una richiesta di supporto nel [portale di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).