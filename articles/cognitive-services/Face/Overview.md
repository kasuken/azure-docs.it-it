---
title: Informazioni sull'API Viso
titleSuffix: Azure Cognitive Services
description: Informazioni su come usare il servizio Viso per rilevare e analizzare i visi nelle immagini.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 10/29/2018
ms.author: sbowles
ms.openlocfilehash: a15b6678b15bf5d1a3078494e12da3a08c57bed3
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633462"
---
# <a name="what-is-the-azure-face-api"></a>Informazioni sull'API Viso di Azure

L'API Viso di Azure è un servizio cognitivo che include algoritmi per il rilevamento, il riconoscimento e l'analisi dei visi umani nelle immagini. La possibilità di elaborare informazioni sui visi umani è importante in molti scenari software diversi, tra cui sicurezza, interfaccia utente naturale, analisi e gestione del contenuto di immagini, app per dispositivi mobili e robotica.

L'API Viso offre varie funzioni, descritte nelle sezioni seguenti. Continuare a leggere per altre informazioni su ognuna di esse e determinare se è adatta alle proprie esigenze.

## <a name="face-detection"></a>Rilevamento del viso

L'API Viso può rilevare i visi umani in un'immagine e restituire le coordinate del rettangolo delle posizioni corrispondenti. Facoltativamente, il rilevamento viso può estrarre una serie di attributi relativi al viso, ad esempio posa, sesso, età, posizione della testa, barba/baffi e occhiali.

![Immagine di una donna e un uomo, con rettangoli disegnati intorno ai visi e informazioni su età e sesso](./Images/Face.detection.jpg)

La funzionalità di rilevamento viso è disponibile anche tramite l'[API Visione artificiale](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home), ma se si vogliono eseguono ulteriori operazioni con i dati sui volti è preferibile usare l'API Viso (questo servizio). Per altre informazioni sul rilevamento del viso, vedere l'[API di rilevamento](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-verification"></a>Verifica del viso

L'API di verifica esegue un'autenticazione su due visi rilevati o un'autenticazione da un viso rilevato a un oggetto persona. In pratica, valuta se due visi appartengano alla stessa persona. Questo è potenzialmente utile negli scenari di sicurezza. Per altre informazioni, vedere l'[API di verifica](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="find-similar-faces"></a>Individuazione di visi simili

L'API di individuazione di visi simili prende un viso di destinazione e un set di visi candidati e individua un set più piccolo di visi più simili a quello di destinazione. Sono supportate due modalità di utilizzo, ovvero **matchPerson** e **matchFace**. La modalità **matchPerson** restituisce visi simili dopo aver filtrato in base all'appartenenza alla stessa persona (usando l'[API di verifica](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)). La modalità **matchFace** ignora il filtro di appartenenza alla stessa persona e restituisce un elenco di visi candidati simili, che possono o meno appartenere alla stessa persona.

Nell'esempio seguente, questo è il viso di destinazione:

![Una donna sorridente](./Images/FaceFindSimilar.QueryFace.jpg)

E questi sono i visi candidati:

![Cinque immagini di persone sorridenti. Le immagini a) e b) sono della stessa persona](./Images/FaceFindSimilar.Candidates.jpg)

Per trovare quattro visi simili, la modalità **matchPerson** restituirebbe (a) e (b), che rappresentano la stessa persona del viso di destinazione. La modalità **matchFace** restituisce (a), (b), (c) e (d)&mdash;esattamente quattro candidati anche se alcuni non appartengono alla stessa persona della destinazione o la somiglianza è minore. Per altre informazioni, vedere l'[API di individuazione di visi simili](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

## <a name="face-grouping"></a>Raggruppamento dei visi

L'API di raggruppamento divide un set di visi sconosciuti in più gruppi basati sulla somiglianza. Ogni gruppo è un subset proprio indipendente del set di visi originale. Tutti i visi nello stesso gruppo appartengono probabilmente alla stessa persona, ma possono essere presenti gruppi diversi per una singola persona, che si differenziano per un altro fattore, ad esempio l'espressione. Per altre informazioni, vedere l'[API di raggruppamento](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="person-identification"></a>Identificazione delle persone

L'API di identificazione può essere usata per identificare un viso rilevato confrontandolo con un database di persone. Questo può essere utile per l'aggiunta automatica di tag alle immagini nei software di gestione delle fotografie. È possibile creare questo database in anticipo e modificarlo nel tempo.

L'immagine seguente è un esempio di un database denominato "myfriends". Ogni gruppo può contenere fino a 1.000.000 di oggetti persona diversi e per ogni oggetto persona è possibile registrare fino a 248 visi.

![Griglia con 3 colonne per persone diverse, ognuna con 3 righe di immagini dei visi](./Images/person.group.clare.jpg)

Dopo la creazione e il training di un database, è possibile eseguire l'identificazione in base al gruppo con un nuovo viso rilevato. Se il viso viene identificato come una persona nel gruppo, viene restituito l'oggetto persona.

Per altre informazioni sull'identificazione di persone, vedere l'[API di identificazione](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="use-containers"></a>Usare i contenitori

[Usare il contenitore Viso](face-how-to-install-containers.md) per rilevare, riconoscere e identificare visi installando un contenitore Docker standardizzato più vicino ai dati.

## <a name="sample-apps"></a>App di esempio

Le applicazioni di esempio seguenti illustrano alcuni dei modi in cui si può usare l'API Viso.

- [API Viso Microsoft: libreria client ed esempio Windows](https://github.com/Microsoft/Cognitive-Face-Windows). App WPF che illustra diversi scenari di rilevamento, analisi e identificazione del viso.
- [App UWP FamilyNotes](https://github.com/Microsoft/Windows-appsample-familynotes). App della piattaforma UWP (Universal Windows Platform) che usa l'identificazione del viso insieme a riconoscimento vocale, Cortana, input penna e fotocamera in uno scenario di condivisione note in una famiglia.

## <a name="next-steps"></a>Passaggi successivi

Seguire una guida introduttiva per implementare un semplice scenario di rilevamento del viso nel codice.
- [Guida introduttiva: Rilevare i visi in un'immagine usando .NET SDK con C#](quickstarts/csharp.md) (disponibile per altri linguaggi)
