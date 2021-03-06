---
title: Opzioni di Microsoft Azure IoT | Microsoft Docs
description: Scegliere la modalità di implementazione della soluzione Azure IoT con gli acceleratori di soluzioni Azure IoT Central e IoT o con hub IoT.
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 0314bf4d26fb0f777fd88debbf09814479ab225a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34630528"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Confrontare le opzioni di Azure IoT Central e Azure IoT Suite

Per implementare una soluzione IoT, Microsoft Azure IoT Central e Azure IoT offrono varie opzioni, ognuna adeguata ai diversi requisiti dei clienti:

* [Azure IoT Central](overview-iot-central.md) è una soluzione SaaS (Software-as-a-Service) che usa un approccio basato su modelli per consentire la creazione di soluzioni IoT di livello aziendale senza competenze nello sviluppo di soluzioni cloud.

* Gli [acceleratori di soluzioni Azure IoT](https://docs.microsoft.com/azure/iot-accelerators/) sono una raccolta di livello aziendale di [acceleratori di soluzioni](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md) basata sulla piattaforma distribuita come servizio (PaaS) di Azure che consentono di velocizzare lo sviluppo di soluzioni IoT personalizzate.

## <a name="azure-iot-hub"></a>Hub IoT Azure

Hub IoT di Azure è la piattaforma PaaS principale di Azure usata sia da Azure IoT Central che dagli acceleratori di soluzioni Azure IoT. L'hub IoT supporta comunicazioni bidirezionali affidabili e sicure tra milioni di dispositivi IoT e una soluzione cloud. L'hub IoT consente di risolvere problematiche associate all'implementazione IoT come le seguenti:

* Connettività e gestione di volumi elevati di dispositivi.
* Inserimento di volumi elevati di dati di telemetria.
* Comando e controllo dei dispositivi.
* Applicazione della sicurezza dei dispositivi.

## <a name="compare-azure-iot-central-and-azure-iot-solution-accelerators"></a>Confrontare gli acceleratori di soluzioni Azure IoT Central e Azure IoT

La scelta del prodotto Azure IoT è una parte essenziale della pianificazione della soluzione IoT. L'hub IoT è un singolo servizio di Azure che non offre, da solo, una soluzione IoT end-to-end, ma può essere usato come punto di partenza per qualsiasi soluzione IoT. Non è necessario usare gli acceleratori di soluzioni Azure IoT o Azure IoT Central per usare l'hub IoT. Sia gli acceleratori di soluzioni IoT che Azure IoT Central usano l'hub IoT insieme ad altri servizi di Azure. La tabella seguente offre un riepilogo delle differenze principali tra gli acceleratori di soluzioni IoT e Azure IoT Central per consentire la scelta dell'opzione corretta per i propri requisiti:

|     | Azure IoT Central | Solution Accelerator di Azure IoT |
| --- | ----------- | --------- |
| Utilizzo primario                      | Accelerare il time-to-market per soluzioni IoT semplici che non richiedono una personalizzazione approfondita dei servizi.                                                    | Velocizzare lo sviluppo di una soluzione IoT personalizzata che necessita della massima flessibilità.                                                                                                                             |
| Accesso ai servizi PaaS sottostanti | SaaS. Soluzione completamente gestita. I servizi sottostanti non sono esposti.                                                                                            | L'utente ha accesso ai servizi di Azure sottostanti per gestirli o, se necessario, sostituirli.                                                                                                                    |
| Flessibilità                        | Media. È possibile usare l'esperienza utente basata su browser predefinita per personalizzare il modello di soluzione e aspetti dell'interfaccia utente. L'infrastruttura non è personalizzabile perché i diversi componenti non sono esposti. | Elevata. Il codice per i microservizi è open source e può essere modificato come si preferisce. È anche possibile personalizzare l'infrastruttura di distribuzione.                                               |
| Livello di competenze                        | Bassa. Sono necessarie competenze di modellazione per personalizzare la soluzione. Non sono richieste competenze nella creazione di codice.                                                                          | Medio-alto. Sono necessarie competenze in Java o .NET per personalizzare il back-end della soluzione e competenze in JavaScript per personalizzare la visualizzazione.                                                                       |
| Esperienza iniziale             | I modelli di applicazione e i modelli di dispositivo forniscono modelli predefiniti distribuibili in pochi minuti.                                                                                                  | Soluzioni preconfigurate implementano scenari IoT comuni, distribuibili in pochi minuti.                                                                                                                            |
| Prezzi                            | Struttura di prezzi semplice e prevedibile.                                                                                                                           | È possibile ottimizzare i servizi per controllare i costi.                                                                                                                                                            |

La scelta del prodotto da usare per creare la propria soluzione IoT è determinata in definitiva da:

* Requisiti aziendali.
* Tipo di soluzione che si vuole creare.
* Set di competenze dell'organizzazione per la creazione e la gestione della soluzione a lungo termine.

## <a name="next-steps"></a>Passaggi successivi

In base al prodotto e all'approccio scelti, i passaggi successivi consigliati sono i seguenti.

* **Azure IoT Central**: [Azure IoT Central](overview-iot-central.md).
* **Acceleratori di soluzioni IoT**: [Quali sono gli acceleratori di soluzioni di Azure IoT?](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md).
* **Hub IoT**: [Panoramica del servizio hub IoT di Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub).