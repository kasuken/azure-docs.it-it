---
title: Origini dati supportate - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker estrae automaticamente coppie di domanda-risposta dal contenuto semistrutturato, ad esempio le domande frequenti, i manuali di prodotti, le linee guida, i documenti di supporto e i criteri archiviati come pagine Web, file PDF o file Microsoft Word. I contenuti possono anche essere aggiunti alla Knowledge Base da file di contenuto QnA strutturati.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/08/2018
ms.author: tulasim
ms.openlocfilehash: e6c654b00ee6be0ed87feb0fb2a5ccba38e5cbe4
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624878"
---
# <a name="data-sources-for-qna-maker-content"></a>Origini dati per i contenuti QnA Maker

QnA Maker estrae automaticamente coppie di domande e risposte da contenuto semistrutturato come domande frequenti, manuali di prodotto, linee guida, documenti di supporto e criteri archiviati come pagine Web, file PDF o file di documento di Microsoft Word. I contenuti possono anche essere aggiunti alla Knowledge Base da file di contenuto QnA strutturati. 

La tabella seguente riepiloga i tipi di contenuto e di formato di file supportati da QnA Maker.

|Tipo di origine|Content Type| Esempi|
|--|--|--|
|URL|Domande frequenti (con struttura piatta, a sezioni o con collegamenti ad altre pagine)|[Pagine semplici di domande frequenti](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), [pagine di domande frequenti con collegamenti](https://www.microsoft.com/software-download/faq), [pagine di domande frequenti con collegamenti ad altre pagine](https://support.microsoft.com/products/windows?os=windows-10)|
|PDF/DOC|Domande frequenti, manuale del prodotto, brochure, documenti, volantino con i Criteri di Azure, guida di supporto tecnico, domanda-risposta strutturato e così via.|[Structured QnA.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx), [Sample Product Manual.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf), [Sample semi-structured.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx), [Sample white paper.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf)|
|Excel|File domanda-risposta strutturato (tra cui RTF, supporto HTML)|[Sample QnA FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|File domanda-risposta strutturato|[Esempio chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

## <a name="faq-urls"></a>Indirizzo Web di domande frequenti

QnA Maker supporta 3 diversi formati di pagine Web di domande frequenti: pagine semplici di domande frequenti, pagine di domande frequenti con collegamenti, pagine di domande frequenti con collegamenti ad altre pagine.

### <a name="plain-faq-pages"></a>Pagine semplici di domande frequenti

Si tratta del tipo più comune di pagina di domande frequenti, in cui le risposte seguono immediatamente le domande nella stessa pagina. 

Di seguito è riportato un esempio di pagina semplice di domande frequenti:

![Pagina semplice di domande frequenti](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>Pagina di domande frequenti con collegamenti 

In questo tipo di pagina di domande frequenti, le domande sono aggregate e collegate a risposte che si trovano in diverse sezioni della stessa pagina o in pagine differenti.

Di seguito è riportato un esempio di pagina di domande frequenti con collegamenti a sezioni che si trovano nella stessa pagina:

 ![Pagina di domande frequenti con collegamenti a sezioni](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>Pagina di domande frequenti con collegamenti ad altre pagine

Questo tipo di pagina di domande frequenti include una home page con argomenti in cui ogni argomento è un collegamento alle relative pagine QnA in una pagina diversa. Qui, QnA Maker esegue una ricerca per indicizzazione di tutte le pagine collegate per estrarre le domande e le risposte corrispondenti.

Di seguito è riportato un esempio di pagina delle domande frequenti in cui la home page di argomenti include collegamenti a sezioni di domande frequenti in pagine diverse. 

 ![Pagina delle domande frequenti con collegamento diretto](../media/qnamaker-concepts-datasources/topics-faq.png) 


## <a name="pdf-doc-files"></a>File PDF/DOC

QnA Maker può elaborare il contenuto semistrutturato di un file PDF o DOC e convertirlo in file domanda-risposta. Un buon file che può inoltre essere estratto è uno in cui il contenuto è organizzato in un formato strutturato e viene rappresentato da sezioni ben definite. Le sezioni possono essere suddivise ulteriormente in sottosezioni o argomenti correlati. Il procedimento di estrazione funziona meglio su documenti che presentano una struttura chiara con intestazioni gerarchiche.

QnA Maker identifica sezioni, sottosezioni e relazioni nel file basandosi su indizi visivi, ad esempio le dimensione del carattere,lo stile del carattere, i numeri, i colori e così via. I file PDF o DOC semistrutturati potrebbero essere manuali, domande frequenti, linee guida, criteri, brochure, volantini e molti altri tipi. Di seguito alcuni esempi di questi tipi di file.

### <a name="product-manuals"></a>Manuali di prodotti

Un manuale è in genere materiale di riferimento fornito insieme a un prodotto. Consente all'utente di configurare, usare e gestire un prodotto e di risolverne i problemi. Quando QnA Maker elabora un manuale, estrae i titoli e sottotitoli come domande e il contenuto successivo come risposte. Vedere un esempio [qui](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

Di seguito è riportato un esempio di un manuale con una pagina di indice e con contenuto gerarchico

 ![Esempio di manuale del prodotto](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> L'estrazione funziona al meglio con i manuali che hanno un sommario e/o una pagina di indice e una struttura chiara con titoli gerarchici.

### <a name="brochures-guidelines-papers-and-other-files"></a>Brochure, linee guida, documenti e altri file

Possono essere elaborati anche molti altri tipi di documento in modo che possano generare coppie di controllo qualità, purché essi dispongano di una struttura e di un layout chiari. Sono inclusi: brochure, linee guida, report, white paper, documenti scientifici, criteri, documentazioni e così via. Vedere un esempio [qui](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Di seguito è riportato un esempio di documento semistrutturato, senza un indice:

 ![Documento semistrutturato di Archiviazione Blob di Azure](../media/qnamaker-concepts-datasources/semi-structured-doc.png) 

### <a name="structured-qna-document"></a>Documento domanda-risposta strutturato

Il formato per documenti domanda-risposta strutturati in file con estensione DOC consiste nell'alternanza di domande e risposte per ogni linea, una domanda per ogni linea seguita dalla relativa risposta nella linea seguente, come illustrato di seguito: 

```text
Question1

Answer1

Question2

Answer2
```

Di seguito è riportato un esempio di un documento di Word di domanda-risposta strutturato:

 ![Documento domanda-risposta strutturato](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>File *TXT*, *TSV* e *XLS* strutturati

Anche i documenti domanda-risposta sotto forma di file *.txt*, *.tsv* oppure *.xls* strutturati possono essere caricati da QnA Maker per creare o estendere una knowledge base.  Questi file possono essere testo normale, o possono disporre di contenuto in formato RTF o HTML. 

| Domanda  | Risposta  | Metadata                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Qualsiasi colonna aggiuntiva nel file di origine viene ignorata.

Di seguito è riportato un esempio di file *.xls* domanda-risposta strutturato, con contenuto HTML:

 ![File Excel domanda-risposta strutturato](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>Formato di dati strutturati tramite l'importazione

L'importazione di una Knowledge Base sostituisce il contenuto della Knowledge Base esistente. L'importazione richiede un file TSV strutturato che contiene informazioni sull'origine dei dati. Queste informazioni consentono a QnA Maker di raggruppare le coppie domanda/risposta e di attribuirle a una specifica origine dati.

| Domanda  | Risposta  | Sorgente| Metadata                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Editoriale|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Aggiunta di contenuti a livello editoriale alla knowledge base

Se non è presente contenuto pre-esistente per popolare la Knowledge Base, è possibile aggiungere contenuti domanda-risposta a livello editoriale nella Knowledge Base di QnA Maker. Informazioni su come aggiornare la Knowledge Base sono disponibili [qui](../How-To/edit-knowledge-base.md).

## <a name="formatting-considerations"></a>Considerazioni di formattazione

Dopo aver importato un file o un URL, questo viene convertito in Markdown e archiviato in questo formato. Se il processo di conversione non converte correttamente i collegamenti nei file e gli URL, è consigliabile modificare le domande e risposte nella pagina **Modifica**. 

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Configurare un servizio QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Vedere anche  

[Panoramica di QnA Maker](../Overview/overview.md)
