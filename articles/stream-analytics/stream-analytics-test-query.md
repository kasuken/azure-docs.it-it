---
title: Testare un processo di Analisi di flusso di Azure con dati di esempio
description: Informazioni su come testare le query nei processi di analisi di flusso.
keywords: Questo articolo descrive come usare il portale di Azure per testare un processo di Analisi di flusso di Azure, un input di esempio e caricare i dati di esempio.
services: stream-analytics
author: jasonwhowell
ms.author: mamccrea
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: 139ad76826f3e67577beba0369f6d4425e242af9
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978350"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Testare una query di Analisi di flusso con dati di esempio

Analisi di flusso di Azure consente di caricare i dati di esempio e testare le query nel portale di Azure senza dover avviare o arrestare un processo.

## <a name="upload-sample-data-and-test-the-query"></a>Caricare i dati di esempio e testare la query

1. Accedere al portale di Azure. 

2. Individuare il processo di Analisi di flusso di Azure esistente e selezionarlo.

3. Nella pagina del processo di Analisi di flusso selezionare **Query** nell'intestazione **Topologia processo** per aprire la finestra dell'editor Query. 

4. Per eseguire il test della query con dati di input di esempio, fare clic con il pulsante destro del mouse su uno degli input.  Selezionare quindi **Upload sample data from file** (Carica dati di esempio da file). I dati devono essere serializzati in JSON, CSV o AVRO. L'esempio di input deve essere codificato in UTF-8 e non compresso. Solo i delimitatori di virgola (,) sono supportati per testare gli input CSV nel portale.

    ![Pulsante per il testo delle query nell'editor query della funzione di analisi di flusso](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Dopo avere completato il caricamento selezione **Test** per testare la query usando i dati di esempio indicati.

    ![dati di esempio per il test nell'editor query della funzione di analisi di flusso](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Se si desidera usare i risultati del test in futuro, l'output della query viene visualizzato nel browser con un collegamento ai risultati del download. 

7. Modificare in modalità iterativa la query e testarla di nuovo per vedere come cambia l'output.

   ![Output di esempio dell'editor query della funzione di analisi di flusso](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Quando si usano più output in una query, è possibile visualizzarne i risultati in schede separate e passare facilmente da un output all'altro.

8. Dopo aver verificato i risultati mostrati nel browser **salvare** la query. Quindi **avviare** il processo e attendere che gli eventi in ingresso vengano elaborati.

## <a name="next-steps"></a>Passaggi successivi
> [!div class="nextstepaction"]
> [Informazioni di riferimento sul linguaggio di query di Analisi di flusso di Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
