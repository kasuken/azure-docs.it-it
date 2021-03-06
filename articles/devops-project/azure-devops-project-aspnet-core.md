---
title: 'Guida introduttiva: Creare una pipeline CI/CD per .NET con Azure DevOps Projects'
description: Azure DevOps Projects permette di iniziare a usare Azure velocemente. Con pochi rapidi passaggi, è possibile avviare un'app .NET in un servizio di Azure a scelta.
ms.prod: devops
ms.technology: devops-cicd
services: azure-devops-project
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 5fabe9ba03c9516f5df41645fc6ab1b7a0cb2050
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262177"
---
# <a name="create-a-cicd-pipeline-for-net-with-azure-devops-projects"></a>Creare una pipeline CI/CD per .NET con Azure DevOps Projects

Configurare integrazione continua (CI, Continuous Integration) e recapito continuo (CD, Continuous Delivery) per un'applicazione .NET Core o ASP.NET con DevOps Projects. DevOps Projects semplifica la configurazione iniziale di una pipeline di compilazione e di versione in Azure Pipelines.

Se non si ha ancora una sottoscrizione di Azure, è possibile ottenerne una gratuita tramite [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Accedere al portale di Azure

DevOps Projects crea una pipeline CI/CD in Azure DevOps. È possibile creare una nuova organizzazione di Azure DevOps o usare un'organizzazione esistente. DevOps Projects crea anche risorse di Azure nella sottoscrizione di Azure di propria scelta.

1. Accedere al [portale di Microsoft Azure](https://portal.azure.com).

1. Nel riquadro a sinistra, selezionare l'icona **Crea una risorsa** sulla barra di spostamento sinistra e quindi cercare **DevOps Projects**.  

3.  Selezionare **Create**.

    ![Avvio del recapito continuo](_img/azure-devops-project-aspnet-core/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Selezionare un'applicazione di esempio e un servizio di Azure

1. Selezionare l'applicazione di esempio .NET. Gli esempi .NET consentono di scegliere tra il framework ASP.NET open source o il framework .NET Core multipiattaforma.

    ![.NET Framework](_img/azure-devops-project-aspnet-core/chooselanguagedotnet.png)

1. Selezionare il framework applicazione .NET Core.  
    Questo esempio è un'applicazione MVC ASP.NET Core.
    
2. Selezionare **Avanti**.  
    La destinazione della distribuzione predefinita è App Web in Windows.  Facoltativamente, è possibile scegliere App Web in Linux o App Web per contenitori.  Il framework applicazione, scelto in precedenza, determina il tipo di destinazione della distribuzione del servizio di Azure disponibile.  
    
3. Lasciare il servizio predefinito e quindi selezionare **Avanti**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Configurare Azure DevOps e una sottoscrizione di Azure 

1. Creare una nuova organizzazione gratuita di Azure DevOps Services o scegliere un'organizzazione esistente.

    a. Scegliere un nome per il progetto. 

    b. Selezionare la sottoscrizione di Azure e la posizione, scegliere un nome per l'applicazione, quindi selezionare **Fine**.  
    Dopo pochi minuti, il dashboard di DevOps Projects viene visualizzato nel portale di Azure. Viene configurata un'applicazione di esempio in un repository nell'organizzazione Azure DevOps, viene eseguita una compilazione e l'applicazione viene distribuita in Azure. Questo dashboard fornisce visibilità su repository di codice, pipeline CI/CD e applicazione in Azure.
    

2. A destra del dashboard selezionare **Sfoglia** per visualizzare l'applicazione in esecuzione.

    ![Visualizzazione dashboard](_img/azure-devops-project-aspnet-core/dashboardnopreview.png) 

## <a name="commit-code-changes-and-execute-cicd"></a>Eseguire il commit delle modifiche al codice e la pipeline di CI/CD

 DevOps Projects ha creato un repository Git in Azure Repos o GitHub. Per visualizzare il repository e apportare modifiche al codice nell'applicazione, seguire questa procedura:

1. A sinistra del dashboard di DevOps Projects selezionare il collegamento per il ramo **master**.  
Questo collegamento apre una visualizzazione del repository Git appena creato.

1. Per visualizzare l'URL clone del repository, selezionare **Clona** in alto a destra nel browser.  
È possibile clonare il repository Git nell'IDE preferito.  Nei passaggi successivi, è possibile usare il Web browser per apportare modifiche al codice ed eseguirne il commit direttamente nel ramo master.

1. A sinistra del browser passare al file **Views/Home/index.cshtml**.

1. Selezionare **Modifica** e quindi apportare una modifica al titolo h2. Ad esempio, digitare **Iniziare subito con Azure DevOps Projects** o apportare altre modifiche.

    ![Editor di codice](_img/azure-devops-project-aspnet-core/codechange.png)

1. Selezionare **Esegui commit** e quindi salvare le modifiche.

1. Nel browser passare al dashboard del progetto DevOps di Azure.  Dovrebbe venire visualizzata una compilazione in corso. Le modifiche apportate vengono compilate e distribuite automaticamente tramite una pipeline CI/CD.

## <a name="examine-the-cicd-pipeline"></a>Esaminare la pipeline CI/CD

Nel passaggio precedente, Azure DevOps Projects ha configurato automaticamente una pipeline CI/CD completa. Esplorare e personalizzare la pipeline in base alle esigenze. Eseguire questa procedura per acquisire familiarità con le pipeline di compilazione e di versione di Azure DevOps.

1. Selezionare **Pipeline di compilazione** nella parte superiore di DevOps Projects.  
Questo collegamento apre una scheda del browser e la pipeline di compilazione di Azure DevOps per il nuovo progetto.

1. Selezionare i puntini di sospensione (...).  Questa azione apre un menu da cui è possibile avviare diverse attività, ad esempio accodare una nuova compilazione, sospendere una compilazione e modificare la pipeline di compilazione.

1. Selezionare **Modifica**.

    ![Pipeline di compilazione](_img/azure-devops-project-aspnet-core/builddef.png)

1. In questo riquadro è possibile esaminare le diverse attività per la pipeline di compilazione.  
 La compilazione esegue diverse attività, ad esempio il recupero delle origini dal repository Git, il ripristino delle dipendenze e la pubblicazione degli output usati per le distribuzioni.

1. Nella parte superiore della pipeline di compilazione selezionare il nome della pipeline di compilazione.

1. Modificare il nome della pipeline di compilazione scegliendo un testo più descrittivo, selezionare **Salva e accoda**, quindi selezionare **Salva**.

1. Sotto il nome della pipeline di compilazione selezionare **Cronologia**.   
Nel riquadro **Cronologia** verrà visualizzato un log di controllo delle modifiche recenti per la compilazione.  Azure Pipelines tiene traccia di tutte le modifiche apportate alla pipeline di compilazione e consente di confrontare le versioni.

1. Selezionare **Trigger**.  
DevOps Projects ha creato automaticamente un trigger di integrazione continua e ogni commit nel repository avvia una nuova compilazione.  Facoltativamente, è possibile scegliere di includere o escludere rami dal processo di integrazione continua.

1. Selezionare **Conservazione**.  
A seconda dello scenario specifico, è possibile indicare i criteri per conservare o rimuovere un determinato numero di compilazioni.

1. Selezionare **Compilazione e versione**, quindi **Versioni**.  
DevOps Projects crea una pipeline di versione per gestire le distribuzioni in Azure.

1.  A sinistra, selezionare i puntini di sospensione (...) accanto alla pipeline di versione e quindi scegliere **Modifica**.  
La pipeline di versione contiene una pipeline che definisce il processo per la versione.  

1. In **Artefatti** selezionare **Elimina**.  La pipeline di compilazione esaminata nei passaggi precedenti produce l'output usato per l'artefatto. 

1. Accanto all'icona **Elimina** selezionare **Trigger di distribuzione continua**.  
Questa pipeline di versione ha un trigger di distribuzione continua abilitato, che esegue una distribuzione ogni volta che è disponibile un nuovo artefatto di compilazione. Facoltativamente, è possibile disabilitare il trigger, in modo che le distribuzioni richiedano l'esecuzione manuale.  

1. A sinistra selezionare **Attività**.   
Le attività sono le operazioni eseguite dal processo di distribuzione. In questo esempio è stata creata un'attività per la distribuzione in Servizio app di Azure.

1. A destra, selezionare **Visualizza versioni**. Questa visualizzazione mostra una cronologia delle versioni.

1. Selezionare i puntini di sospensione (...) accanto a una delle versioni e quindi selezionare **Apri**.  
Ci sono diversi menu da esplorare, ad esempio un riepilogo delle versioni, gli elementi di lavoro associati e i test.


1. Selezionare **Commit**.   
Questa visualizzazione mostra i commit di codice associati alla distribuzione specifica. 

1. Selezionare **Log**.  
I log contengono informazioni utili sul processo di distribuzione. Possono essere visualizzati durante e dopo le distribuzioni.


## <a name="clean-up-resources"></a>Pulire le risorse

Quando non servono più, è possibile eliminare il Servizio app di Azure e altre risorse correlate creati in precedenza. Usare la funzionalità **Elimina** nel dashboard di DevOps Projects.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla modifica delle pipeline di compilazione e di versione in base alle esigenze del team, vedere questa esercitazione:

> [!div class="nextstepaction"]
> [Personalizzare il processo di distribuzione continua](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Video

> [!VIDEO https://www.youtube.com/embed/itwqMf9aR0w]
