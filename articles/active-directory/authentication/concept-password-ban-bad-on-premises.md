---
title: Anteprima della protezione password di Azure AD
description: Escludere password vulnerabili in Active Directory locale usando la funzionalità di anteprima di protezione password di Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: ca412e94f65c7e1ed9a547ec9dcabc62fac7d42f
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741828"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Anteprima: Applicare la protezione password di Azure AD per Windows Server Active Directory

|     |
| --- |
| La protezione password e l'elenco personalizzato di password escluse di Azure AD sono funzionalità in anteprima pubblica di Azure Active Directory. Per altre informazioni sulle funzionalità in anteprima, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

La protezione password di Azure AD è una nuova funzionalità in anteprima pubblica basata su Azure Active Directory (Azure AD) volta a migliorare i criteri password in un'organizzazione. La distribuzione locale della protezione password di Azure AD usa l'elenco globale e l'elenco personalizzato di password escluse archiviati in Azure AD ed esegue in locale gli stessi controlli svolti per le modifiche basate sul cloud di Azure AD.

Ci sono tre componenti software che costituiscono la protezione password di Azure AD:

* Il servizio proxy di protezione password di Azure AD viene eseguito in tutti i computer aggiunti a un dominio nella foresta di Active Directory corrente. Inoltra le richieste dai controller di dominio ad Azure AD e restituisce la risposta da Azure AD al controller di dominio.
* Il servizio agente del controller di dominio di protezione password di Azure AD riceve le richieste di convalida della password dalla DLL di filtro delle password dell'agente del controller di dominio, le elabora in base ai criteri password correnti disponibili localmente e restituisce il risultato di esito positivo o negativo. Questo servizio è responsabile di chiamare periodicamente (ogni ora) il servizio proxy di protezione password di Azure AD per recuperare le nuove versioni dei criteri password. Le comunicazioni per le chiamate da e verso il servizio proxy di protezione password di Azure AD vengono gestite tramite RPC (Remote Procedure Call) su TCP. Dopo il recupero, i nuovi criteri vengono archiviati in una cartella sysvol, dove possono essere replicati in altri controller di dominio. Il servizio agente del controller di dominio monitora anche la cartella sysvol per rilevare eventuali modifiche nel caso in cui altri controller di dominio abbiano scritto nuovi criteri password. Se sono già disponibili criteri adeguatamente recenti, il controllo del servizio proxy di protezione password di Azure AD viene ignorato.
* La DLL di filtro delle password dell'agente del controller di dominio riceve le richieste di convalida delle password dal sistema operativo e le inoltra al servizio agente del controller di dominio di protezione password di Azure AD in esecuzione in locale nel controller di dominio.

![Come si integrano i componenti della password di protezione di Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="license-requirements"></a>Requisiti relativi alle licenze

I vantaggi dell'elenco globale di password escluse si applicano a tutti gli utenti di Azure Active Directory (Azure AD).

L'elenco personalizzato di password escluse richiede licenze di Azure AD Basic.

La protezione password di Azure AD per Windows Server Active Directory richiede licenze di Azure AD Premium.

Informazioni aggiuntive sulle licenze, inclusi i costi, sono disponibili nella pagina [Prezzi di Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Download

Sono necessari due programmi di installazione per la password di protezione di Azure AD che possono essere scaricati dall'[area download Microsoft](https://www.microsoft.com/download/details.aspx?id=57071).

## <a name="answers-to-common-questions"></a>Risposte alle domande comuni

* Non è richiesta la connettività Internet dai controller di dominio. I computer che eseguono il servizio proxy di protezione password di Azure AD sono gli unici che richiedono la connettività Internet.
* Non vengono aperte porte di rete nei controller di dominio.
* Non sono necessarie modifiche dello schema di Active Directory.
* Il software usa il contenitore Active Directory esistente e gli oggetti dello schema serviceConnectionPoint.
* Non ci sono requisiti minimi per il livello funzionale della foresta o del dominio di Active Directory.
* Il software non crea né richiede account nei domini di Active Directory protetti.
* La distribuzione incrementale è supportata, ma i criteri password vengono applicati solo dove è installato l'agente del controller di dominio.
* È consigliabile installare l'agente del controller di dominio su tutti i controller di dominio per garantire l'imposizione della protezione della password. 
* La protezione password di Azure AD non è un motore di applicazione dei criteri in tempo reale. Potrebbe esserci un ritardo tra una modifica della configurazione dei criteri password e la sua distribuzione e applicazione in tutti i controller di dominio.

## <a name="next-steps"></a>Passaggi successivi

[Distribuire la protezione delle password di Azure AD](howto-password-ban-bad-on-premises-deploy.md)
