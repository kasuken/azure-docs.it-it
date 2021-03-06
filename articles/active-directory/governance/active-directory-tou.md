---
title: Condizioni per l'utilizzo di Azure Active Directory| Microsoft Docs
description: Condizioni per l'utilizzo di Azure Active Directory consente a un'azienda di specificare condizioni per l'utilizzo per gli utenti dei servizi di Azure AD.
services: active-directory
author: rolyon
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 11/02/2018
ms.author: rolyon
ms.openlocfilehash: 8fddcdbb8aa523cf3a98a8f2b203440ceedbdf06
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51015206"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Funzionalità Condizioni per l'utilizzo di Azure Active Directory
Condizioni per l'utilizzo di Azure Active Directory offre un sistema semplice che le organizzazioni possono usare per presentare le informazioni agli utenti finali. In questo modo si garantisce che gli utenti vedano le dichiarazioni rilevanti di non responsabilità che si riferiscono ai requisiti legali o di conformità. Questo articolo descrive come iniziare con Condizioni per l'utilizzo.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Panoramica video

Il video seguente presenta una rapida panoramica di Condizioni per l'utilizzo.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Per altri video, vedere:
- [Come distribuire Condizioni per l'utilizzo in Azure Active Directory](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Come distribuire Condizioni per l'utilizzo in Azure Active Directory](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>Quali operazioni si possono eseguire con Condizioni per l'utilizzo?
Condizioni per l'utilizzo di Azure AD consente di eseguire le operazioni seguenti:
- Fare accettare le condizioni per l'utilizzo ai dipendenti o agli ospiti prima dell'accesso.
- Presentare condizioni per l'utilizzo generali per tutti gli utenti dell'organizzazione.
- Presentare condizioni per l'utilizzo specifiche basate sugli attributi di un utente (ad esempio, medici o infermieri oppure dipendenti nazionali o internazionali, tramite [gruppi dinamici](../users-groups-roles/groups-dynamic-membership.md)).
- Presentare condizioni per l'utilizzo specifiche al momento dell'accesso ad applicazioni a elevato impatto aziendale, come Salesforce.
- Presentare condizioni per l'utilizzo in lingue diverse.
- Contribuire a rispettare le normative sulla privacy.
- Elencare gli utenti hanno accettato o rifiutato le condizioni per l'utilizzo.
- Visualizzare un log dell'attività di Condizioni per l'utilizzo per conformità e controllo.
- Creare e gestire Condizioni per l'utilizzo con [API Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/agreement) (attualmente in anteprima).

## <a name="prerequisites"></a>Prerequisiti
Per usare e configurare Condizioni per l'utilizzo di Azure AD, è necessario disporre di:

- Una sottoscrizione di Azure AD Premium P1, P2, EMS E3 o EMS E5.
    - Se non si dispone di una di queste sottoscrizioni, è possibile [ottenere Azure AD Premium](../fundamentals/active-directory-get-started-premium.md) oppure [abilitare la versione di valutazione di Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).
- Uno dei seguenti account di amministratore per la directory da configurare:
    - Amministratore globale
    - Amministratore della sicurezza
    - Amministratore di accesso condizionale

## <a name="terms-of-use-document"></a>Documento sulle condizioni per l'utilizzo

Il contenuto di Condizioni per l'utilizzo di Azure AD viene presentato in formato PDF. Il contenuto del file PDF può essere di qualsiasi tipo, ad esempio documenti di contratti esistenti. Questo consente di acquisire il consenso degli utenti finali durante l'accesso. La dimensione consigliata per i caratteri nel file PDF è 24.

## <a name="add-terms-of-use"></a>Aggiungere le condizioni per l'utilizzo
Dopo avere completato le condizioni per l'utilizzo, seguire questa procedura per aggiungerle.

1. Accedere ad Azure come amministratore globale, amministratore della sicurezza o amministratore di accesso condizionale.

1. Passare a **Condizioni per l'utilizzo** all'indirizzo [https://aka.ms/catou](https://aka.ms/catou).

    ![Pannello di Condizioni per l'utilizzo](./media/active-directory-tou/tou-blade.png)

1. Fare clic su **Nuove condizioni**.

    ![Aggiungere le condizioni per l'utilizzo](./media/active-directory-tou/new-tou.png)

1. Immettere il **Nome** per le condizioni per l'utilizzo

2. Immettere il **Nome visualizzato**.  Questa è l'intestazione visualizzata dagli utenti quando effettuano l'accesso.

3. Fare clic su **Sfoglia** per trovare il PDF delle condizioni per l'utilizzo completate e selezionarlo.

4. **Selezionare** una lingua per le condizioni per l'utilizzo.  L'opzione relativa alla lingua consente di caricare più versioni delle condizioni per l'utilizzo, ognuna in una lingua diversa.  La versione visualizzata all'utente finale dipenderà dalle preferenze del browser.

5. Attivare o disattivare l'opzione **Richiedi agli utenti di espandere le Condizioni d'uso**.  Se questa opzione è attivata, agli utenti finali verrà richiesto di visualizzare le condizioni per l'utilizzo prima dell'accettazione.

6. In **Accesso condizionale** è possibile **applicare** le condizioni per l'utilizzo caricate selezionando un modello dall'elenco a discesa o criteri di accesso condizionale personalizzati.  I criteri personalizzati di accesso condizionale consentono condizioni per l'utilizzo granulari, fino a un'applicazione cloud o a un gruppo di utenti specifici.  Per altre informazioni, vedere la pagina relativa alla [configurazione dei criteri di accesso condizionale](../../cognitive-services/qnamaker/concepts/best-practices.md).

    >[!IMPORTANT]
    >L'applicazione dei controlli dei criteri di accesso condizionale (incluse le condizioni per l'utilizzo) non è supportata per gli account di servizio.  È consigliabile escludere tutti gli account di servizio dai criteri di accesso condizionale.

7. Fare clic su **Create**(Crea).

8. Se è stato selezionato un modello personalizzato di accesso condizionale, verrà visualizzata una nuova schermata che consente di personalizzare il criterio di accesso condizionale.

    A questo punto dovrebbero essere visualizzate le nuove condizioni per l'utilizzo.

    ![Aggiungere le condizioni per l'utilizzo](./media/active-directory-tou/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Visualizzare il report degli utenti che hanno accettato e rifiutato
Nel pannello delle condizioni per l'utilizzo è visualizzato il numero di utenti che hanno accettato e rifiutato. Questi conteggi e gli utenti che hanno accettato o rifiutato vengono archiviati per tutta la durata delle condizioni per l'utilizzo.

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [ https://aka.ms/catou ](https://aka.ms/catou).

    ![Evento di controllo](./media/active-directory-tou/view-tou.png)

1. Fare clic sui numeri sotto **Accettato** o **Rifiutato** per visualizzare lo stato corrente degli utenti.

    ![Evento di controllo](./media/active-directory-tou/accepted-tou.png)

## <a name="view-azure-ad-audit-logs"></a>Visualizzare i log di controllo di Azure AD
Se si vuole visualizzare attività aggiuntive, Condizioni per l'utilizzo di Azure Active Directory include log di controllo. Ogni consenso dell'utente genera un evento nei log di controllo che viene conservato per 30 giorni. È possibile visualizzare questi log nel portale o scaricarli come file CSV.

Per iniziare a usare i log di controllo di Azure AD, seguire questa procedura:

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [ https://aka.ms/catou ](https://aka.ms/catou).

1. Fare clic su **Visualizza log di controllo**.

    ![Evento di controllo](./media/active-directory-tou/audit-tou.png)

1. Nella schermata dei log di controllo di Azure AD è possibile filtrare le informazioni usando i menu a discesa per visualizzare informazioni specifiche dei log di controllo.

    ![Evento di controllo](./media/active-directory-tou/audit-logs-tou.png)

1. È inoltre possibile fare clic su **Download** per scaricare le informazioni in un file con estensione CSV per l'uso in locale.

## <a name="what-terms-of-use-looks-like-for-users"></a>Presentazione agli utenti delle condizioni per l'utilizzo
Una volta create e applicate le condizioni per l'utilizzo, gli utenti inclusi nell'ambito visualizzeranno la schermata seguente durante l'accesso.

![Evento di controllo](./media/active-directory-tou/user-tou.png)

La schermata seguente illustra l'aspetto delle condizioni per l'utilizzo nei dispositivi mobili.

![Evento di controllo](./media/active-directory-tou/mobile-tou.png)

Gli utenti devono solamente accettare le Condizioni per l'utilizzo una sola volta e non vedranno nuovamente le Condizioni per l'utilizzo agli accessi successivi.

### <a name="how-users-can-review-their-terms-of-use"></a>Visualizzazione delle condizioni per l'utilizzo da parte degli utenti
Gli utenti possono visualizzare e verificare le condizioni per l'utilizzo accettate usando la procedura seguente.

1. Accedere a [https://myapps.microsoft.com](https://myapps.microsoft.com).

1. Nell'angolo superiore destro fare clic sul proprio nome e selezionare **Profilo** nel menu a discesa.

    ![Profilo](./media/active-directory-tou/tou14.png)

1. Nella pagina Profilo fare clic su **Verificare le condizioni d'uso**.

    ![Evento di controllo](./media/active-directory-tou/tou13a.png)

1. Sarà quindi possibile verificare le condizioni per l'utilizzo accettate. 

## <a name="edit-terms-of-use-details"></a>Modificare i dettagli delle condizioni per l'utilizzo
È possibile modificare alcuni dettagli delle condizioni per l'utilizzo, ma non è possibile modificare un documento esistente. La procedura seguente descrive come modificare i dettagli.

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [ https://aka.ms/catou ](https://aka.ms/catou).

1. Selezionare le condizioni per l'utilizzo da modificare.

1. Fare clic su **Modifica le condizioni**.

1. Nel riquadro della modifica delle condizioni per l'utilizzo, modificare il nome, il nome visualizzato o richiedere che gli utenti espandano i valori.

    ![Aggiungere le condizioni per l'utilizzo](./media/active-directory-tou/edit-tou.png)

1. Fare clic su **Salva** per salvare le modifiche.

    Dopo aver salvato le modifiche, gli utenti dovranno accettare nuovamente le condizioni nuove.

## <a name="add-a-terms-of-use-language"></a>Aggiungere una lingua delle condizioni per l'utilizzo
La procedura seguente descrive come aggiungere una lingua delle condizioni per l'utilizzo.

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [ https://aka.ms/catou ](https://aka.ms/catou).

1. Selezionare le condizioni per l'utilizzo da modificare.

1. Nel riquadro dei dettagli fare clic sulla scheda **Lingue**.

    ![Aggiungere le condizioni per l'utilizzo](./media/active-directory-tou/languages-tou.png)

1. Fare clic su **Aggiungi lingua**.

1. Nel riquadro Aggiungi la lingua delle condizioni per l'utilizzo caricare il PDF localizzato e selezionare la lingua.

    ![Aggiungere le condizioni per l'utilizzo](./media/active-directory-tou/language-add-tou.png)

1. Fare clic su **Aggiungi** per aggiungere la lingua.

## <a name="delete-terms-of-use"></a>Eliminare le condizioni per l'utilizzo
È possibile eliminare le condizioni per l'utilizzo obsolete con la procedura seguente.

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [ https://aka.ms/catou ](https://aka.ms/catou).

1. Selezionare le condizioni per l'utilizzo da rimuovere.

1. Fare clic su **Elimina le condizioni**.

1. Nel messaggio visualizzato in cui viene chiesto se si vuole continuare fare clic su **Sì**.

    ![Aggiungere le condizioni per l'utilizzo](./media/active-directory-tou/delete-tou.png)

    A questo punto le condizioni per l'utilizzo non dovrebbero più essere visualizzate.

## <a name="deleted-users-and-active-terms-of-use"></a>Utenti eliminati e condizioni per l'utilizzo attive
Per impostazione predefinita, un utente eliminato rimane comunque in Azure AD per 30 giorni, periodo durante il quale un amministratore potrà eseguirne il ripristino se necessario.  Dopo 30 giorni l'utente verrà eliminato definitivamente.  Inoltre, tramite il portale di Azure Active Directory, un amministratore globale può in modo esplicito [eliminare definitivamente un utente eliminato di recente](../fundamentals/active-directory-users-restore.md) prima della scadenza di tale periodo.  Dopo l'eliminazione definitiva di un utente, i dati riguardanti tale utente verranno rimossi dalle condizioni per l'utilizzo attive.  Le informazioni di controllo sugli utenti eliminati restano nel log di controllo.

## <a name="policy-changes"></a>Modifiche dei criteri
I criteri di accesso condizionale diventano effettivi immediatamente. In questo caso l'amministratore inizierà a vedere nuvolette "tristi" o messaggi relativi a problemi di token di Azure AD. Dovrà quindi disconnettersi e accedere nuovamente per soddisfare il nuovo criterio.

>[!IMPORTANT]
> Gli utenti inclusi nell'ambito devono disconnettersi e accedere per soddisfare un nuovo criterio se:
> - è abilitato un criterio di accesso condizionale per una condizione per l'utilizzo
> - o viene creata una seconda condizione

## <a name="frequently-asked-questions"></a>Domande frequenti

**D: come posso sapere se un utente ha accettato le condizioni per l'utilizzo?**</br>
R: Nel pannello Condizioni per l'utilizzo fare clic sul numero sotto **Accettato**. È anche possibile visualizzare o cercare l'attività accettata nei log di controllo di Azure AD. Per altre informazioni, vedere [Visualizzare il report degli utenti che hanno accettato e rifiutato](#view-who-has-accepted-and-declined) e [Visualizzare i log di controllo di Azure AD](#view-azure-ad-audit-logs).

**D: per quanto tempo sono archiviate le informazioni?**</br>
R: i conteggi inclusi nel report delle Condizioni per l'utilizzo e gli utenti che hanno accettato o rifiutato vengono archiviati per tutta la durata delle Condizioni per l'utilizzo. I log di controllo di Azure AD vengono archiviati per 30 giorni.

**D: perché viene visualizzato un numero diverso di consensi nel report delle Condizioni per l'utilizzo rispetto ai log di controllo di Azure AD?**</br>
R: il report delle Condizioni per l'utilizzo viene archiviato per la durata di tale Condizioni per l'utilizzo, mentre i log di controllo di Azure AD vengono archiviati per 30 giorni. Inoltre, i report delle Condizioni per l'utilizzo mostrano solo lo stato di consenso corrente degli utenti. Ad esempio, se un utente rifiuta e in seguito accetta, il report delle Condizioni per l'utilizzo mostrerà solo quest'ultima azione dell'utente. Se è necessario visualizzare la cronologia, è possibile usare i log di controllo di Azure AD.

**D: Se i termini delle condizioni per l'utilizzo vengono modificati, è necessario che gli utenti le accettino di nuovo?**</br>
R: Sì, se un amministratore modifica i dettagli delle condizioni per l'utilizzo, gli utenti devono accettare le nuove condizioni.

**D: È possibile aggiornare un documento esistente sulle condizioni per l'utilizzo?**</br>
R: Al momento non è possibile aggiornare un documento esistente sulle condizioni per l'utilizzo. Per modificare un documento sulle condizioni per l'utilizzo, è necessario creare una nuova istanza delle condizioni per l'utilizzo.

**D: se sono presenti collegamenti ipertestuali nel documento PDF delle Condizioni per l'utilizzo, gli utenti finali possono selezionarli?**</br>
R: viene eseguito il rendering predefinito del file PDF in formato JPEG, in modo che i collegamenti ipertestuali non siano selezionabili. Gli utenti hanno la possibilità di selezionare **In caso di problemi di visualizzazione fare clic qui** per eseguire il rendering PDF in modo nativo in cui sono supportati i collegamenti ipertestuali.

**D: le condizioni per l'utilizzo possono supportare più lingue?**</br>
A: Sì. L'amministratore può attualmente configurare 108 lingue diverse per singole condizioni per l'utilizzo.

**D: quando vengono attivate le condizioni per l'utilizzo?**</br>
R: le condizioni per l'utilizzo vengono attivate durante l'accesso.

**D: a quali altre applicazioni è possibile applicare le condizioni per l'utilizzo?**</br>
R: è possibile creare un criterio di accesso condizionale per le applicazioni aziendali utilizzando l'autenticazione moderna.  Per altre informazioni, vedere le [applicazioni aziendali](./../manage-apps/view-applications-portal.md).

**D: è possibile aggiungere più condizioni per l'utilizzo a un utente o a un'app specifici?**</br>
R: sì, mediante la creazione di più criteri di accesso condizionale destinati a tali gruppi o applicazioni. Se un utente rientra nell'ambito di più condizioni per l'utilizzo, accetta una condizione alla volta.

**D: cosa accade se un utente rifiuta le condizioni per l'utilizzo?**</br>
R: ne viene bloccato l'accesso all'applicazione. L'utente deve accedere nuovamente e accettare le condizioni.

**D: È possibile annullare l'accettazione di Condizioni per l'utilizzo accettate in precedenza?**</br>
R: È possibile [annullare l'accettazione di Condizioni per l'utilizzo accettate in precedenza](#how-users-can-review-their-terms-of-use), ma attualmente non esiste un modo per farlo.

**D: Cosa succede se si usano anche termini e condizioni di Intune?**</br>
R: Se sono stati configurati sia Condizioni per l'utilizzo di Azure AD sia [termini e condizioni di Intune](/intune/terms-and-conditions-create), l'utente dovrà accettarli entrambi. Per altre informazioni, vedere il [post di blog sulla scelta della soluzione di Condizioni di utilizzo più adatta per l'organizzazione](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

## <a name="next-steps"></a>Passaggi successivi

- [Procedure consigliate per l'accesso condizionale in Azure Active Directory](../../active-directory/conditional-access/best-practices.md)
