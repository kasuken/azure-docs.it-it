---
title: Integrazione e distribuzione continue di Azure IoT Edge | Microsoft Docs
description: Panoramica sull'integrazione e sulla distribuzione continue di Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 06dec64a55aaece4cd67ebf0485e34aa206a8936
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633734"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Integrazione e distribuzione continue in Azure IoT Edge

Si può facilmente adottare DevOps con le applicazioni Azure IoT Edge con [Azure IoT Edge per Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) o [Plugin Azure IoT Edge per Jenkins](https://plugins.jenkins.io/azure-iot-edge). Questo articolo descrive come usare le funzionalità di integrazione continua e di distribuzione continua di Azure Pipelines e Microsoft Team Foundation Server (TFS) per compilare, testare e distribuire applicazioni in Azure IoT Edge in modo rapido ed efficiente. 

In questo articolo verrà spiegato come:
* Creare e archiviare un esempio di soluzione IoT Edge.
* Installare l'estensione Azure IoT Edge per Azure DevOps.
* Configurare l'integrazione continua (CI) per compilare la soluzione.
* Configurare la distribuzione continua (CD) per distribuire la soluzione e vedere i responsi.

Per seguire la procedura descritta in questo articolo sono richiesti 30 minuti.

![CI e CD](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Creare una soluzione Azure IoT Edge di esempio usando Visual Studio Code

In questa sezione si crea una soluzione IoT Edge di esempio contenente unit test che è possibile eseguire come parte del processo di compilazione. Prima di applicare le indicazioni di questa sezione, completare la procedura descritta in [Sviluppare una soluzione IoT Edge con più moduli in Visual Studio Code](tutorial-multiple-modules-in-vscode.md).

1. Nel riquadro comandi VS Code digitare ed eseguire il comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: Nuova soluzione IoT Edge). Selezionare quindi la cartella dell'area di lavoro, specificare il nome della soluzione (il nome predefinito è **EdgeSolution**) e creare un modulo C# (**FilterModule**) come primo modulo utente di questa soluzione. È anche necessario specificare il repository di immagini Docker per il primo modulo. Il repository di immagini Docker si basa su un registro Docker locale (`localhost:5000/filtermodule`). Per un'ulteriore integrazione continua, è necessario modificare l'impostazione di questo repository in modo che si basi sul Registro contenitori di Azure (`<your container registry address>/filtermodule`) o su Hub Docker.

    ![Impostare il Registro contenitori di Azure](./media/how-to-ci-cd/acr.png)

2. La finestra VS Code caricherà l'area di lavoro della soluzione IoT Edge. È facoltativamente possibile digitare ed eseguire **Azure IoT Edge: aggiungi modulo IoT Edge** per aggiungere altri moduli. Nella cartella radice sono presenti una cartella `modules`, una cartella `.vscode` e un file modello del manifesto della distribuzione. Tutti i codici dei moduli utente saranno sottocartelle della cartella `modules`. `deployment.template.json` è il modello del manifesto della distribuzione. Alcuni dei parametri di questo file verranno analizzati da `module.json`, presente nella cartella di ogni modulo.

3. L'esempio di soluzione IoT Edge è ora pronto. Il modulo predefinito in C# si comporta come modulo di messaggi pipe. Nel file `deployment.template.json` si vede che questa soluzione contiene due moduli. Il messaggio viene generato dal modulo `tempSensor` e inoltrato direttamente tramite pipe con `FilterModule`, quindi viene inviato all'hub IoT.

4. Salvare i progetti, quindi archiviarli nel repository di Azure Repos o TFS.
    
> [!NOTE]
> Per altre informazioni sull'uso di Azure Repos, vedere [Condividere il codice con Visual Studio e Azure Repos](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Configurare Azure Pipelines per l'integrazione continua
In questa sezione si crea una pipeline di compilazione che viene configurata per l'esecuzione automatica quando si archiviano modifiche all'esempio di soluzione IoT Edge e verranno mostrati log di compilazione in Azure Pipelines.

1. Accedere all'organizzazione Azure DevOps (**https://dev.azure.com/{your organization}/**) e aprire il progetto in cui è stata archiviata l'app di esempio.

    ![Archiviare il codice](./media/how-to-ci-cd/init-project.png)

1. Visita [Azure IoT Edge per Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) in Marketplace di Azure DevOps. Fare clic su **Get it free** e seguire la procedura guidata per installare l'estensione nell'organizzazione Azure DevOps o scaricarla in TFS.

    ![Installare l'estensione](./media/how-to-ci-cd/install-extension.png)

1. In Azure Pipelines, aprire l'hub **Compilazione e versione**, quindi nella scheda **Compilazioni** scegliere **+ Nuova pipeline**. In alternativa, se si dispone già di pipeline di compilazione, scegliere il pulsante **+ Nuova**.

    ![Nuova pipeline](./media/how-to-ci-cd/add-new-build.png)

1. Se richiesto, selezionare il tipo di origine **Git**. Selezionare quindi progetto, repository e ramo in cui si trova il codice. Scegliere **Continua**.

    ![Selezionare git](./media/how-to-ci-cd/select-vsts-git.png)

    Nella finestra **Select a template** (Selezionare un modello) scegliere **start with an Empty process** (iniziare con un processo vuoto).

    ![Selezionare un modello:](./media/how-to-ci-cd/start-with-empty.png)

1. Nell'editor di pipeline, scegliere il pool di agenti. 
    
    * Se si desidera compilare i propri moduli nella piattaforma amd64 per i contenitori Linux, scegliere **Hosted Ubuntu 1604**
    * Se si desidera compilare i propri moduli nella piattaforma amd64 per i contenitori Windows, scegliere **Hosted VS2017** 
    * Se si desidera compilare i moduli nella piattaforma arm32v7 per contenitori Linux, è necessario configurare il proprio agente di compilazione facendo clic sul pulsante **Gestisci**.
    
    ![Configurare l'agente di compilazione](./media/how-to-ci-cd/configure-env.png)

1. Nel processo dell'agente, fare clic su "+" per aggiungere due attività nella pipeline di compilazione. Il primo è da **Azure IoT Edge**. E il secondo è da **Pubblica artefatti di compilazione**
    
    ![Aggiungere attività](./media/how-to-ci-cd/add-tasks.png)

1. Nella prima attività di **Azure IoT Edge**, aggiornare il **Display name** (Nome visualizzato) con **Module Build and Push** (Compilazione e push del modulo), quindi nell'elenco a discesa **Action** (Azione) selezionare **Build and Push** (Compila ed esegui il push). Nella casella di testo **Module.json File** (File Module.json), aggiungere il percorso seguente. Scegliere quindi il **Container Registry Type** (Tipo di registro contenitori), assicurarsi di configurare e selezionare lo stesso registro nel codice (module.json). Questa attività compila ed esegue il push di tutti i moduli della soluzione e pubblica il risultato nel registro contenitori specificato. Se il push dei moduli verrà eseguito in registri diversi, è possibile avere più attività **Module Build and Push** (Compilazione e push del modulo). Nel caso in cui la soluzione IoT Edge non sia presente nella radice del repository del codice, è possibile specificare la **radice della soluzione di percorso di Edge** nella definizione di compilazione.
    
    ![Compilazione ed esecuzione del push](./media/how-to-ci-cd/build-and-push.png)

1. Nelle attività **pubblica artefatti di compilazione**, si specificherà il file di distribuzione generato dall'attività di compilazione. Impostare il **percorso per la pubblicazione** per "config/deployment.json". Se si configura la **radice della soluzione di percorso di Edge** nell'ultima attività, sarà necessario aggiungere qui il percorso radice. Ad esempio, se il percorso della radice della soluzione Edge è "./edgesolution", il **percorso per la pubblicazione** deve essere "./edgesolution/config/deployment.json". Il file `deployment.json` viene generato durante la fase di compilazione, pertanto è possibile ignorare le righe di errore rosse nella casella di testo. 

    ![Pubblicare artefatti](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Visualizzare la scheda **Trigger** e attivare il trigger **Integrazione continua**. Verificare che il ramo contenente il codice sia incluso.

    ![Configurazione del trigger](./media/how-to-ci-cd/configure-trigger.png)

    Salva la nuova pipeline di compilazione. Fare clic sul pulsante **Salva** .


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Configurare Azure Pipelines per la distribuzione continua
In questa sezione si creerà una pipeline di versione configurata per essere eseguita automaticamente quando la pipeline di compilazione rilascerà gli artefatti e mostrerà i registri di distribuzione in Azure Pipelines.

1. Nella scheda **Versioni**, scegliere **+ Nuova pipeline**. In alternativa, se si dispone già di pipeline di versione, scegliere il pulsante **+ Nuova**.  

    ![Aggiungere pipeline di versione](./media/how-to-ci-cd/add-release-pipeline.png)

    Nella finestra **Selezionare un modello** scegliere **Iniziare con un processo vuoto**.

    ![Iniziare con processo vuoto](./media/how-to-ci-cd/start-with-empty-job.png)

2. Quindi la pipeline di versione verrà inizializzata con un'unica fase: **Fase 1**. Rinominare la **Fase 1** in **Controllo qualità** e considerarlo come un ambiente di test. In una pipeline di distribuzione continua tipica, si verificano in genere più fasi, è possibile creare di più sulla base di procedure consigliate di DevOps.

    ![Creare fase](./media/how-to-ci-cd/QA-env.png)

3. Collegare la versione agli artefatti di compilazione. Fare clic su **Aggiungi** nell'area degli elementi.

    ![Aggiungere artefatti](./media/how-to-ci-cd/add-artifacts.png)  
    
    Nella pagina **Aggiungi un artefatto**, scegliere il tipo di origine **Compilazione**. Selezionare il progetto e la pipeline di compilazione che è stata creata. Fare quindi clic su **Aggiungi**.

    ![Aggiungere un elemento](./media/how-to-ci-cd/add-an-artifact.png)

    Aprire il trigger di distribuzione continua in modo che venga creata una nuova versione ogni volta che è disponibile una nuova compilazione.

    ![Configurazione del trigger](./media/how-to-ci-cd/add-a-trigger.png)

4. Passare alla **fase di controllo qualità** e configurare le attività in questa fase.

    ![Configurare le attività di controllo di qualità](./media/how-to-ci-cd/view-stage-tasks.png)

   L’attività di distribuzione è indipendente dalla piattaforma, il che significa che è possibile scegliere **Hosted VS2017** o **Hosted Ubuntu 1604** nel **Pool di agenti** (o qualsiasi altro agente gestito dall'utente stesso). Fare clic su "+" e aggiungere un'attività.

    ![Aggiungere attività per il controllo qualità](./media/how-to-ci-cd/add-task-qa.png)

5. Nell'attività di Azure IoT Edge, passare all’elenco a discesa **Azione**, selezionare **Distribuisci al dispositivo IoT Edge**. Selezionare la **sottoscrizione di Azure** e immettere il **nome dell'hub IoT**. È possibile specificare un **ID distribuzione** IoT Edge e la **priorità** della distribuzione. È inoltre possibile scegliere di distribuire a uno o più dispositivi. Se si distribuisce a **più dispositivi**, è necessario specificare la **condizione di destinazione** del dispositivo. La condizione di destinazione è un filtro per la corrispondenza di un set di dispositivi Edge nell'hub IoT. Se si vogliono usare i Tag del dispositivo come condizione, è necessario aggiornare i tag di dispositivo corrispondenti con il dispositivo gemello hub IoT. Supponiamo di avere diversi dispositivi IoT Edge con tag 'qa', in questo caso la configurazione del task dovrebbe essere come nello screenshot seguente. 

    ![Distribuzione nel controllo di qualità](./media/how-to-ci-cd/deploy-to-qa.png)

    Salvare la nuova pipeline di versione. Fare clic sul pulsante **Salva** . Quindi fare clic su **Pipeline** per tornare alla pipeline.

6. La seconda fase è per l'ambiente di produzione. Per aggiungere una nuova fase "PROD", è possibile semplicemente clonare la fase "QA" e rinominare la fase clonata in **PROD**,

    ![Clonazione fase](./media/how-to-ci-cd/clone-stage.png)

7. Configurare le attività nell'ambiente di produzione. Supponiamo di avere diversi dispositivi IoT Edge con tag 'prod', nelle configurazioni dei task, aggiornare la Target Condition a "prod" e impostare l'ID di distribuzione come "deploy-prod". Fare clic sul pulsante **Salva** . Quindi fare clic su **Pipeline** per tornare alla pipeline.
    
    ![Distribuzione nell'ambiente di produzione](./media/how-to-ci-cd/deploy-to-prod.png)

7. Attualmente, l'artefatto di compilazione verrà generato in modo continuativo nella fase **QA** e quindi nella fase **PROD**. Ma la maggior parte delle volte è necessario integrare alcuni test case sui dispositivi QA e approvare manualmente i bit. In un secondo momento verranno distribuiti i bit all'ambiente PROD. Consente di impostare un'approvazione nella fase PROD come indicato di seguito.

    1. Aprire il pannello delle impostazioni **Condizioni di pre-distribuzione**.

        ![Aprire le condizioni di pre-distribuzione](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Impostare **Abilitato** in **Approvazioni pre-distribuzione**. Quindi immettere l’input **Responsabili approvazione**. Fare quindi clic su **Salva**.
    
        ![Condizioni di configurazione](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. A questo punto la pipeline di versione è stata impostata come segue.

    ![Pipeline di versione](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Verificare la CI/CD di IoT Edge con la compilazione e le pipeline di rilascio

In questa sezione, si attiva una compilazione per far funzionare la pipeline CI/CD. Verificare che la distribuzione abbia esito positivo.

1. Per attivare un processo di compilazione, è possibile eseguire il push di un commit al repository del codice sorgente o attivarlo manualmente. È possibile attivare un processo di compilazione nella propria pipeline di compilazione facendo clic sul pulsante **Coda** come nell'immagine seguente.

    ![Trigger manuale](./media/how-to-ci-cd/manual-trigger.png)

2. Se la pipeline di compilazione viene completata correttamente, verrà attivata una versione nella fase **QA**. Passare ai log della pipeline di compilazione per vedere quanto segue.

    ![Log di compilazione](./media/how-to-ci-cd/build-logs.png)

3. Il completamento della distribuzione nella fase **QA** consente di attivare una notifica al responsabile approvazione. Passare alla pipeline di versione per vedere quanto segue. 

    ![In attesa di approvazione](./media/how-to-ci-cd/pending-approval.png)


4. Dopo che il responsabile di approvazione approva questa modifica, può essere eseguita la distribuzione a **PROD**.

    ![Distribuzione a prod](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Passaggi successivi

* Per altre informazioni sulla distribuzione IoT Edge, vedere [Understand IoT Edge deployments for single devices or at scale](module-deployment-monitoring.md) (Informazioni sulle distribuzioni IoT Edge per singoli dispositivi o su vasta scala)
* Eseguire le procedure per creare, aggiornare o eliminare una distribuzione in [Distribuire e monitorare i moduli di IoT Edge su larga scala](how-to-deploy-monitor.md).
