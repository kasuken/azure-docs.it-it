---
title: Usare un'identità gestita assegnata dal sistema per una macchina virtuale Linux per accedere ad Azure Cosmos DB
description: Esercitazione che illustra come usare un'identità gestita assegnata dal sistema per una macchina virtuale Linux per accedere ad Azure Cosmos DB.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: daveba
ms.openlocfilehash: 07e13930b5778c94bc85097668b9a793fae8773f
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51625556"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-managed-identity-to-access-azure-cosmos-db"></a>Esercitazione: Usare un'identità gestita assegnata dal sistema per una macchina virtuale Linux per accedere ad Azure Cosmos DB 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


Questa esercitazione mostra come usare un'identità gestita assegnata dal sistema per una macchina virtuale Linux per accedere ad Azure Cosmos DB. Si apprenderà come:

> [!div class="checklist"]
> * Creare un account Cosmos DB
> * Creare una raccolta nell'account Cosmos DB
> * Concedere l'accesso a un'istanza di Azure Cosmos DB all'identità gestita assegnata dal sistema
> * Recuperare il valore di `principalID` dell'identità gestita assegnata dal sistema della macchina virtuale Linux
> * Ottenere un token di accesso e usarlo per chiamare Azure Resource Manager
> * Ottenere le chiavi di accesso da Azure Resource Manager per eseguire le chiamate a Cosmos DB

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

Per eseguire gli esempi di script dell'interfaccia della riga di comando in questa esercitazione sono disponibili due opzioni:

- Usare [Azure Cloud Shell](~/articles/cloud-shell/overview.md) tramite il portale di Azure o il pulsante **Prova**, che si trova nell'angolo in alto a destra di ogni blocco di codice.
- [Installare la versione più recente dell'interfaccia della riga di comando 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 o successiva) se si preferisce usare una console dell'interfaccia della riga di comando locale.

## <a name="create-a-cosmos-db-account"></a>Creare un account Cosmos DB 

Se non ne è già disponibile uno, creare un account Cosmos DB. È possibile ignorare questo passaggio e usare un account Cosmos DB esistente. 

1. Fare clic sul pulsante **+/Crea nuovo servizio** nell'angolo superiore sinistro del portale di Azure.
2. Fare clic su **Database**, quindi su **Azure Cosmos DB**. Verrà visualizzato un nuovo pannello "Nuovo account".
3. Immettere un **ID** per l'account Cosmos DB, da usare in un secondo momento.  
4. **API** deve essere impostato su "SQL". L'approccio descritto in questa esercitazione può essere usato con gli altri tipi di API disponibili, ma i passaggi illustrati si riferiscono all'API SQL.
5. Verificare che le impostazioni in **Sottoscrizione** e **Gruppo di risorse** corrispondano a quelle specificate al momento della creazione della macchina virtuale nel passaggio precedente.  Selezionare un **Percorso** in cui è disponibile Cosmos DB.
6. Fare clic su **Create**(Crea).

## <a name="create-a-collection-in-the-cosmos-db-account"></a>Creare una raccolta nell'account Cosmos DB

Successivamente, aggiungere una raccolta di dati nell'account Cosmos DB su cui è possibile eseguire query nei passaggi successivi.

1. Passare all'account Cosmos DB appena creato.
2. Nella scheda **Panoramica** fare clic sul pulsante **+/Aggiungi raccolta**. Verrà visualizzato il pannello "Aggiungi raccolta".
3. Assegnare alla raccolta un ID database e un ID raccolta, selezionare una capacità di archiviazione, immettere una chiave di partizione, specificare un valore di velocità effettiva, quindi fare clic su **OK**.  Per questa esercitazione, è sufficiente usare "Test" come ID database e ID raccolta, selezionare una capacità di archiviazione fissa e una velocità effettiva più bassa (400 UR/sec).  

## <a name="retrieve-the-principalid-of-the-linux-vms-system-assigned-managed-identity"></a>Recuperare il valore di `principalID` dell'identità gestita assegnata dal sistema della macchina virtuale Linux

Per ottenere l'accesso alle chiavi di accesso dell'account Cosmos DB da Resource Manager nella sezione seguente, è necessario recuperare il valore di `principalID` dell'identità gestita assegnata dal sistema della macchina virtuale Linux.  Assicurarsi di sostituire i valori dei parametri `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` (gruppo di risorse in cui si trova la macchina virtuale) e `<VM NAME>` con valori personalizzati.

```azurecli-interactive
az resource show --id /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe> --api-version 2017-12-01
```
La risposta include i dettagli dell'identità gestita assegnata dal sistema. Prendere nota del valore principalID perché verrà usato nella sezione successiva:

```bash  
{
    "id": "/subscriptions/<SUBSCRIPTION ID>/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe>",
  "identity": {
    "principalId": "6891c322-314a-4e85-b129-52cf2daf47bd",
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533f8",
    "type": "SystemAssigned"
 }

```
## <a name="grant-your-linux-vms-system-assigned-identity-access-to-the-cosmos-db-account-access-keys"></a>Concedere l'accesso alle chiavi di accesso dell'account Cosmos DB all'identità assegnata dal sistema della macchina virtuale Linux

Cosmos DB non supporta l'autenticazione di Azure AD in modo nativo. È tuttavia possibile usare un'identità gestita per recuperare una chiave di accesso Cosmos DB da Resource Manager e usarla per accedere a Cosmos DB. In questo passaggio si concede l'accesso alle chiavi dell'account Cosmos DB all'identità gestita assegnata dal sistema.

Per concedere all'identità gestita assegnata dal sistema l'accesso all'account Cosmos DB in Azure Resource Manager tramite l'interfaccia della riga di comando di Azure, aggiornare i valori per `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` e `<COSMOS DB ACCOUNT NAME>` per l'ambiente in uso. Sostituire `<MI PRINCIPALID>` con la proprietà `principalId` restituita dal comando `az resource show` in [Recuperare il valore principalID dell'identità gestita della macchina virtuale Linux](#retrieve-the-principalID-of-the-linux-VM's-system-assigned-identity).  Cosmos DB supporta due livelli di granularità quando si usano le chiavi di accesso: accesso all'account in lettura/scrittura e in sola lettura.  Assegnare il ruolo `DocumentDB Account Contributor` se si desidera ottenere le chiavi di lettura/scrittura per l'account o assegnare il ruolo `Cosmos DB Account Reader Role` se si desidera ottenere le chiavi di sola lettura per l'account:

```azurecli-interactive
az role assignment create --assignee <MI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

La risposta include i dettagli relativi all'assegnazione di ruolo creata:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>/providers/Microsoft.Authorization/roleAssignments/5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "name": "5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "properties": {
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-linux-vms-system-assigned-managed-identity-and-use-it-to-call-azure-resource-manager"></a>Ottenere un token di accesso usando l'identità gestita assegnata dal sistema della macchina virtuale Linux e usarlo per chiamare Azure Resource Manager

La parte rimanente dell'esercitazione prevede che le operazioni vengano svolte dalla macchina virtuale creata in precedenza.

Per completare questi passaggi, è necessario disporre di un client SSH. Se si usa Windows, è possibile usare il client SSH nel [sottosistema Windows per Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Per richiedere assistenza nella configurazione delle chiavi del client SSH, vedere [Come usare le chiavi SSH con Windows in Azure](../../virtual-machines/linux/ssh-from-windows.md) o [Come creare e usare una coppia di chiavi SSH pubblica e privata per le macchine virtuali Linux in Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Nel portale di Azure passare a **Macchine virtuali**, selezionare la macchina virtuale Linux e quindi nella parte superiore della pagina **Panoramica** fare clic su **Connetti**. Copiare la stringa di connessione alla macchina virtuale. 
2. Connettersi alla macchina virtuale tramite il client SSH.  
3. Successivamente viene richiesto di immettere la **Password** aggiunta durante la creazione della **macchina virtuale Linux**. A questo punto l'accesso è stato eseguito correttamente.  
4. Usare CURL per ottenere un token di accesso per Azure Resource Manager: 
     
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
 
    > [!NOTE]
    > Nella richiesta precedente il valore del parametro "risorsa" deve corrispondere esattamente a quello previsto da Azure AD. Quando si usa l'ID risorsa di Azure Resource Manager, è necessario includere la barra finale nell'URI.
    > Nella risposta seguente l'elemento access_token è stato abbreviato per comodità.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Ottenere le chiavi di accesso da Azure Resource Manager per eseguire le chiamate a Cosmos DB  

Di seguito viene usato CURL per chiamare Resource Manager usando il token di accesso recuperato nella sezione precedente per recuperare la chiave di accesso dell'account Cosmos DB. Dopo avere ottenuto la chiave di accesso, è possibile eseguire una query su Cosmos DB. Sostituire i valori dei parametri `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` e `<COSMOS DB ACCOUNT NAME>` con i valori desiderati. Sostituire il valore `<ACCESS TOKEN>` con il token di accesso recuperato in precedenza.  Se si desidera recuperare le chiavi di lettura/scrittura, usare il tipo di operazione della chiave `listKeys`.  Se si desidera recuperare le chiavi di sola lettura, usare il tipo di operazione della chiave `readonlykeys`:

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> Per il testo nell'URL precedente viene fatta distinzione tra maiuscole e minuscole. Assicurarsi quindi di usare la stessa combinazione di maiuscole e minuscole usata per i gruppi di risorse. Inoltre, è importante sapere che si tratta di una richiesta POST, non di una richiesta GET, e verificare di passare un valore per acquisire un limite di lunghezza con -d che può essere NULL.  

La risposta CURL restituisce l'elenco di chiavi.  Ad esempio, se si ottengono le chiavi di sola lettura:  

```bash 
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```

Ora che si è in possesso della chiave di accesso per l'account Cosmos DB, è possibile trasferirla a un SDK Cosmos DB ed eseguire chiamate per accedere all'account.  Per un esempio rapido, è possibile passare la chiave di accesso all'interfaccia della riga di comando di Azure.  È possibile ottenere il valore <COSMOS DB CONNECTION URL> dalla scheda **Panoramica** nel pannello dell'account Cosmos DB nel portale di Azure.  Sostituire <ACCESS KEY> con il valore ottenuto in precedenza:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Questo comando dell'interfaccia della riga di comando restituisce i dettagli sulla raccolta:

```bash
{
  "collection": {
    "_conflicts": "conflicts/",
    "_docs": "docs/",
    "_etag": "\"00006700-0000-0000-0000-5a8271e90000\"",
    "_rid": "Es5SAM2FDwA=",
    "_self": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/",
    "_sprocs": "sprocs/",
    "_triggers": "triggers/",
    "_ts": 1518498281,
    "_udfs": "udfs/",
    "id": "Test",
    "indexingPolicy": {
      "automatic": true,
      "excludedPaths": [],
      "includedPaths": [
        {
          "indexes": [
            {
              "dataType": "Number",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "String",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "Point",
              "kind": "Spatial"
            }
          ],
          "path": "/*"
        }
      ],
      "indexingMode": "consistent"
    }
  },
  "offer": {
    "_etag": "\"00006800-0000-0000-0000-5a8271ea0000\"",
    "_rid": "f4V+",
    "_self": "offers/f4V+/",
    "_ts": 1518498282,
    "content": {
      "offerIsRUPerMinuteThroughputEnabled": false,
      "offerThroughput": 400
    },
    "id": "f4V+",
    "offerResourceId": "Es5SAM2FDwA=",
    "offerType": "Invalid",
    "offerVersion": "V2",
    "resource": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/"
  }
}
```

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come usare un'identità gestita assegnata dal sistema in una macchina virtuale Linux per accedere a Cosmos DB.  Per altre informazioni su Cosmos DB, consultare:

> [!div class="nextstepaction"]
>[Panoramica di Azure Cosmos DB](/azure/cosmos-db/introduction)

