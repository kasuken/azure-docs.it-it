---
title: Informazioni su come i criteri aumentano l'accuratezza della stima
titleSuffix: Azure Cognitive Services
description: I criteri sono progettati per migliorare l'accuratezza quando vi sono più espressioni molto simili. Un modello consente di ottenere maggiore accuratezza in relazione a una finalità senza fornire molte altre espressioni.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 09c869bf28b804d8fabe331c4a9c2d222accc1e5
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300371"
---
# <a name="patterns-improve-prediction-accuracy"></a>Migliorare l'accuratezza della stima con i criteri
I criteri sono progettati per migliorare l'accuratezza quando vi sono più espressioni molto simili.  Un modello consente di ottenere maggiore accuratezza in relazione a una finalità senza fornire molte altre espressioni. 

## <a name="patterns-solve-low-intent-confidence"></a>I criteri risolvono il problema dell'attendibilità ridotta della finalità
Si consideri un'app di risorse umane che genera report nel grafico aziendale su un dipendente. Dato un nome e una relazione dipendente, LUIS restituisce i dipendenti coinvolti. Si consideri un dipendente, Tom, con un manager di nome Alice, e un team di sottoposti che si chiamano Michael, Rebecca e Carl.

![Immagine del grafico aziendale](./media/luis-concept-patterns/org-chart.png)

|Espressioni|Finalità stimata|Punteggio finalità|
|--|--|--|
|Chi è il sottoposto di Tom?|GetOrgChart|.30|
|Come si chiama il sottoposto di Tom?|GetOrgChart|.30|

Se un'app ha tra le 10 e le 20 espressioni con diverse lunghezze di frase, diverso ordine di parole e persino diverse parole (sinonimi di "sottoposto", "gestire", "report"), LUIS può restituire un punteggio corrispondente a un'attendibilità bassa. Per consentire a LUIS di comprendere l'importanza dell'ordine delle parole, è necessario creare un criterio. 

I criteri risolvono le situazioni seguenti: 

* Quando il punteggio della finalità è basso
* Quando la finalità corretta non è il punteggio più alto ma è troppo vicina al punteggio più alto. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>I criteri non sono una garanzia della finalità
I criteri usano una combinazione di tecnologie di stima. L'impostazione di una finalità per l'espressione di un modello in un criterio non è una garanzia per la stima della finalità ma è un segnale. 

## <a name="patterns-do-not-improve-entity-detection"></a>I criteri non migliorano il rilevamento di entità
Anche se i criteri richiedono le entità, non servono a rilevarle. Un criterio è concepito solo per l'uso di finalità e ruoli con le stime.  

## <a name="patterns-use-entity-roles"></a>I criteri usano i ruoli delle entità
Se due o più entità in un criterio hanno una relazione di tipo contestuale, i criteri usano i [ruoli](luis-concept-roles.md) delle entità per estrarre informazioni contestuali sull'entità. Ciò equivale agli elementi figlio nelle gerarchiche di entità, ma è disponibile **solo** nei criteri. 

## <a name="prediction-scores-with-and-without-patterns"></a>Punteggi di stima con e senza criteri
Con un numero sufficiente di espressioni di esempio, LUIS dovrebbe poter aumentare l'attendibilità delle stime anche senza criteri. I criteri aumentano il punteggio di attendibilità senza dovere specificare un numero così elevato di espressioni.  

## <a name="pattern-matching"></a>Corrispondenza dei criteri
Per cercare una corrispondenza tra criteri, prima vengono rilevate le entità all'interno del criterio, poi vengono convalidate le parole restanti e l'ordine delle parole del criterio. Le entità nel criterio sono necessarie perché venga individuata una corrispondenza del criterio. 

## <a name="pattern-syntax"></a>Sintassi dei criteri
La sintassi dei criteri è un modello per un'espressione. Il modello deve contenere parole ed entità che si vuole far corrispondere, nonché le parole e la punteggiatura che si vuole ignorare. **Non** si tratta di un'espressione regolare. 

Le entità nei criteri sono racchiuse tra parentesi graffe, `{}`. I criteri possono includere entità ed entità con ruoli. Pattern.any è un'entità usata solo nei criteri. La sintassi viene illustrata nelle sezioni seguenti.

### <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Sintassi per aggiungere un'entità a un modello di criteri
Per aggiungere un'entità a un modello di criteri, racchiudere il nome dell'entità tra parentesi graffe, come ad esempio in `Who does {Employee} manage?`. 

|Criteri con entità|
|--|
|`Who does {Employee} manage?`|

### <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Sintassi per aggiungere un'entità e un ruolo a un modello di criteri
Un ruolo di entità viene indicato come `{entity:role}` con il nome dell'entità seguito da due punti e dal nome del ruolo. Per aggiungere un'entità con un ruolo a un modello di criteri, racchiudere il nome dell'entità e il nome del ruolo tra parentesi graffe, come ad esempio in `Book a ticket from {Location:Origin} to {Location:Destination}`. 

|Criteri con ruoli di entità|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

### <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Sintassi per aggiungere un'entità pattern.any a un modello di criteri
L'entità Pattern.any consente di aggiungere al criterio un'entità di lunghezza variabile. L'entità Pattern.any può avere qualsiasi lunghezza, purché il modello di criteri sia rispettato. 

Per aggiungere un'entità **Pattern.any** al modello di criteri, racchiuderla tra parentesi graffe, come ad esempio in `How much does {Booktitle} cost and what format is it available in?`.  

|Criteri con entità pattern.any|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Titoli di libri nel criterio|
|--|
|How much does **steal this book** cost and what format is it available in?|
|How much does **ask** cost and what format is it available in?|
|How much does **The Curious Incident of the Dog in the Night-Time** cost and what format is it available in?| 

In questi titoli di libri di esempio, le parole contestuali del titolo del libro non generano confusione per LUIS. LUIS sa dove finisce il titolo del libro perché è in un criterio ed è contrassegnato con un'entità Pattern.any.

### <a name="explicit-lists"></a>Elenchi espliciti
Se il criterio contiene un'entità Pattern.any e la sintassi del criterio offre la possibilità di un'estrazione dell'entità non corretta in base all'espressione, creare un [elenco esplicito](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) tramite l'API di creazione per consentire l'eccezione. 

Ad esempio, si supponga che un criterio contenga sia la sintassi facoltativa, `[]`, che la sintassi dell'entità, `{}`, combinate in modo da estrarre i dati in modo non corretto.

Si consideri il criterio "[find] email about {subject} [from {person}]". Nelle espressioni seguenti le entità **subject** e **person** vengono estratte in modo corretto e non corretto:

|Espressione|Entità|Estrazione corretta|
|--|--|:--:|
|email about dogs from Chris|subject=dogs<br>person=Chris|✔|
|email about the man from La Mancha|subject=the man<br>person=La Mancha|X|

Nella tabella precedente per l'espressione `email about the man from La Mancha`, l'elemento subject dovrebbe essere `the man from La Mancha` (titolo di un libro) ma dal momento che include la parola facoltativa `from`, il titolo viene stimato in modo non corretto. 

Per risolvere questa eccezione nel criterio, aggiungere `the man from la mancha` come corrispondenza dell'elenco esplicito per l'entità {subject} usando l'[API di creazione per l'elenco esplicito](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

### <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Sintassi per contrassegnare un testo facoltativo in un'espressione del modello
Contrassegnare il testo facoltativo nell'espressione usando la sintassi tra parentesi quadre dell'espressione regolare, `[]`. Nel testo facoltativo possono essere nidificate un massimo di due parentesi quadre.

|Criteri con testo facoltativo|
|--|
|`[find] email about {subject} [from {person}]`|

I segni di punteggiatura, come ad esempio `.`, `!` e `?` possono essere ignorati usando le parentesi quadre. Per ignorare questi segni, ognuno di essi deve essere in un criterio distinto. La sintassi facoltativa attualmente non supporta la possibilità di ignorare un elemento contenuto in un elenco di più elementi.

## <a name="patterns-only"></a>Uso dei soli criteri
LUIS consente che un'app non abbia espressioni di esempio nella finalità. Questo uso è consentito solo se vengono usati i criteri. I criteri richiedono almeno un'entità in ogni criterio. Per le app con solo criteri, il criterio non deve contenere entità provenienti da Machine Learning in quanto richiedono espressioni di esempio. 

## <a name="best-practices"></a>Procedure consigliate
Apprendere le [procedure consigliate](luis-concept-best-practices.md).

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Esegui l'esercitazione per implementare i criteri](luis-tutorial-pattern.md)