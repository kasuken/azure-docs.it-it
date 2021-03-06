---
title: "Guida introduttiva: Rilevare i visi in un'immagine con l'API REST di Azure e Python"
titleSuffix: Azure Cognitive Services
description: In questa guida introduttiva si userà l'API REST Viso di Azure con Python per rilevare i visi in un'immagine.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 11/09/2018
ms.author: pafarley
ms.openlocfilehash: d2a98226895bbe5996785ca4726f20df9b98ffdd
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683611"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-python"></a>Guida introduttiva: Rilevare i visi in un'immagine con l'API REST Viso e Python

In questa guida introduttiva si userà l'API REST Viso di Azure con Python per rilevare i visi umani in un'immagine. Lo script disegna cornici intorno ai visi, sovrapponendo informazioni su sesso ed età sull'immagine.

![Un uomo e una donna, con rettangoli disegnati intorno ai visi e informazioni su età e sesso visualizzate sull'immagine](../images/labelled-faces-python.png)

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare. 


## <a name="prerequisites"></a>Prerequisiti

- Una chiave di sottoscrizione API Viso. È possibile ottenere una chiave di sottoscrizione della versione di valutazione gratuita da [Prova Servizi cognitivi](https://azure.microsoft.com/try/cognitive-services/?api=face-api). In alternativa, seguire le istruzioni in [Creare un account Servizi cognitivi](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) per effettuare la sottoscrizione al servizio API Viso e ottenere la chiave.

## <a name="run-the-jupyter-notebook"></a>Avviare il notebook Jupyter

È possibile eseguire questa guida introduttiva come Jupyter Notebook in [MyBinder](https://mybinder.org). Per avviare Binder, selezionare il pulsante seguente. Quindi, seguire le istruzioni contenute nel notebook.

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=FaceAPI.ipynb)

## <a name="next-steps"></a>Passaggi successivi

Successivamente, esplorare la documentazione di riferimento dell'API Viso per altre informazioni sugli scenari supportati.

> [!div class="nextstepaction"]
> [API Viso](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
