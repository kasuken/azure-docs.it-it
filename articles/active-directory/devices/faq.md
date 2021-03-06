---
title: Domande frequenti sulla gestione dei dispositivi di Azure Active Directory | Microsoft Docs
description: Domande frequenti sulla gestione dei dispositivi di Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 9402147e2dab7fbf52fc893f339f6f3b8e112377
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515642"
---
# <a name="azure-active-directory-device-management-faq"></a>Domande frequenti sulla gestione dei dispositivi di Azure Active Directory

**D: Di recente è stato registrato un dispositivo. Perché non viene visualizzato nelle informazioni dell'utente all'interno del portale di Azure? Oppure Perché il proprietario del dispositivo è contrassegnato come N/D per i dispositivi aggiunti ad Azure AD ibrido?**
**R:** I dispositivi Windows 10 aggiunti ad Azure AD ibrido non vengono visualizzati tra i dispositivi UTENTE.
È necessario usare la visualizzazione Tutti i dispositivi nel portale di Azure. In alternativa, è possibile usare il cmdlet [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) di PowerShell.

Nei dispositivi utente vengono elencati solo i dispositivi seguenti:

- Tutti i dispositivi personali non aggiunti ad Azure AD ibrido 
- Tutti i dispositivi non Windows 10/Windows Server 2016
- Tutti i dispositivi non Windows 

--- 

**D: Come è possibile conoscere lo stato di registrazione del dispositivo client?**

**R:** È possibile usare il portale di Azure, selezionare Tutti i dispositivi e cercare il dispositivo tramite l'ID dispositivo. Controllare il valore nella colonna Tipo di join. In alcuni casi, è possibile che il dispositivo sia stato reimpostato o che ne sia stata ricreata l'immagine. È quindi importante controllare anche lo stato di registrazione del dispositivo all'interno del dispositivo:

- Per i dispositivi Windows 10 e Windows Server 2016 o versioni successive, eseguire dsregcmd.exe /status.
- Per le versioni di sistemi operativi di livello inferiore, eseguire "%programFiles%\Microsoft Workplace Join\autoworkplace.exe"

---

**D: Il record del dispositivo è presente nelle informazioni UTENTE nel portale di Azure e viene indicato come registrato nel dispositivo. La configurazione è corretta per l'uso dell'accesso condizionale?**

**R:** Lo stato di join del dispositivo, indicato dall'ID dispositivo, deve corrispondere a quello in Azure AD e soddisfare eventuali criteri di valutazione per l'accesso condizionale. Per altre informazioni, vedere [Richiedere dispositivi gestiti per l'accesso alle app cloud con l'accesso condizionale](../conditional-access/require-managed-devices.md).

---

**D: Ho eliminato un dispositivo nel portale di Azure o tramite Windows PowerShell, ma lo stato locale indica che è ancora registrato.**

**A:** Si tratta di un comportamento previsto da progettazione. Il dispositivo non avrà accesso alle risorse nel cloud. 

Se si vuole registrare nuovamente il dispositivo, è necessario farlo manualmente da quest'ultimo. 

Per cancellare lo stato di join dai dispositivi Windows 10 e Windows Server 2016 aggiunti a un dominio AD locale:

1.  Aprire il prompt dei comandi come amministratore.

2.  Digitare `dsregcmd.exe /debug /leave`.

3.  Disconnettersi e accedere per attivare l'attività pianificata che registra nuovamente il dispositivo con Azure AD. 

Per le versioni di sistemi operativi Windows di livello inferiore aggiunti a un dominio AD locale:

1.  Aprire il prompt dei comandi come amministratore.
2.  Digitare `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Digitare `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

**D: Perché vengono visualizzate voci di dispositivo duplicate nel portale di Azure?**

**R:**

-   Per i dispositivi Windows 10 e Windows Server 2016, se vengono effettuati tentativi ripetuti di rimozione o aggiunta dello stesso dispositivo, potrebbero essere visualizzate voci duplicate. 

-   Ciascun utente Windows che usa l'opzione Aggiungi account aziendale o dell'istituto di istruzione creerà un nuovo record di dispositivo con lo stesso nome.

-   Per le versioni di sistemi operativi Windows di livello inferiore aggiunte a un dominio AD locale tramite registrazione automatica verrà creato un nuovo record di dispositivo con lo stesso nome per ciascun utente del dominio che accede al dispositivo. 

-   Un computer aggiunto ad Azure AD cancellato, reinstallato e aggiunto nuovamente con lo stesso nome verrà visualizzato con un record diverso ma con lo stesso nome dispositivo.

---

**D: Perché un utente può comunque accedere alle risorse da un dispositivo disattivato nel portale di Azure?**

**R:** Potrebbe volerci fino a un'ora per applicare una revoca.

>[!Note] 
>Per quanto riguarda i dispositivi registrati, è consigliabile cancellarli per assicurare che gli utenti non possano accedere alle relative risorse. Per altre informazioni, vedere [Registrare i dispositivi per la gestione in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 
---

# <a name="azure-ad-join-faq"></a>Domande frequenti sull'aggiunta ad Azure AD

**D: In che modo è possibile separare un dispositivo aggiunto da Azure AD in locale sul dispositivo?**

**R:** 
- Per i dispositivi aggiunti all'identità ibrida di Azure AD, disattivare le registrazione automatica in modo che l'attività pianificata non registri nuovamente il dispositivo. A questo punto, aprire il prompt dei comandi come amministratore e digitare `dsregcmd.exe /debug /leave`. In alternativa, è possibile eseguire il comando come script tra più dispositivi per eseguire la separazione in blocco.

- Per i dispositivi puri aggiunti ad Azure AD, assicurarsi di avere un account amministratore locale offline o crearne uno poiché non sarà possibile accedere con le credenziali di un utente Azure AD. A questo punto, passare a **Impostazioni** > **Account** > **Accedi all'azienda o all'istituto di istruzione**. Selezionare l'account e fare clic su **Disconnetti**. Seguire le istruzioni e, quando richiesto, fornire le credenziali di amministratore locale. Riavviare il dispositivo per completare il processo di separazione.

---

**D: Gli utenti non possono eseguire la ricerca delle stampanti da dispositivi aggiunti ad Azure AD. In che modo è possibile abilitare la stampa a partire dai dispositivi aggiunti ad Azure AD?**

**R:** Per la distribuzione delle stampanti nei dispositivi aggiunti ad Azure AD, vedere [Stampa di cloud ibrido](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). È necessario un Server Windows locale per la distribuzione della stampa di cloud ibrido. Il servizio di stampa basato sul cloud non è attualmente disponibile. 

---

**D: Come è possibile connettersi a un dispositivo aggiunto a un'istanza remota di Azure AD?**
**R:** Per i dettagli, vedere l'articolo https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc.

---

**D: Perché viene visualizzato un messaggio che avvisa dell'impossibilità di raggiungere un determinato dispositivo?**

**R:** Se sono state configurate determinate regole di accesso condizionale per richiedere uno stato specifico del dispositivo e il dispositivo in questione non soddisfa i criteri, gli utenti vengono bloccati con questo messaggio. Esaminare le regole dei criteri di accesso condizionale e assicurarsi che il dispositivo soddisfi i criteri per evitare che venga visualizzato questo messaggio.

---

**D: Perché alcuni dei miei utenti non ottengono richieste di autenticazione a più fattori sui dispositivi aggiunti ad Azure AD?**

**R:** Se l'utente viene aggiunto o registra un dispositivo con Azure AD usando la Multi-Factor Authentication, il dispositivo stesso diventerà un secondo fattore attendibile per tale utente. Successivamente, ogni volta che lo stesso utente esegue l'accesso al dispositivo e a un'applicazione, Azure AD considera il dispositivo come secondo fattore e consente all'utente di accedere facilmente alle applicazioni senza ulteriori richieste di autenticazione a più fattori. Questa procedura non è applicabile a nessun altro utente che accede a tale dispositivo, in modo tale che a tutti gli altri utenti verrà comunque richiesta un'autenticazione a più fattori prima di accedere alle applicazioni che richiedono l'autenticazione.

---

**D: Perché viene visualizzato un messaggio di nome utente o password non corretti per un dispositivo appena aggiunto ad Azure AD?**

**R:** Di seguito vengono elencati alcuni motivi comuni per questo scenario:

- Le credenziali dell'utente non sono più valide.

- Il computer non riesce a comunicare con Azure Active Directory. Verificare la presenza di eventuali problemi di connettività di rete.

- Gli accessi federati richiedono un server di federazione con supporto per endpoint WS-Trust abilitati e accessibili. 

- È stata abilitata l'Autenticazione pass-through e l'utente ha una password temporanea che deve essere modificata quando si esegue l'accesso.

---

**D: Perché viene visualizzata una finestra di dialogo di errore quando si tenta di aggiungere il PC ad Azure AD?**

**R:** La finestra viene visualizzata quando si registra Azure Active Directory con Intune. Verificare che all'utente che sta cercando di aggiungere il PC ad Azure AD sia assegnata la licenza di Intune corretta. Per altre informazioni, vedere [Configurare la gestione dei dispositivi Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**D: Perché il tentativo di aggiunta di un PC ad Azure AD non è andato a buon fine ma non vengono comunque visualizzate informazioni sull'errore?**

**R:** È probabile che l'utente abbia effettuato l'accesso al dispositivo con l'account amministratore predefinito locale. Creare un account locale diverso prima di usare l'aggiunta ad Azure Active Directory per completare la configurazione. 

---

# <a name="hybrid-azure-ad-join-faq"></a>Domande frequenti sull'aggiunta ad Azure AD ibrido

**D: Dov'è possibile trovare informazioni sulla risoluzione dei problemi per poter diagnosticare errori che si sono verificati durante l'aggiunta ad Azure AD ibrido?**

**R:** Per informazioni sulla risoluzione dei problemi, vedere:

- [Risoluzione dei problemi di registrazione automatica di computer aggiunti al dominio di Azure AD - Windows 10 e Windows Server 2016](troubleshoot-hybrid-join-windows-current.md)

- [Risoluzione dei problemi di registrazione automatica di computer aggiunti al dominio di Azure AD per client di livello inferiore di Windows](troubleshoot-hybrid-join-windows-legacy.md)
 

---

# <a name="azure-ad-register-faq"></a>Domande frequenti sulla registrazione ad Azure AD

**D: È possibile registrare dispositivi Android o iOS in modalità BYOD?**

**R:** Sì, ma solo con il servizio di registrazione dispositivo di Azure e per clienti che usano ambienti ibridi. La modalità BYOD non è supportata con il servizio di registrazione dispositivo locale in AD FS.

**D: Come si registra un dispositivo macOS?**

**R:** Per registrare un dispositivo macOS:

1.  [Creare i criteri di conformità del dispositivo](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Definire i criteri di accesso condizionale per i dispositivi macOS](../active-directory-conditional-access-azure-portal.md) 

**Osservazioni:**

- Gli utenti che vengono inclusi nei criteri di accesso condizionale necessitano di una [versione di Office supportata per macOS](../conditional-access/technical-reference.md#client-apps-condition) per accedere alle risorse. 

- Durante il primo tentativo di accesso, agli utenti viene richiesto di registrare il dispositivo tramite il portale aziendale.

---
