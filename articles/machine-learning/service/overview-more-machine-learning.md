---
title: Confrontare le opzioni dei prodotti di machine learning di Microsoft - Azure | Microsoft Docs
description: Confrontare la varietà di prodotti Microsoft per compilare, distribuire e gestire i propri modelli di Machine Learning. Decidere quali prodotti scegliere per la soluzione.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 09/24/2018
ms.openlocfilehash: 182504373795b3cb0f2794acbed5e253ac6bc95c
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419560"
---
# <a name="what-are-the-machine-learning-product-options-from-microsoft"></a>Informazioni sulle opzioni di machine learning fornite da Microsoft

Microsoft offre un'ampia gamma di prodotti per compilare, distribuire e gestire i propri modelli di machine learning. Confrontare questi prodotti e scegliere ciò che è necessario per sviluppare le soluzioni di machine learning nel modo più efficace.

| Prodotto di machine learning | Che cos'è | Cosa permette di fare |
|-|-|-|
| Nel cloud | | |
| [Servizio Azure Machine Learning](#azure-machine-learning-services) | Servizio cloud per Machine Learning gestito  | Eseguire il training, distribuire e gestire i modelli in Azure con Python e CLI |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | Interfaccia visiva di trascinamento della selezione per Machine Learning | Compilare, provare e distribuire i modelli usando algoritmi preconfigurati |
| [Azure Databricks](#azure-databricks) | Piattaforma di analisi basata su Spark | Compilare e distribuire modelli e flussi di lavoro dei dati |
| [Servizi cognitivi di Azure](#azure-cognitive-services) | Servizi di Azure con modelli predefiniti di Machine Learning e intelligenza artificiale | È possibile aggiungere con facilità funzionalità intelligenti alle app |
| [Macchina virtuale di data science di Azure](#azure-data-science-virtual-machine) | Macchina virtuale con strumenti di data science preinstallati | Sviluppare soluzioni di Machine Learning in un ambiente preconfigurato |
| Locale | | |
| [SQL Server Machine Learning Services](#sql-server-machine-learning-services) | Motore di analisi incorporato in SQL | Creare e distribuire modelli all'interno di SQL Server |
| [Microsoft Machine Learning Server](#microsoft-machine-learning-server) | Server enterprise autonomo per l'analisi predittiva | Creare e distribuire modelli con R e Python |
| Strumenti per sviluppatori | | |
| [ML.NET](#mlnet) | SDK di Machine Learning open source, multipiattaforma | Sviluppare soluzioni di Machine Learning per le applicazioni .NET |
| [Windows ML](#windows-ml) | Piattaforma di Machine Learning per Windows 10 | Valutare modelli con training su un dispositivo Windows 10 |

## <a name="azure-machine-learning-service"></a>Servizio Azure Machine Learning

[Servizio Azure Machine Learning](overview-what-is-azure-ml.md) (anteprima) è un servizio cloud completamente gestito usato per eseguire il training, distribuire e gestire modelli di Machine Learning su larga scala. Supporta completamente le tecnologie open source e consente pertanto di usare decine di migliaia di pacchetti Python open source quali TensorFlow, PyTorch e scikit-learn. Sono inoltre disponibili strumenti avanzati, come [Azure notebooks](https://notebooks.azure.com/), [Jupyter notebooks](http://jupyter.org), o [Visual Studio Code Tools for AI](https://visualstudio.microsoft.com/downloads/ai-tools-vscode/) per semplificare l'esplorazione e la trasformazione dei dati, per poi eseguire il training e distribuire i modelli. Il servizio Azure Machine Learning include anche funzionalità che consentono di automatizzare la generazione e l'ottimizzazione dei modelli in modo semplice, efficiente e accurato.

Usare il servizio di Azure Machine Learning per eseguire il training, distribuire e gestire modelli di Machine Learning con Python e CLI su scalabilità cloud.

>[!Note]
> È possibile provare gratuitamente Azure Machine Learning. Non è necessaria una carta di credito o una sottoscrizione di Azure. Per iniziare. https://azure.microsoft.com/free/

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

[Azure Machine Learning Studio](../studio/what-is-ml-studio.md) offre un'area di lavoro interattiva e visiva utilizzabile per compilare, testare e distribuire in modo facile e rapido i modelli usando algoritmi di machine learning predefiniti. Machine Learning Studio pubblica i modelli come servizi Web che possono essere facilmente usati da applicazioni personalizzate o strumenti di Business Intelligence, ad esempio Excel.
Non è necessaria alcuna programmazione: è possibile creare il modello di machine learning connettendo i set di dati e i moduli di analisi in una canvas interattiva, per poi distribuirlo con pochi clic.

Usare Machine Learning Studio quando si desidera sviluppare e distribuire modelli che non richiedono alcun codice.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/what-is-azure-databricks) è una piattaforma di analisi basata su Apache Spark ottimizzata per la piattaforma dei servizi cloud di Microsoft Azure. Databricks è integrato con Azure per offrire l'installazione con un clic, flussi di lavoro semplificati e un'area di lavoro interattiva che consente la collaborazione tra data scientist, ingegneri dei dati e business analyst.
Usare i codici Python, R, Scala e SQL nei notebook basati sul Web per eseguire una query, visualizzare e modellare i dati.

Usare Databricks quando si desidera collaborare alla creazione di soluzioni di machine learning in Spark Apache.

## <a name="azure-cognitive-services"></a>Servizi cognitivi di Azure

[Servizi cognitivi di Azure](/azure/cognitive-services/welcome) è un set di API che consente di creare app che usano metodi di comunicazione naturali. Queste API consentono alle app di vedere, sentire, parlare, comprendere e interpretare le esigenze degli utenti con poche righe di codice. È possibile aggiungere con facilità funzionalità intelligenti alle app, ad esempio: 

- Rilevamento di emozioni e sentiment
- Riconoscimento visivo e vocale
- Comprensione del linguaggio (LUIS, Language Understanding)
- Conoscenza e ricerca

Usare i Servizi cognitivi per sviluppare app tra dispositivi e piattaforme diverse. Le API vengono migliorate continuamente e sono facili da installare.

## <a name="azure-data-science-virtual-machine"></a>Macchina virtuale di data science di Azure

[Macchina virtuale di data science di Azure](../data-science-virtual-machine/overview.md) è un ambiente di macchina virtuale personalizzato nel cloud di Microsoft Azure, creato in modo specifico per l'analisi scientifica dei dati. Include diversi strumenti comuni per l'analisi scientifica dei dati e altri strumenti preinstallati e preconfigurati per implementare rapidamente la creazione di applicazioni intelligenti per l'analisi avanzata.
La macchina virtuale di data science è disponibile nelle versioni per Windows e Linux Ubuntu (il servizio Azure Machine Learning non è supportato per Linux CentOS).
Per informazioni sulla versione specifica e un elenco di quanto  incluso, vedere [Introduzione a Data Science Virtual Machine di Azure](../data-science-virtual-machine/overview.md).
La macchina virtuale di data science è supportata come destinazione per il servizio Machine Learning di Azure.

Usare la macchina virtuale di data science quando è necessario eseguire oppure ospitare processi in un singolo nodo. o se è necessario aumentare in modo remoto le prestazioni di elaborazione in un singolo computer.

## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Services

[SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) aggiunge analisi statistiche, visualizzazione dei dati e analisi predittiva in R e Python per i dati relazionali nel database SQL Server. Le librerie di R e Python di Microsoft includono la creazione di modelli avanzata e algoritmi di Machine Learning, che possono essere eseguiti in parallelo e su larga scala, in SQL Server.

Usare SQL Server Machine Learning Services quando si necessita di intelligenza artificiale incorporata e di analisi predittive su dati relazionali in SQL Server.

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server

[Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) è un server aziendale per l'hosting e la gestione di carichi di lavoro paralleli e distribuiti di processi R e Python. Microsoft Machine Learning Server viene eseguito su Linux, Windows, Hadoop e Apache Spark ed è disponibile anche su [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Fornisce un motore di esecuzione per le soluzione create tramite [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), e [pacchetti MicrosoftML](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package), ed estende R open source e Python con il supporto per analisi ad alte prestazioni, analisi statistiche, machine learning e set di dati di grandi dimensioni. La funzionalità viene fornita tramite pacchetti proprietari installati insieme al server. Per lo sviluppo è possibile usare IDE come [R Tools per Visual Studio](https://www.visualstudio.com/vs/rtvs/) e [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python/).

Usare Microsoft Machine Learning Server quando è necessario compilare e rendere operativi i modelli creati con R e Python in un server, o distribuire training R e Python su larga scala in un cluster Hadoop o Spark.

## <a name="mlnet"></a>ML.NET

[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/) è un framework di machine learning gratuito, open source e multipiattaforma che consente di compilare soluzioni personalizzate di machine learning e di integrarle nelle applicazioni .NET.

Usare ML.NET quando si desidera integrare soluzioni di machine learning nelle applicazioni .NET.

## <a name="windows-ml"></a>Windows ML

[Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/) consente di usare modelli di Machine Learning sottoposti a training nelle proprie applicazioni, valutando i modelli sottoposti a training in locale su dispositivi Windows 10.

Usare Windows ML quando si desidera utilizzare modelli di Machine Learning sottoposti a training all'interno delle proprie applicazioni Windows.

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni su tutti i prodotti di sviluppo di intelligenza artificiale disponibili in Microsoft, vedere la[Piattaforma di intelligenza artificiale di Microsoft](https://www.microsoft.com/ai)
- Per il training sulle modalità di sviluppo di soluzioni di intelligenza artificiale, vedere [Microsoft AI School](https://aischool.microsoft.com/learning-paths)
