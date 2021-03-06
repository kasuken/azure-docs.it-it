---
title: Servizio o server MFA di Azure, in locale o nel cloud?
description: Come amministratore di Azure AD, è necessario che sia a conoscenza della versione di Multi-Factor Authentication che sarebbe consigliabile distribuire?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 696c331111fef9d6fc238b733e15230ab3ee1e56
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378017"
---
# <a name="which-version-of-azure-mfa-is-right-for-my-organization"></a>Quale versione di Azure MFA è più adatta alla mia organizzazione?

Prima di poter decidere come e dove distribuire Multi-Factor Authentication (MFA), è necessario rispondere a tre domande di base.

* [Cosa si sta tentando di proteggere?](#what-am-i-trying-to-secure)
* [Dove si trovano gli utenti?](#where-are-the-users-located)
* [Quali funzionalità sono necessarie?](#what-features-do-i-need)

In ognuna delle sezioni seguenti vengono fornite informazioni utili per rispondere alle domande precedenti.

## <a name="what-am-i-trying-to-secure"></a>Cosa si sta tentando di proteggere?

Per individuare la soluzione di verifica in due passaggi corretta, è necessario prima stabilire cosa si sta tentando di proteggere con un fattore di autenticazione aggiuntivo. Si tratta di un'applicazione in Azure? Oppure di un sistema di accesso remoto? L'identificazione degli elementi da proteggere consente di stabilire dove abilitare la modalità Multi-Factor Authentication.

| Cosa si intende proteggere | Autenticazione a più fattori nel cloud | Server MFA |
| --- |:---:|:---:|
| App prodotte direttamente da Microsoft |● |● |
| App SaaS nella Raccolta di app |● |  |
| Applicazioni Web pubblicate tramite il proxy di applicazione di Azure AD |● |  |
| Le applicazioni IIS non pubblicate tramite proxy app per Azure AD | |● |
| Accesso remoto, ad esempio VPN, RDG | ● | ● |

## <a name="where-are-the-users-located"></a>Dove si trovano gli utenti?

Successivamente, a seconda di dove si trovano gli utenti dell'organizzazione, è possibile determinare la soluzione corretta da usare: nel cloud o in locale con il server MFA.

| Ubicazione degli utenti | Autenticazione a più fattori nel cloud | Server MFA |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD e AD locale usando la federazione con AD FS |● |● |
| Azure AD e AD locale con Azure AD Connect, senza sincronizzazione dell'hash delle password o autenticazione pass-through |● |● |
| Azure AD e AD locale con Azure AD Connect, con sincronizzazione dell'hash delle password o autenticazione pass-through |● | |
| Active Directory locale | |● |

## <a name="what-features-do-i-need"></a>Quali funzionalità sono necessarie?

La tabella seguente confronta le funzionalità disponibili con Multi-Factor Authentication nel cloud e con il server Multi-Factor Authentication.

| Funzionalità | Autenticazione a più fattori nel cloud | Server MFA |
| --- |:---:|:---:|
| Notifica dell'app per dispositivi mobili come secondo fattore | ● | ● |
| Codice di verifica dell'app per dispositivi mobili come secondo fattore | ● | ● |
| Chiamata telefonica come secondo fattore | ● | ● |
| SMS unidirezionale come secondo fattore | ● | ● |
| Token hardware come secondo fattore | ● (Anteprima pubblica) | ● |
| Password di app per i client Office 365 che non supportano MFA | ● | |
| Controllo amministrazione sui metodi di autenticazione | ● | ● |
| Modalità PIN | | ● |
| Avviso di illecito | ● | ● |
| Report MFA | ● | ● |
| Bypass monouso | | ● |
| Messaggi di saluto personalizzati per le telefonate | ● | ● |
| ID chiamante personalizzabile per le telefonate | ● | ● |
| Indirizzi IP attendibili | ● | ● |
| Memorizzazione di MFA per dispositivi attendibili | ● | |
| Accesso condizionale | ● | ● |
| Cache |  | ● |

## <a name="next-steps"></a>Passaggi successivi

Ora che si sono apprese le differenze tra Azure Multi-Factor Authentication nel cloud e il server MFA in locale, è possibile configurare e usare Azure Multi-Factor Authentication. **Selezionare l'icona che rappresenta lo scenario**

<center>

[![MFA nel cloud](./media/concept-mfa-whichversion/cloud2.png)](howto-mfa-getstarted.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [![Server MFA](./media/concept-mfa-whichversion/server2.png)](howto-mfaserver-deploy.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
