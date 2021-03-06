---
title: Registrare il ASDK con Azure | Microsoft Docs
description: Viene descritto come registrare Azure Stack con Azure per abilitare la diffusione di marketplace e report sull'utilizzo.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: e86ff4ebf91d0c0b691caf429d9489bf769f16af
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/19/2018
ms.locfileid: "51975193"
---
# <a name="azure-stack-registration"></a>Registrazione con Azure Stack
È possibile registrare l'installazione di Azure Stack Development Kit (ASDK) con Azure per scaricare elementi di marketplace di Azure e per impostare i dati di e-commerce segnalazioni a Microsoft. È necessario eseguire la registrazione per supportare la funzionalità di Azure Stack completa, tra cui diffusione di marketplace. La registrazione è consigliata perché consente di testare le funzionalità di Azure Stack importanti come la diffusione di marketplace e report sull'utilizzo. Dopo la registrazione di Azure Stack, sull'utilizzo viene segnalato ad Azure commerce. È possibile visualizzarlo nella sottoscrizione che è usata per la registrazione. Tuttavia, gli utenti ASDK non vengono addebitate spese per qualsiasi utilizzo che generano report.

Se non si registrano i ASDK, è possibile visualizzare un **attivazione obbligatoria** messaggio di avviso che richiede di eseguire la registrazione di Azure Stack Development Kit. Si tratta di un comportamento previsto.

## <a name="prerequisites"></a>Prerequisiti
Prima di usare queste istruzioni per registrare il ASDK con Azure, assicurarsi di avere installato di PowerShell per Azure Stack e scaricare gli strumenti di Azure Stack come descritto nel [configurazione post-distribuzione](asdk-post-deploy.md) articolo.

Inoltre, la modalità di linguaggio di PowerShell deve essere impostata su **FullLanguageMode** nel computer usato per registrare il ASDK con Azure. Per verificare che la modalità linguaggio corrente è impostata su full, aprire una finestra di PowerShell con privilegi elevata ed eseguire i comandi PowerShell seguenti:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Verificare che l'output restituisce **FullLanguageMode**. Se viene restituita qualsiasi altra modalità di linguaggio, registrazione dovrà essere eseguito in un altro computer o la modalità di linguaggio dovrà essere impostata su **FullLanguageMode** prima di continuare.

## <a name="register-azure-stack-with-azure"></a>Registrare Azure Stack con Azure
Seguire questi passaggi per registrare il ASDK con Azure.

> [!NOTE]
> Tutti questi passaggi devono essere eseguiti da un computer dotato di accesso all'endpoint con privilegi. Per ASDK, ovvero il computer host kit di sviluppo.

1. Aprire una console PowerShell come amministratore.  

2. Eseguire i comandi di PowerShell seguenti per registrare l'installazione ASDK con Azure. È necessario accedere a sia la sottoscrizione di Azure e l'installazione ASDK locale. Se non hai una sottoscrizione di Azure, è possibile [crea qui un account Azure gratuito](https://azure.microsoft.com/free/?b=17.06). La registrazione di Azure Stack viene addebitata alcuna tariffa nella sottoscrizione di Azure.<br><br>Se si esegue lo script di registrazione in più di un'istanza di Azure Stack usando lo stesso ID di sottoscrizione di Azure, impostare un nome univoco per la registrazione quando si esegue la **Set-AzsRegistration** cmdlet. Il **RegistrationName** parametro ha valore predefinito è **AzureStackRegistration**. Tuttavia, se si usa lo stesso nome in più di un'istanza di Azure Stack, lo script avrà esito negativo.

    ```PowerShell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
    Add-AzureRmAccount -EnvironmentName "<environment name>"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    #Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    #Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    $RegistrationName = "<unique-registration-name>"
    Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint AzS-ERCS01 `
    -BillingModel Development `
    -RegistrationName $RegistrationName `
    -UsageReportingEnabled:$true
    ```
3. Al termine dell'esecuzione dello script, verrà visualizzato questo messaggio: **l'ambiente è ora registrato e attivati mediante i parametri specificati.**

    ![L'ambiente è ora registrato](media/asdk-register/1.PNG)


## <a name="register-in-disconnected-environments"></a>Registrare in ambienti non connessi
Se si sta registrando Azure Stack in un ambiente disconnesso (con nessuna connettività a internet), è necessario ottenere una registrazione token dall'ambiente Azure Stack e quindi usare il token in un computer in grado di connettersi ad Azure per registrare e creare un'attivazione risorse per l'ambiente ASDK.
 
 > [!IMPORTANT]
 > Prima di usare queste istruzioni per registrare Azure Stack, assicurarsi di avere installato PowerShell per Azure Stack e scaricare gli strumenti di Azure Stack come descritto nel [configurazione post-distribuzione](asdk-post-deploy.md) articolo nell'host sia ASDK computer e il computer con accesso a internet utilizzato per connettersi ad Azure e registrazione.

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Ottenere una registrazione token dall'ambiente Azure Stack
Nel computer host ASDK, avviare PowerShell come amministratore e passare al **registrazione** cartella le **AzureStack-strumenti-master** directory creata quando è stato scaricato gli strumenti di Azure Stack. Usare i comandi di PowerShell seguenti per importare i **RegisterWithAzure.psm1** modulo e quindi utilizzare il **Get-AzsRegistrationToken** per ottenere il token di registrazione:  

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $CloudAdminCred `
   -UsageReportingEnabled:$false `
   -PrivilegedEndpoint AzS-ERCS01 `
   -BillingModel Development `
   -MarketplaceSyndicationEnabled:$false `
   -TokenOutputFilePath $FilePathForRegistrationToken
   ```
Per impostazione predefinita, il token di registrazione viene salvato nel file specificato per *$FilePathForRegistrationToken* parametro. È possibile modificare il percorso file o file a propria discrezione.

Salvare questo token di registrazione per l'uso nel computer connesso a internet. È possibile copiare il file o il testo da $FilePathForRegistrationToken.

### <a name="connect-to-azure-and-register"></a>Connettersi ad Azure e registrazione
Nel computer connesso a internet, avviare PowerShell come amministratore e passare al **registrazione** cartella le **AzureStack-strumenti-master** directory creata al momento del download di Azure Stack di strumenti. Usare i comandi di PowerShell seguenti per importare i **RegisterWithAzure.psm1** modulo e quindi utilizzare il **Register-AzsEnvironment** cmdlet per registrazione in Azure, fornendo la registrazione del token è appena creato e un nome di registrazione univoci:  

  ```PowerShell  
  $registrationToken = "<your registration token>"
  $RegistrationName = "<unique registration name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

In alternativa, è possibile usare la **Get-Content** cmdlet in modo che punti a un file che contiene il token di registrazione:

  ```PowerShell  
  $registrationToken = Get-Content -Path '<path>\<registration token file>'
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

Salvare il token di registrazione e il nome di risorsa di registrazione per riferimento futuro.

### <a name="retrieve-an-activation-key-from-the-azure-registration-resource"></a>Recuperare una chiave di attivazione dalla risorsa di registrazione di Azure

Usa ancora il computer connesso a internet, recuperare una chiave di attivazione dalla risorsa di registrazione creata durante la registrazione con Azure.

Per ottenere la chiave di attivazione, eseguire i seguenti comandi di PowerShell, usare lo stesso valore di nome di registrazione univoci che è specificato durante la registrazione con Azure nel passaggio precedente:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName `
  -KeyOutputFilePath $KeyOutputFilePath
  ```

La chiave di attivazione viene salvata nel file specificato per *$KeyOutputFilePath*. È possibile modificare il percorso file o file a propria discrezione.

### <a name="create-an-activation-resource-in-azure-stack"></a>Crea una risorsa di attivazione in Azure Stack

Tornare all'ambiente di Azure Stack con il file o il testo dalla chiave di attivazione creato da **Get-AzsActivationKey**. Eseguire i comandi di PowerShell seguenti per creare una risorsa di attivazione in Azure Stack tramite tale chiave di attivazione:   

  ```Powershell
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

In alternativa, è possibile usare la **Get-Content** cmdlet in modo che punti a un file che contiene il token di registrazione:

  ```Powershell
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  $ActivationKey = Get-Content -Path '<path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

## <a name="verify-the-registration-was-successful"></a>Verificare che la registrazione ha avuto esito positivo
Seguire questi passaggi per verificare che la registrazione ASDK con Azure è riuscita.

1. Accedi per il [portale di amministrazione di Azure Stack](https://adminportal.local.azurestack.external).

2. Fare clic su **Marketplace Management** > **aggiungere da Azure**.

    ![](media/asdk-register/2.PNG)

3. Se viene visualizzato un elenco di elementi disponibili in Azure, l'attivazione è riuscita.

    ![](media/asdk-register/3.PNG)

## <a name="move-a-registration-resource"></a>Spostare una risorsa di registrazione
Lo spostamento di una risorsa di registrazione tra gruppi di risorse nella stessa sottoscrizione **è** supportati. Per altre informazioni sullo spostamento delle risorse in un nuovo gruppo di risorse, vedere [spostare le risorse in un nuovo gruppo di risorse o sottoscrizione](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Passaggi successivi
[Aggiungere un elemento del marketplace Azure Stack](.\.\azure-stack-marketplace.md)
