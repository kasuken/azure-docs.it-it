---
title: Connettersi ad Azure Stack con CLI | Microsoft Docs
description: Informazioni su come usare l'interfaccia della riga di comando multipiattaforma (CLI) per gestire e distribuire le risorse in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 2c862dcaf5f9267265879faa8ac927ddf7515419
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277274"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>Usare i profili delle versioni API con il comando di Azure in Azure Stack

È possibile seguire i passaggi descritti in questo articolo per informazioni su backup l'interfaccia della riga di comando di Azure per gestire le risorse di Azure Stack Development Kit dalle piattaforme client Linux, Mac e Windows.

## <a name="install-cli"></a>Installare l'interfaccia della riga di comando

Accedere alla workstation di sviluppo e installare CLI. Azure Stack richiede la versione 2.0 o versione successiva della riga di comando di Azure. È possibile installare che tramite la procedura descritta nel [installare CLI Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) articolo. Per verificare se l'installazione è riuscita, aprire un terminale o una finestra del prompt dei comandi ed eseguire il comando seguente:

```azurecli
az --version
```

Dovrebbe visualizzare la versione della CLI di Azure e altre librerie dipendenti installate nel computer.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Considerare attendibile il certificato di autorità di certificazione di Azure Stack radice

1. Ottenere il certificato di radice della CA di Azure Stack [l'operatore di Azure Stack](..\azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) e attendibile. Per rendere attendibile il certificato radice CA Azure Stack, aggiungerlo al certificato Python esistente.

1. Trovare il percorso di certificato nel computer. Il percorso può variare in base alla quale è stato installato Python. Dovrai disporre [pip](https://pip.pypa.io) e il [quella ottenibile](https://pypi.org/project/certifi/) modulo installato. È possibile usare il comando Python seguito dal prompt di bash:

  ```bash  
    python -c "import certifi; print(certifi.where())"
  ```

  Prendere nota del percorso di certificati. Ad esempio: `~/lib/python3.5/site-packages/certifi/cacert.pem`. Il percorso specifica varia in base al sistema operativo e la versione di Python installata.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Impostare il percorso per un computer di sviluppo all'interno di cloud

Se si eseguono CLI da un computer Linux che viene creato all'interno dell'ambiente Azure Stack, eseguire il comando bash seguente con il percorso del certificato.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Impostare il percorso per un computer di sviluppo all'esterno del cloud

Se si eseguono CLI da un computer **esterno** l'ambiente Azure Stack:  

1. È necessario configurare [connettività VPN ad Azure Stack](azure-stack-connect-azure-stack.md).

1. Copiare il certificato con estensione PEM ottenuto da operatore di Azure Stack e prendere nota del percorso del file (PATH_TO_PEM_FILE).

1. Eseguire i comandi seguenti, a seconda della finale nel sistema operativo della workstation di sviluppo.

#### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="windows"></a>Windows

```powershell
$pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

$root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$root.Import($pemFile)

Write-Host "Extracting needed information from the cert file"
$md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Ottenere l'endpoint di alias di macchina virtuale

Prima che gli utenti possono creare macchine virtuali con CLI, devono contattare l'operatore di Azure Stack e ottenere l'URI dell'endpoint alias macchina virtuale. Ad esempio, Azure Usa l'URI seguente: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. L'amministratore del cloud deve configurare un endpoint simile per Azure Stack con le immagini disponibili nel marketplace di Azure Stack. Gli utenti devono passare l'URI dell'endpoint per il `endpoint-vm-image-alias-doc` parametro per il `az cloud register` seguente come mostrato nella sezione successiva. 
   

## <a name="connect-to-azure-stack"></a>Connettersi ad Azure Stack

Usare la procedura seguente per connettersi ad Azure Stack:

1. Registrare l'ambiente dello Stack di Azure eseguendo il `az cloud register` comando.
   
   a. Per registrare il *cloud amministrativi* ambiente, usare:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

   b. Per registrare il *utente* ambiente, usare:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```
    c. Per registrare il *utente* in un ambiente multi-tenancy, usare:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases> \
        --endpoint-active-directory-resource-id=<URI of the ActiveDirectoryServiceEndpointResourceID> \
        --profile 2018-03-01-hybrid
      ```

1. Impostare l'ambiente attivo usando i comandi seguenti.

   a. Per il *cloud amministrativi* ambiente, usare:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

   b. Per il *utente* ambiente, usare:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

1. Aggiornare la configurazione dell'ambiente per utilizzare il profilo di versione API specifico dello Stack di Azure. Per aggiornare la configurazione, eseguire il comando seguente:

   ```azurecli
   az cloud update \
     --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Se si esegue una versione di Azure Stack prima della compilazione 1808, è necessario disporre usare il profilo di versione API **2017-03-09-profile** anziché il profilo di versione API **2018-03-01-hybrid**.

1. Accedere all'ambiente Azure Stack usando il `az login` comando. È possibile accedere all'ambiente Azure Stack come un utente o come un [entità servizio](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects). 

    * Ambienti di AAD
      * Accedere come un *utente*: È possibile specificare il nome utente e password direttamente all'interno di `az login` comando o eseguire l'autenticazione usando un browser. È necessario farlo se l'account è abilitata l'autenticazione a più fattori.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Se l'account utente è attivata l'autenticazione a più fattori, è possibile usare la `az login command` senza fornire il `-u` parametro. Esecuzione del comando fornisce un URL e un codice che è necessario usare per eseguire l'autenticazione.
   
      * Accedere come un *entità servizio*: prima di accedere, [creare un'entità servizio tramite il portale di Azure](azure-stack-create-service-principals.md) o CLI e assegnarle un ruolo. Accedere a questo punto, usando il comando seguente:

      ```azurecli
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```
    * Ambienti di AD FS

        * Accedere come un *entità servizio*: 
          1.    Preparare il file con estensione PEM da utilizzare per l'accesso dell'entità servizio.
                * Nel computer client in cui l'entità è stata creata, esportare il certificato dell'entità servizio come un file pfx con la chiave privata (che si trova in cert: \CurrentUser\My; il nome del certificato ha lo stesso nome dell'entità).

                *   Convertire il file pfx in pem (utilità di uso OpenSSL).

          1.    Effettuare l'accesso all'interfaccia della riga di comando. :
                ```azurecli
                az login --service-principal \
                 -u <Client ID from the Service Principal details> \
                 -p <Certificate's fully qualified name. Eg. C:\certs\spn.pem>
                 --tenant <Tenant ID> \
                 --debug 
                ```

## <a name="test-the-connectivity"></a>Testare la connettività

Ora che abbiamo tutto quello che è possibile usare CLI per creare risorse in Azure Stack, programma di installazione. Ad esempio, è possibile creare un gruppo di risorse per un'applicazione e aggiungere una macchina virtuale. Usare il comando seguente per creare un gruppo di risorse denominato "MyResourceGroup":

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Se il gruppo di risorse viene creato correttamente, il comando precedente restituisce le proprietà seguenti della risorsa appena creata:

![Gruppo di risorse creare output](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Problemi noti
Esistono alcuni problemi noti che è necessario considerare quando si utilizza CLI in Azure Stack:

 - Ovvero la modalità interattiva dell'interfaccia della riga il `az interactive` comando non è ancora supportato in Azure Stack.
 - Per ottenere l'elenco delle immagini di macchina virtuale disponibili in Azure Stack, usare il `az vm image list --all` comando anziché la `az vm image list` comando. Specifica il `--all` opzione assicura che risposta restituisce solo le immagini disponibili nell'ambiente Azure Stack.
 - Gli alias di immagine di macchina virtuale disponibili in Azure potrebbero non essere applicabili ad Azure Stack. Quando si usano immagini di macchine virtuali, è necessario usare l'intero parametro URN (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) anziché l'alias dell'immagine. Questo URN deve corrispondere le specifiche di immagine in base il `az vm images list` comando.

## <a name="next-steps"></a>Passaggi successivi

[Distribuire modelli con l'interfaccia della riga di comando di Azure](azure-stack-deploy-template-command-line.md)

[Abilitare la CLI di Azure per gli utenti di Azure Stack (operatore)](..\azure-stack-cli-admin.md)

[Gestire le autorizzazioni utente](azure-stack-manage-permissions.md)
