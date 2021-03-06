---
title: Applicazioni per il rendering di Batch
description: Applicazioni preinstallate per il rendering di Batch
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 28acd1b7275694d38a52f14d2b2c32b79cc8183e
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/10/2018
ms.locfileid: "40036806"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>Applicazioni preinstallate nelle immagini di VM per il rendering

Con Azure Batch è possibile usare qualsiasi applicazione per il rendering. Tuttavia, le immagini di macchine virtuali (VM) di Azure Marketplace sono disponibili con applicazioni comuni preinstallate.

Dove applicabile, sono disponibili licenze con pagamento in base al consumo per le applicazioni per il rendering preinstallate. Quando viene creato un pool di Batch, è possibile specificare le applicazioni richieste e sia il costo della VM sia quello delle applicazioni verrà addebitato al minuto. I prezzi delle applicazioni sono elencati nella [pagina dei prezzi di Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

Alcune applicazioni supportano solo Windows, ma la maggior parte è supportata sia in Windows che in Linux.

## <a name="applications-on-centos-7-rendering-nodes"></a>Applicazioni nei nodi di rendering CentOS 7

* Autodesk Maya I/O 2017 Update 5 (cut 201708032230)
* Autodesk Maya I/O 2018 Update 2 (cut 201711281015)
* Autodesk Arnold per Maya 2017 (Arnold versione 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold per Maya 2018 (Arnold versione 5.0.1.4) MtoA-2.1.0.3-2018
* Chaos Group V-Ray per Maya 2017 (versione 3.60.04)
* Chaos Group V-Ray per Maya 2018 (versione 3.60.04)
* Blender (2.68)

## <a name="applications-on-windows-server-2016-rendering-nodes"></a>Applicazioni nei nodi di rendering Windows Server 2016

* Autodesk Maya I/O 2017 Update 5 (versione 17.4.5459)
* Autodesk Maya I/O 2018 Update 3 (versione 18.3.0.7040)  
* Autodesk 3ds Max I/O 2019 Update 1 (versione 21.10.1314)
* Autodesk 3ds Max I/O 2018 Update 4 (versione 20.4.0.4254)
* Autodesk Arnold per Maya (Arnold versione 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold per Maya (Arnold versione 5.0.1.4) MtoA-2.0.2.3-2018
* Autodesk Arnold per 3ds Max (Arnold versione 5.0.2.4 )(versione 1.2.926)
* Chaos Group V-Ray per Maya (versione 3.52.03)
* Chaos Group V-Ray per 3ds Max (versione 3.60.02)
* Blender (2.79)

## <a name="next-steps"></a>Passaggi successivi

Per usare le immagini di VM per il rendering, è necessario specificare le immagini nella configurazione del pool al momento della creazione. Vedere le [funzionalità del pool di Batch per il rendering](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools).