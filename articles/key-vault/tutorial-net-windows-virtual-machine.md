---
title: 'Esercitazione: Come usare Azure Key Vault con una macchina virtuale Windows di Azure in .NET | Microsoft Docs'
description: "Esercitazione: Configurare un'applicazione ASP.NET Core per la lettura di un segreto da un insieme di credenziali delle chiavi"
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: d1f24c8bebc8740f47dc0f02089db1091c22f597
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51711328"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-windows-virtual-machine-in-net"></a>Esercitazione: Come usare Azure Key Vault con una macchina virtuale Windows di Azure in .NET

Azure Key Vault facilita la protezione di segreti come le chiavi API, le stringhe di connessione di database necessarie per accedere alle applicazioni, i servizi e le risorse IT.

In questa esercitazione si eseguiranno i passaggi necessari per fare in modo che un'applicazione Console legga informazioni da Azure Key Vault usando le identità gestite per le risorse di Azure. L'esercitazione è basata su [App Web di Azure](../app-service/app-service-web-overview.md). Nel corso dell'esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare un insieme di credenziali delle chiavi.
> * Archiviare un segreto nell'insieme di credenziali delle chiavi.
> * Recuperare un segreto dall'insieme di credenziali delle chiavi.
> * Creare una macchina virtuale di Azure.
> * Abilitare un'[identità gestita](../active-directory/managed-identities-azure-resources/overview.md) per la macchina virtuale.
> * Concedere le autorizzazioni necessarie per l'applicazione console per la lettura dei dati dall'insieme di credenziali delle chiavi.
> * Recuperare segreti da Key Vault.

Prima di continuare, leggere i [concetti di base](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Prerequisiti
* Tutte le piattaforme:
  * Git ([download](https://git-scm.com/downloads)).
  * Una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.
  * [Interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versione 2.0.4 o versioni successive. È disponibile per Windows, Mac e Linux.

Questa esercitazione usa un'identità del servizio gestita.

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Che cos'è un'identità del servizio gestita e come funziona?
Prima di continuare, è importante conoscere i concetti alla base dell'identità del servizio gestita. Azure Key Vault consente di archiviare le credenziali in una posizione sicura in modo che non siano incluse nel codice, tuttavia per recuperarle è necessario eseguire l'autenticazione con Azure Key Vault. E per eseguire l'autenticazione con Key Vault servono le credenziali. Un classico problema di bootstrap. Tramite Azure e Azure AD, l'identità del servizio gestita offre una "identità di bootstrap" che semplifica notevolmente il processo iniziale.

Ecco come funziona. Quando si abilita l'identità del servizio gestita per un servizio di Azure come Macchine virtuali, Servizio app o Funzioni, Azure crea un'[entità servizio](key-vault-whatis.md#basic-concepts) per l'istanza del servizio in Azure Active Directory e inserisce le credenziali per l'entità servizio nell'istanza del servizio. 

![Identità del servizio gestita](media/MSI.png)

Il codice chiama quindi un servizio di metadati locale disponibile nella risorsa di Azure per ottenere un token di accesso.
Il codice usa il token di accesso ottenuto dal MSI_ENDPOINT locale per eseguire l'autenticazione con un servizio Azure Key Vault. 

## <a name="log-in-to-azure"></a>Accedere ad Azure

Per accedere ad Azure tramite l'interfaccia della riga di comando di Azure, immettere:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Creare un gruppo di risorse con il comando [az group create](/cli/azure/group#az-group-create). Un gruppo di risorse di Azure è un contenitore logico in cui le risorse di Azure vengono distribuite e gestite.

Selezionare il nome di un gruppo di risorse e compilare il segnaposto.
L'esempio seguente crea un gruppo di risorse nell'area Stati Uniti occidentali:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Il gruppo di risorse appena creato viene usato in tutto l'articolo.

## <a name="create-a-key-vault"></a>Creare un insieme di credenziali delle chiavi

Viene ora creato un insieme di credenziali delle chiavi nel gruppo di risorse creato nel passaggio precedente. Specificare le informazioni seguenti:

* Nome dell'insieme di credenziali delle chiavi: il nome deve essere una stringa di 3-24 caratteri e deve contenere solo (0-9, a-z, A-Z e -).
* Nome del gruppo di risorse.
* Località: **Stati Uniti occidentali**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
A questo punto, l'account Azure è l'unico autorizzato a eseguire qualsiasi operazione su questo nuovo insieme di credenziali.

## <a name="add-a-secret-to-the-key-vault"></a>Aggiungere un segreto all'insieme di credenziali delle chiavi

Verrà aggiunto un segreto per illustrare il funzionamento di questa operazione. Si potrebbe archiviare una stringa di connessione SQL o qualsiasi altra informazione che è necessario conservare in modo sicuro, rendendola però disponibile per l'applicazione.

Digitare i comandi seguenti per creare un segreto nell'insieme di credenziali delle chiavi denominato **AppSecret**. Questo segreto archivierà il valore **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>Creare una macchina virtuale
Visitare i collegamenti seguenti per creare una macchina virtuale Windows

[Interfaccia della riga di comando di Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli) 

[PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)

[Portale](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-identity-to-virtual-machine"></a>Assegnare un'identità alla macchina virtuale
In questo passaggio si crea un'identità assegnata dal sistema per la macchina virtuale eseguendo questo comando nell'interfaccia della riga di comando di Azure

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Si noti l'elemento systemAssignedIdentity illustrato di seguito. L'output del comando precedente sarà 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-vm-identity-permission-to-key-vault"></a>Concedere all'identità della VM l'autorizzazione per Key Vault
È ora possibile concedere all'identità creata sopra l'autorizzazione per Key Vault eseguendo questo comando

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Accedere alla macchina virtuale

È possibile seguire questa [esercitazione](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)

## <a name="install-net-core"></a>Installare .NET Core

È possibile installare .NET Core seguendo la procedura illustrata in questo [articolo](https://www.microsoft.com/net/download)

## <a name="create-and-run-sample-dot-net-app"></a>Creare ed eseguire un'app .NET di esempio

Aprire un prompt dei comandi

Eseguendo questi comandi, nella console verrà visualizzato "Hello World"

```
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-console-app"></a>Modificare l'app console
Aprire il file Program.cs e aggiungere questi pacchetti
```
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```
Modificare quindi il file di classe in modo da includere il codice riportato di seguito. È un processo in 2 passaggi. 
1. Recuperare un token dall'endpoint locale dell'identità del servizio gestita nella VM, che a propria volta recupera un token da Azure Active Directory
2. Passare il token a Key Vault e recuperare il segreto 

```
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from local (URI) Managed Service Identity endpoint which in turn fetches it from Azure Active Directory
            var token = GetToken();

            // Step 2: Fetch the secret value from Key Vault
            System.Console.WriteLine(FetchSecretValueFromKeyVault(token));
        }

        static string GetToken()
        {
            WebRequest request = WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net");
            request.Headers.Add("Metadata", "true");
            WebResponse response = request.GetResponse();
            return ParseWebResponse(response, "access_token");
        }

        static string FetchSecretValueFromKeyVault(string token)
        {
            WebRequest kvRequest = WebRequest.Create("https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01");
            kvRequest.Headers.Add("Authorization", "Bearer "+  token);
            WebResponse kvResponse = kvRequest.GetResponse();
            return ParseWebResponse(kvResponse, "value");
        }

        private static string ParseWebResponse(WebResponse response, string tokenName)
        {
            string token = String.Empty;
            using (Stream stream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                String responseString = reader.ReadToEnd();

                JObject joResponse = JObject.Parse(responseString);    
                JValue ojObject = (JValue)joResponse[tokenName];             
                token = ojObject.Value.ToString();
            }
            return token;
        }
    }
```


Il codice precedente illustra come eseguire operazioni con Azure Key Vault in una macchina virtuale Linux di Azure. 



## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [API REST di Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
