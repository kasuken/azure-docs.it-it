---
title: Autenticazione basata su certificati di Azure Active Directory in iOS
description: Informazioni sugli scenari supportati e i requisiti per configurare l'autenticazione basata su certificati nelle soluzioni con i dispositivi iOS
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: annaba
ms.openlocfilehash: 655fa6b4bf0f04f2d88e9a3f11cb9d3917ea3dd3
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43346546"
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>Autenticazione basata su certificati di Azure Active Directory in iOS

I dispositivi iOS possono usare l'autenticazione basata sui certificati per eseguire l'autenticazione in Azure Active Directory usando un certificato client sul dispositivo durante la connessione a:

* Applicazioni Office per dispositivi mobili, come Microsoft Outlook e Microsoft Word
* Client Exchange ActiveSync (EAS)

La configurazione di questa funzionalità elimina la necessità di immettere una combinazione di nome utente e password in determinate applicazioni di posta e applicazioni Microsoft Office sul dispositivo mobile.

Questo argomento indica i requisiti e gli scenari supportati per la configurazione dell'autenticazione basata su certificati su un dispositivo iOS (Android) per gli utenti dei tenant nei piani di Office 365 Enterprise, Business, Education, US Government, China e Germany.

Questa funzionalità è disponibile in anteprima nei piani di Office 365 US Government Defense e Federal.

## <a name="microsoft-mobile-applications-support"></a>Supporto delle applicazioni per dispositivi mobili Microsoft

| App | Supporto |
| --- | --- |
| App Azure Information Protection |![Controllo][1] |
| Portale aziendale Intune |![Controllo][1] |
| Microsoft Teams |![Controllo][1] |
| OneNote |![Controllo][1] |
| OneDrive |![Controllo][1] |
| Outlook |![Controllo][1] |
| Power BI |![Controllo][1] |
| Skype for Business Online |![Controllo][1] |
| Word / Excel / PowerPoint |![Controllo][1] |
| Yammer |![Controllo][1] |

## <a name="requirements"></a>Requisiti

La versione del sistema operativo del dispositivo deve essere iOS 9 o successiva

È necessario configurare un server federativo.

Per le applicazioni Office in iOS è richiesto Microsoft Authenticator.

Perché Azure Active Directory possa revocare un certificato client, il token ADFS deve avere le attestazioni seguenti:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (Il numero di serie del certificato client)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (La stringa per l'autorità emittente del certificato client)

Azure Active Directory aggiunge queste attestazioni per il token di aggiornamento se sono disponibili nel token ADFS (o in qualsiasi altro token SAML). Quando il token di aggiornamento deve essere convalidato, queste informazioni vengono usate per controllare la revoca.

Come procedura consigliata, è necessario aggiornare le pagine di errore di AD FS dell'organizzazione con le informazioni seguenti:

* Il requisito dell'installazione di Microsoft Authenticator in iOS
* Istruzioni su come ottenere un certificato utente.

Per altre informazioni, vedere [Personalizzazione delle pagine di accesso ad AD FS](https://technet.microsoft.com/library/dn280950.aspx).

Alcune app di Office, in cui non è abilitata l'autenticazione moderna, inviano "*prompt=login*" ad Azure AD nella richiesta. Per impostazione predefinita, Azure AD traduce "*prompt=login*" nella richiesta ad AD FS di eseguire l'autorizzazione di nome utente e password, ovvero "*wauth=usernamepassworduri*", e di ignorare lo stato SSO ed eseguire una nuova autenticazione, ovvero "*wfresh=0*". Per abilitare l'autenticazione basata su certificati per queste applicazioni, è necessario modificare il comportamento predefinito di Azure AD. È sufficiente impostare "*PromptLoginBehavior*" tra le impostazioni del dominio federato su "*Disabled*" (Disabilitato).
Per eseguire questa operazione è possibile usare il cmdlet [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0):

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Supporto dei client Exchange ActiveSync

In iOS 9 o versioni successive è supportato il client di posta iOS nativo. Per tutte le altre applicazioni Exchange ActiveSync, contattare lo sviluppatore dell'applicazione per determinare se questa funzionalità è supportata.

## <a name="next-steps"></a>Passaggi successivi

Se si desidera configurare l'autenticazione basata su certificati nel proprio ambiente, vedere [Introduzione all'autenticazione basata su certificati in Android](../authentication/active-directory-certificate-based-authentication-get-started.md) per le istruzioni.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
