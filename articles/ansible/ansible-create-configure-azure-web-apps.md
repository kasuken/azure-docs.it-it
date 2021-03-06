---
title: Creare app Web di Azure con Ansible (anteprima)
description: Informazioni su come usare Ansible per creare un'app Web con il runtime di Java 8 e del contenitore Tomcat nel servizio app in Linux
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, Servizio app di Azure, App Web, Java
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: 48b4c201b2b96bd4662e8c90be7298a4f418af53
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426556"
---
# <a name="create-azure-app-service-web-apps-by-using-ansible-preview"></a>Creare app Web del servizio app di Azure con Ansible (anteprima)
Le [app Web del servizio app di Azure ](https://docs.microsoft.com/azure/app-service/app-service-web-overview), o semplicemente app Web, consentono l'hosting di applicazioni Web, API REST e back-end mobili. È possibile usare il linguaggio di sviluppo preferito &mdash; .NET, .NET Core, Java, Ruby, Node.js, PHP o Python.

Ansible consente di automatizzare la distribuzione e la configurazione delle risorse nell'ambiente in uso. Questo articolo illustra come usare Ansible per creare un'app Web usando il runtime di Java. 

## <a name="prerequisites"></a>Prerequisiti
- **Sottoscrizione di Azure** - Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) prima di iniziare.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> In questa esercitazione, per eseguire i playbook di esempio seguenti è necessario Ansible 2.7. È possibile installare la versione RC 2.7 di Ansible eseguendo `sudo pip install ansible[azure]==2.7.0rc2`. Dopo il rilascio di Ansible 2.7, non sarà necessario specificare la versione qui perché la versione predefinita sarà 2.7. 

## <a name="create-a-simple-app-service"></a>Creare un servizio app semplice
In questa sezione viene presentato un playbook Ansible di esempio che definisce le seguenti risorse:
- Gruppo di risorse, in cui verranno distribuiti l'app Web e il piano di servizio app
- App Web con il runtime di Java 8 e del contenitore Tomcat nel servizio app in Linux

```
- hosts: localhost
  connection: local
  vars:
    resource_group: myfirstResourceGroup
    webapp_name: myfirstWebApp
    location: eastus
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

    - name: Create App Service on Linux with Java Runtime
      azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: myappplan
          is_linux: true
          sku: S1
          number_of_workers: 1
        frameworks:
          - name: "java"
            version: "8"
            settings:
              java_container: tomcat
              java_container_version: 8.5
```
Salvare il playbook precedente come **firstwebapp.yml**.

Per eseguire il playbook, usare il comando **ansible-playbook** come segue:
```bash
ansible-playbook firstwebapp.yml
```

L'output generato dall'esecuzione del playbook Ansible indica che l'app Web è stata creata correttamente:

```
TASK [Create a resource group] *************************************************
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ******************************
 [WARNING]: Azure API profile latest does not define an entry for
WebSiteManagementClient
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0   
```

## <a name="create-an-app-service-by-using-traffic-manager"></a>Creare un servizio app tramite Gestione traffico
È possibile usare [Gestione traffico di Azure](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager) per controllare il modo in cui le richieste dai client Web vengono distribuite alle app nel servizio app di Azure. Quando si aggiungono endpoint di servizio app a un profilo di Gestione traffico di Azure, Gestione traffico rileva lo stato delle app del servizio app. Lo stato può essere in esecuzione, arrestata ed eliminata. Gestione traffico può quindi decidere quali di questi endpoint devono ricevere il traffico.

Nel servizio app un'app viene eseguita in un [piano di servizio app](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview
). Un piano di servizio app definisce un set di risorse di calcolo per l'esecuzione di un'app Web. È possibile gestire il piano di servizio app e l'app Web in gruppi diversi.

In questa sezione viene presentato un playbook Ansible di esempio che definisce le seguenti risorse:
- Gruppo di risorse, in cui verrà distribuito il piano di servizio app
- Piano di servizio app
- Gruppo di risorse secondario, in cui verrà distribuita l'app Web
- App Web con il runtime di Java 8 e del contenitore Tomcat nel servizio app in Linux
- Profilo di Gestione traffico
- Endpoint di Gestione traffico, con sito Web creato

```
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_resource_group: planResourceGroup
    app_name: myLinuxWebApp
    location: eastus
    linux_plan_name: myAppServicePlan
    traffic_manager_profile_name: myTrafficManagerProfile
    traffic_manager_endpoint_name: myTrafficManagerEndpoint

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

  - name: Create secondary resource group
    azure_rm_resourcegroup:
        name: "{{ plan_resource_group }}"
        location: "{{ location }}"

  - name: Create App Service Plan
    azure_rm_appserviceplan:
      resource_group: "{{ plan_resource_group }}"
      name: "{{ linux_plan_name }}"
      location: "{{ location }}"
      is_linux: true
      sku: S1
      number_of_workers: 1

  - name: Create App Service on Linux with Java Runtime
    azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ app_name }}"
        plan:
          resource_group: "{{ plan_resource_group }}"
          name: "{{ linux_plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        app_settings:
          testkey: "testvalue"
        frameworks:
          - name: java
            version: 8
            settings:
              java_container: "Tomcat"
              java_container_version: "8.5"

  - name: Get web app facts
    azure_rm_webapp_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ app_name }}"
    register: webapp
    
  - name: Create Traffic Manager Profile
    azure_rm_trafficmanagerprofile:
      resource_group: "{{ resource_group }}"
      name: "{{ traffic_manager_profile_name }}"
      location: global
      routing_method: performance
      dns_config:
        relative_name: "{{ traffic_manager_profile_name }}"
        ttl:  60
      monitor_config:
        protocol: HTTPS
        port: 80
        path: '/'

  - name: Add endpoint to traffic manager profile, using created web site
    azure_rm_trafficmanagerendpoint:
      resource_group: "{{ resource_group }}"
      profile_name: "{{ traffic_manager_profile_name }}"
      name: "{{ traffic_manager_endpoint_name }}"
      type: azure_endpoints
      location: "{{ location }}"
      target_resource_id: "{{ webapp.webapps[0].id }}"

```
Salvare il playbook precedente come **webapp.yml** o [scaricare il playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/webapp.yml).

Per eseguire il playbook, usare il comando **ansible-playbook** come segue:
```bash
ansible-playbook webapp.yml
```

L'output generato dall'esecuzione del playbook Ansible indica che il piano di servizio app, l'app Web, il profilo di Gestione traffico e l'endpoint sono stati creati correttamente:
```
TASK [Create resource group] ****************************************************************************
changed: [localhost]

TASK [Create resource group for app service plan] ****************************************************************************
changed: [localhost]

TASK [Create App Service Plan] ****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ****************************************************************************
changed: [localhost]

TASK [Get web app facts] *****************************************************************************
ok: [localhost]

TASK [Create Traffic Manager Profile] *****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for TrafficManagerManagementClient

changed: [localhost]

TASK [Add endpoint to traffic manager profile, using the web site created above] *****************************************************************************
changed: [localhost]

TASK [Get Traffic Manager Profile facts] ******************************************************************************
ok: [localhost]

PLAY RECAP ******************************************************************************
localhost                  : ok=9    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Passaggi successivi
> [!div class="nextstepaction"] 
> [Ansible in Azure](https://docs.microsoft.com/azure/ansible/)