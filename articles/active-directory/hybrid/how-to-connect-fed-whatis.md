---
title: Azure AD Connect e federazione | Documentazione Microsoft
description: Questa pagina è il punto centrale per tutta la documentazione correlata alle operazioni di ADFS che usano Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 214dd95bb277053794656e1ba3dd148c085688ce
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900445"
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect e federazione
Azure Active Directory (Azure AD) consente di configurare la federazione con Active Directory Federation Services (ADFS) locale e Azure AD. Grazie all'accesso federato, gli utenti possono accedere ai servizi basati su Azure AD con le proprie password locali e, se usano la rete aziendale, senza dover immettere di nuovo le password. Usando l'opzione di federazione con ADFS, è possibile distribuire una nuova installazione di ADFS oppure specificare un'installazione esistente in una farm di Windows Server 2012 R2.

Questo argomento è il punto centrale per le informazioni sulle funzionalità relative alla federazione per Azure AD Connect e contiene un elenco di collegamenti ad altri argomenti correlati. Per collegamenti relativi ad Azure AD Connect, vedere [Integrazione delle identità locali con Azure Active Directory](whatis-hybrid-identity.md).

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect - Argomenti sulla federazione
| Argomento | Contenuti e destinatari |
|:--- |:--- |
| **Opzioni di accesso utente di Azure AD Connect** | |
| [Informazioni sulle opzioni di accesso utente](plan-connect-user-signin.md) |Informazioni sulle diverse opzioni di accesso utente e del modo in cui influiscono sull'esperienza utente di accesso di Azure. |
| **Installare ADFS con Azure AD Connect** | |
| [Prerequisiti](how-to-connect-install-custom.md#ad-fs-configuration-pre-requisites) |Vedere i prerequisiti per una corretta installazione di ADFS con Azure AD Connect. |
| [Configurare una farm ADFS](how-to-connect-install-custom.md#configuring-federation-with-ad-fs) |Installare una nuova farm ADFS tramite Azure AD Connect. |
| [Eseguire la federazione con Azure AD usando un ID di accesso alternativo](how-to-connect-fed-management.md#alternateid) | Configurare la federazione usando un ID di accesso alternativo.  |
| **Modifica della configurazione di ADFS** | |
| [Ripristinare il trust](how-to-connect-fed-management.md#repairthetrust) |Ripristinare il trust corrente tra l'istanza di ADFS locale e Office 365/Azure. |
| [Aggiungere un nuovo server ADFS](how-to-connect-fed-management.md#addadfsserver) |Espandere la farm ADFS con un server ADFS aggiuntivo dopo l'installazione iniziale. |
| [Aggiungere un nuovo server WAP ADFS](how-to-connect-fed-management.md#addwapserver) |Espandere la farm ADFS con un server Web Application Proxy (WAP) aggiuntivo dopo l'installazione iniziale. |
| [Aggiungere un nuovo dominio federato](how-to-connect-fed-management.md#addfeddomain) |Aggiungere un altro dominio per la federazione con Azure AD. |
| [Aggiornare il certificato SSL](how-to-connect-fed-ssl-update.md)| Aggiornare il certificato SSL per una farm ADFS. |
| [Rinnovare i certificati di federazione per Office 365 e Azure AD](how-to-connect-fed-o365-certs.md)|Rinnovare il certificato per Office 365 con Azure AD.|
| **Configurazione aggiuntiva della federazione** | |
| [Eseguire la federazione di più istanze di Azure AD con una singola istanza di AD FS](how-to-connect-fed-single-adfs-multitenant-federation.md) | Eseguire la federazione di più istanze di Azure AD con una singola farm AD FS.| 
| [Aggiungere l'illustrazione o il logo personalizzato della società](how-to-connect-fed-management.md#customlogo) |Come modificare l'esperienza di accesso specificando il logo personalizzato che viene visualizzato nella pagina di accesso di ADFS. |
| [Aggiungere una descrizione di accesso](how-to-connect-fed-management.md#addsignindescription) |Modificare la descrizione di accesso nella pagina di accesso di ADFS. |
| [Modificare le regole attestazioni per AD FS](how-to-connect-fed-management.md#modclaims) |Modificare o aggiungere le regole attestazioni ADFS corrispondenti alla configurazione della sincronizzazione di Azure AD Connect. |


## <a name="additional-resources"></a>Risorse aggiuntive
* [Eseguire la federazione di due istanze di Azure AD con una singola istanza di AD FS](how-to-connect-fed-single-adfs-multitenant-federation.md)
* [Distribuzione di AD FS in Azure](how-to-connect-fed-azure-adfs.md)
* [Distribuzione di ADFS a disponibilità elevata tra aree geografiche in Azure con Gestione traffico di Azure](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
